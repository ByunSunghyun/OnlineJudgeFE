<template>
  <Row type="flex" justify="space-around">
    <Col :span="20" id="status">
      <Alert :type="status.type" showIcon>
        <span class="title">{{$t('m.' + status.statusName.replace(/ /g, "_"))}}</span>
        <div slot="desc" class="content">
          <template v-if="isCE">
            <pre>{{submission.statistic_info.err_info}}</pre>
          </template>
          <template v-else>
            <span>{{$t('m.Time')}}: {{submission.statistic_info.time_cost | submissionTime}}</span>
            <span>{{$t('m.Memory')}}: {{submission.statistic_info.memory_cost | submissionMemory}}</span>
            <span>{{$t('m.Lang')}}: {{submission.language}}</span>
            <span>{{$t('m.Author')}}: {{submission.username}}</span>
          </template>
        </div>
      </Alert>
    </Col>

    <!-- 백그라운드로 돌아가면 나타납니다. 권한 제어 백그라운드로 이동 -->
    <Col :span="20" id = "button">
      <div class="btnfooter">
        <el-button plain type="primary" @click="goRegist">{{$t('m.Question_regist')}}</el-button>
        <cancel @click.native="backPage"></cancel>
      </div>
    </Col>
    <Col v-if="submission.info && !isCE" :span="6">
      <Table stripe :loading="loading" :disabled-hover="true" :columns="columns" :data="submission.info.data"></Table>
    </Col>

    <Col :span="6">
      <Highlight :code="submission.code" :language="submission.language" :border-color="status.color"></Highlight>
    </Col>
    <Col :span="6" id="debuglist">
      <Button @click="prevStep">{{ "prev_step" }}</Button>
      <Button @click="nextStep">{{ "next_step" }}</Button>
      <p>{{ "max step: " }}{{ maxStep }}</p>
      <p>{{ "now step: " }}{{ nowStep }}</p>
      <table>
        <td>name</td>
        <td>value</td>
        <td>type</td>
        <td>Check</td>
        <tr v-if="index < nowStep" v-for="(item, index) in items" :key="items.name">
          <td><span v-if="index < nowStep" v-html="item.name"></span></td>
          <td><span v-if="index < nowStep" v-html="item.value"></span></td>
          <td><span v-if="index < nowStep" v-html="index"></span></td>
          <td><span v-if="index < nowStep" v-html="index"></span></td>
        </tr>
      </table>
    </Col>
    <Col :span="6">
      <p>visualization 구현부분</p>
      <Highlight :code="submission.code" :language="submission.language" :border-color="status.color"></Highlight>
    </Col>
  </Row>
  
    <!-- <Col v-if="submission.can_unshare" :span="20">
      <div id="share-btn">
        <Button v-if="submission.shared"
                type="warning" size="large" @click="shareSubmission(false)">
          {{$t('m.UnShare')}}
        </Button>
        <Button v-else
                type="primary" size="large" @click="shareSubmission(true)">
          {{$t('m.Share')}}
        </Button>
      </div>
    </Col>-->
</template>

<script>
  import api from '@oj/api'
  import {JUDGE_STATUS} from '@/utils/constants'
  import utils from '@/utils/utils'
  import Highlight from '@/pages/oj/components/Highlight'

  export default {
    name: 'submissionDetails',
    components: {
      Highlight
    },
    data () {
      return {
        nowStep: 0,
        maxStep: 0,
        items: [
          {
            'name': 'a',
            'value': '123'
          },
          {
            'name': 'b',
            'value': '234'
          },
          {
            'name': 'c',
            'value': '123'
          },
          {
            'name': 'd',
            'value': '456'
          }
        ],
        columns: [
          {
            title: this.$i18n.t('m.ID'),
            align: 'center',
            type: 'index'
          },
          {
            title: this.$i18n.t('m.Status'),
            align: 'center',
            render: (h, params) => {
              return h('Tag', {
                props: {
                  color: JUDGE_STATUS[params.row.result].color
                }
              }, this.$i18n.t('m.' + JUDGE_STATUS[params.row.result].name.replace(/ /g, '_')))
            }
          },
          {
            title: this.$i18n.t('m.Memory'),
            align: 'center',
            render: (h, params) => {
              return h('span', utils.submissionMemoryFormat(params.row.memory))
            }
          },
          {
            title: this.$i18n.t('m.Time'),
            align: 'center',
            render: (h, params) => {
              return h('span', utils.submissionTimeFormat(params.row.cpu_time))
            }
          }
        ],
        submission: {
          result: '0',
          code: '',
          info: {
            data: []
          },
          statistic_info: {
            time_cost: '',
            memory_cost: ''
          }
        },
        submission1: {
          'data': {
            'id': 'a1ae53b0b238a89d46df1648c25456e3',
            'problem': '1',
            'create_time': '2022-11-08T05:04:40.983545Z',
            'user_id': 9137,
            'username': '12345654',
            'code': '#include <stdio.h>    \nint main(){\n    int a, b;\n    scanf(\\"%d%d\\", &a, &b);\n    printf(\\"%d\\n\\", a+b);\n    return 0;\n}',
            'result': 0,
            'language': 'C',
            'shared': false,
            'statistic_info': {
              'time_cost': 1,
              'memory_cost': 1736704
            }
          }
        },
        isConcat: false,
        loading: false
      }
    },
    mounted () {
      this.getSubmission1()
      this.init()
    },
    methods: {
      goRegist () {
        this.$router.push({
          name: 'questionregister'
        })
      },
      backPage () {
        this.$router.go(-1)
      },
      init () {
        this.maxStep = this.items.length
      },
      prevStep () {
        if (this.nowStep <= 0) {
          this.nowStep = 0
        } else {
          this.nowStep = this.nowStep - 1
        }
      },
      nextStep () {
        if (this.items.length === this.nowStep) {
          this.nowStep = this.items.length
        } else {
          this.nowStep = this.nowStep + 1
        }
      },
      getSubmission1 () {
        this.submission = this.submission1.data
      },
      getSubmission () {
        this.loading = true
        api.getSubmission(this.$route.params.id).then(res => {
          this.loading = false
          let data = res.data.data
          if (data.info && data.info.data && !this.isConcat) {
            // score exist means the submission is OI problem submission
            if (data.info.data[0].score !== undefined) {
              this.isConcat = true
              const scoreColumn = {
                title: this.$i18n.t('m.Score'),
                align: 'center',
                key: 'score'
              }
              this.columns.push(scoreColumn)
              this.loadingTable = false
            }
            if (this.isAdminRole) {
              this.isConcat = true
              const adminColumn = [
                {
                  title: this.$i18n.t('m.Real_Time'),
                  align: 'center',
                  render: (h, params) => {
                    return h('span', utils.submissionTimeFormat(params.row.real_time))
                  }
                },
                {
                  title: this.$i18n.t('m.Signal'),
                  align: 'center',
                  key: 'signal'
                }
              ]
              this.columns = this.columns.concat(adminColumn)
            }
          }
          this.submission = data
        }, () => {
          this.loading = false
        })
      },
      shareSubmission (shared) {
        let data = {id: this.submission.id, shared: shared}
        api.updateSubmission(data).then(res => {
          this.getSubmission()
          this.$success(this.$i18n.t('m.Succeeded'))
        }, () => {
        })
      }
    },
    computed: {
      status () {
        return {
          type: JUDGE_STATUS[this.submission.result].type,
          statusName: JUDGE_STATUS[this.submission.result].name,
          color: JUDGE_STATUS[this.submission.result].color
        }
      },
      isCE () {
        return this.submission.result === -2
      },
      isAdminRole () {
        return this.$store.getters.isAdminRole
      }
    }
  }
</script>

<style scoped lang="less">
table {
    width: 100%;
    border: 1px solid #444444;
    border-collapse: collapse;
  }
  th, td {
    border: 1px solid #444444;
    padding: 10px;
  }
  #debuglist {
    padding: 10px;
    background-color: white;
    //border: 1px solid rgb(0, 0, 0); 
  }
  #status {
    .title {
      font-size: 20px;
    }
    .content {
      margin-top: 10px;
      font-size: 14px;
      span {
        margin-right: 10px;
      }
      pre {
        white-space: pre-wrap;
        word-wrap: break-word;
        word-break: break-all;
      }
    }
  }

  .admin-info {
    margin: 5px 0;
    &-content {
      font-size: 16px;
      padding: 10px;
    }
  }

  #share-btn {
    float: right;
    margin-top: 5px;
    margin-right: 10px;
  }

  pre {
    border: none;
    background: none;
  }
  .btnfooter {
    // display: inline-block;
    margin: 10px 5px 10px 5px;
    float: right;
  }
</style>
