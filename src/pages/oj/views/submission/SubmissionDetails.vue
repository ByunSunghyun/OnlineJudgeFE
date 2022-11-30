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
    <Col v-if="submission.info && !isCE" :span="20">
      <Table stripe :loading="loading" :disabled-hover="true" :columns="columns" :data="submission.info.data"></Table>
    </Col>
    <Col :span="20">
      <div class="btnfooter">
          <el-button plain type="primary" @click="goRegist">{{$t('m.Question_regist')}}</el-button>
          <cancel @click.native="backPage"></cancel>
      </div>
    </Col>
    <Col :span="6">
      <Highlight :code="submission.code" :language="submission.language" :border-color="status.color"></Highlight>
    </Col>
    <Col :span="6" id="debuglist">
      <Button id="prevBtn" @click="prevStep">{{ "prev step" }}</Button>
      <Button id="nextBtn" @click="nextStep">{{ "next step" }}</Button>
      <p>{{ "max step: " }}{{ maxStep }}</p>
      <p>{{ "now step: " }}{{ nowStep }}</p>
      <table>
        <td>name</td>
        <td>value</td>  
        <template v-for="(item, index) in submitcode">
          <tr v-if="index==nowStep" v-for="(key, value) in item.var">
            <template v-if="key != isError">
              <td>{{value}}</td>
              <td>{{key}}</td>
            </template>
          </tr>
        </template>
      </table>
    </Col>
    <Col :span="6" id="visualTree">
      <button id="treeBtn" @click="getSubmission1">Show Tree</button>
      <template v-if="isTree==1">
        <template v-for="(item, index) in submitcode">
          <template v-if="index==nowStep">
            <TreeChart :json=item.treeData />
          </template>
        </template>
      </template>
    </Col>
  </Row>
  

</template>

<script>
  import api from '@oj/api'
  import {JUDGE_STATUS} from '@/utils/constants'
  import utils from '@/utils/utils'
  import Highlight from '@/pages/oj/components/Highlight'
  import TreeChart from 'vue-tree-chart'

  export default {
    name: 'submissionDetails',
    components: {
      Highlight,
      TreeChart
    },
    data () {
      return {
        isTree: 0,
        nowStep: 0,
        maxStep: 100,
        isError: 'ERROR',
        submitcode: [],
        testobj: [
          {'frame': 'main', 'next': 'scanf(\'%d\', &a);', 'step': 0, 'var': {'&a': '(int *) 0x7ffeb981cd84', 'a': 0}},
          {'frame': 'main', 'next': 'a = a + 9;', 'step': 1, 'var': {'&a': '(int *) 0x7ffeb981cd84', 'a': 0}},
          {'frame': 'main', 'next': 'printf(\'%d\', a);', 'step': 2, 'var': {'&a': '(int *) 0x7ffeb981cd84', 'a': 9}},
          {'frame': 'main', 'next': 'printf(\'\\n\');', 'step': 3, 'var': {'&a': '(int *) 0x7ffeb981cd84', 'a': 9}},
          {'frame': 'main', 'next': 'return (0);', 'output': ['9'], 'step': 4, 'var': {'&a': '(int *) 0x7ffeb981cd84', 'a': 9}},
          {'frame': 'main', 'next': '}', 'step': 5, 'var': {'&a': '(int *) 0x7ffeb981cd84', 'a': 9}}
        ],
        test1: 'test1',
        test2: false,
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
        isConcat: false,
        loading: false
      }
    },
    mounted () {
      this.getSubmission()
    },
    methods: {
      goRegist () {
        this.$router.push({
          name: 'questionregister', params: {submitID: this.submission.id}
        })
      },
      backPage () {
        this.$router.go(-1)
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
          this.getVisual(data.id, data.problem, data.user_id)
        }, () => {
          this.loading = false
        })
      },
      getSubmission1 () {
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
          this.getTree(data.id, data.problem, data.user_id)
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
      },
      prevStep () {
        if (this.nowStep <= 0) {
          this.nowStep = 0
        } else {
          this.nowStep = this.nowStep - 1
        }
      },
      nextStep () {
        if (this.maxStep === this.nowStep) {
          this.nowStep = this.items.length
        } else {
          this.nowStep = this.nowStep + 1
        }
      },
      getVisual (a, b, c) {
        let params = {
          submission_id: a,
          problem_id: b,
          user_id: c,
          dataType: 0
        }
        this.test1 = params.submission_id
        api.getVisual(params).then(res => {
          this.test2 = true
          this.submitcode = res.data.data
          this.submitcode = JSON.parse(this.submitcode)
          this.maxStep = this.submitcode.length - 1
        })
      },
      getTree (a, b, c) {
        this.isTree = 1
        let params = {
          submission_id: a,
          problem_id: b,
          user_id: c,
          dataType: 1
        }
        api.getVisual(params).then(res => {
          this.test2 = true
          this.submitcode = res.data.data
          this.submitcode = JSON.parse(this.submitcode)
          this.maxStep = this.submitcode.length - 1
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
  .btnfooter {
    // display: inline-block;
    margin: 10px 5px 10px 5px;
    float: right;
  }
  #debuglist {
    padding: 10px;
    background-color: white;
    font-size: 17px;
    //border: 1px solid rgb(0, 0, 0); 
  }
  #visualTree {
    padding: 10px;
    background-color: white;
  }
  #treeBtn {
    padding: 10px 10px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 20px;
    margin: 4px 2px;
    border-radius: 5%;
  }
  #prevBtn {
    padding: 10px 10px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 20px;
    margin: 4px 2px;
    border-radius: 5%;
  }
  #nextBtn {
    padding: 10px 10px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 20px;
    margin: 4px 2px;
    border-radius: 5%;
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
</style>
