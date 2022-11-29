<template>
  <Panel shadow :padding="10">
      <div slot="title">
        {{$t('m.My_question')}}
        <i class="fa-solid fa-circle-question"></i>
      </div>
      <div slot="extra">
        <Button v-if="listVisible" type="info" @click="init" :loading="btnLoading">{{$t('m.Refresh')}}</Button>
        <Button v-else type="ghost" icon="ios-undo" @click="goBack">{{$t('m.Back')}}</Button>
      </div>

      <Table estyle="width: 100%; font-size: 16px;" 
        :columns="QuestionTableColumns"
        :data="questionList"
        disabled-hover> </Table>

        <!--
        <div>
          <Button type="button" @click="goRegist" style="margin: 10px">{{$t('m.Question_regist')}}</Button>
          <Button type="button" @click="goDetail" style="margin: 10px">DetailPage</Button>
          <Button type="button" @click="goAnswer" style="margin: 10px">답변 등록</Button>
          <Button type="button" @click="goAnswerDet" style="margin: 10px">Detail_Answer</Button>
        </div>
        -->

      <!--
        <p>[{{this.total}}]</p>
        <p>[{{this.loading}}]</p>
        <p>[{{this.name}}]</p>
      -->
  </panel>
  
</template>

<script>
  import api from '@oj/api'
  import utils from '@/utils/utils'
  import Pagination from '@oj/components/Pagination'
  export default {
    name: 'Question',
    components: {
      Pagination
    },
    data () {
      return {
        QuestionTableColumns: [
          {
            title: this.$i18n.t('m.Class'),
            align: 'center',
            width: '10%',
            render: (h, params) => {
              return h('Button', {
                props: {
                  type: 'text',
                  size: 'large'
                },
                on: {
                  click: () => {
                    this.$router.push({name: 'contest-details', params: {contestID: params.row.class_id}})
                  }
                },
                style: {
                  textAlign: 'center'
                }
              }, params.row.class_id)
            }
          },
          {
            title: this.$i18n.t('m.Problem'),
            align: 'center',
            width: '10%',
            render: (h, params) => {
              return h('Button', {
                props: {
                  type: 'text',
                  size: 'large'
                },
                on: {
                  click: () => {
                    this.$router.push({name: 'questionDetail', params: {questionID: params.row.question_id}})
                  }
                },
                style: {
                  textAlign: 'center'
                }
              }, params.row.problem_id)
            }
          },
          {
            title: this.$i18n.t('m.Title'),
            align: 'center',
            width: '30%',
            render: (h, params) => {
              return h('Button', {
                props: {
                  type: 'text',
                  size: 'large'
                },
                on: {
                  click: () => {
                    this.$router.push({name: 'questionDetail', params: {questionID: params.row.question_id}})
                  }
                },
                style: {
                  textAlign: 'center'
                }
              }, params.row.title)
            }
          },
          {
            title: this.$i18n.t('m.Answer'),
            align: 'center',
            width: '10%',
            render: (h, params) => {
              return h('Button', {
                props: {
                  type: 'text',
                  size: 'large'
                },
                on: {
                  click: () => {
                    this.$router.push({name: 'questionDetail', params: {questionID: params.row.question_id}})
                  }
                },
                style: {
                  textAlign: 'center'
                }
              }, this.printAnswer(params.row.answer_id))
            }
          }
        ],
        loading: false,
        routeName: '',
        total: 0,
        limit: 20,
        page: 1,
        questionList: [],
        questionList1: [
          {
            'id': '12',
            'class_id': '123',
            'problem_id': '456',
            'title': 'question',
            'answer': ''
          },
          {
            'id': '45',
            'class_id': '456',
            'problem_id': '12',
            'title': 'question2',
            'answer': '12'
          }
        ],
        listVisible: true,
        username: '',
        name: '',
        profile: {},
        hasAnswer: false
      }
    },
    mounted () {
      this.username = this.$route.query.username
      api.getUserInfo(this.username).then(res => {
        this.profile = res.data.data
        this.name = res.data.data.user.username
        this.getQuestionList()
      })
      // this.getQuestionList1()
    },
    methods: {
      printAnswer (answer) {
        if (answer === null) {
          this.hasAnswer = true
          return 'Waiting'
        } else {
          this.hasAnswer = false
          return 'Complete'
        }
      },
      goRegist () {
        this.$router.push({
          name: 'questionregister'
        })
      },
      goDetail () {
        this.$router.push({
          name: 'questionDetail'
        })
      },
      goAnswer () {
        this.$router.push({
          name: 'answerRegister'
        })
      },
      goAnswerDet () {
        this.$router.push({
          name: 'answerDetail'
        })
      },
      init () {
        this.routeName = this.$route.name
        this.getQuestionList()
      },
      pushRouter () {
        this.$router.push({
          name: '/question',
          query: utils.filterEmptyValue(this.query)
        })
      },
      buildQuery () {
        return {
          username: this.name,
          page: this.page
        }
      },
      getQuestionList () {
        // this.loading = true
        // let params = {}
        // let offset = (this.page - 1) * this.limit
        api.getQuestionList(this.name).then(res => {
          this.loading = true
          this.total = res.data.data.total
          this.questionList = res.data.data
        }, res => {
          this.loading = false
        })
      },
      getQuestionList1 () {
        this.questionList = this.questionList1
      },
      // announcement.vue
      goBack () {
        this.listVisible = true
        this.announcement = ''
      }
    },
    computed: {
    },
    watch: {
      '$route' (newVal, oldVal) {
        if (newVal !== oldVal) {
          this.init(true)
        }
      }
    }
  }
</script>

<style scoped lang="less">
  .announcements-container {
    margin-top: -10px;
    margin-bottom: 10px;
    li {
      padding-top: 15px;
      list-style: none;
      padding-bottom: 15px;
      margin-left: 20px;
      font-size: 16px;
      border-bottom: 1px solid rgba(187, 187, 187, 0.5);
      &:last-child {
        border-bottom: none;
      }
      .flex-container {
        .title {
          flex: 1 1;
          text-align: left;
          padding-left: 10px;
          a.entry {
            color: #495060;
            &:hover {
              color: #2d8cf0;
              border-bottom: 1px solid #2d8cf0;
            }
          }
        }
        .creator {
          flex: none;
          width: 200px;
          text-align: center;
        }
        .date {
          flex: none;
          width: 200px;
          text-align: center;
        }
      }
    }
  }
  .content-container {
    padding: 0 20px 20px 20px;
  }
  .no-announcement {
    text-align: center;
    font-size: 16px;
  }changeLocale
  .announcement-animate-enter-active {
    animation: fadeIn 1s;
  }
</style>