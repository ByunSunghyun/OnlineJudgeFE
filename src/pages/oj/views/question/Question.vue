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

      <div>
        <Button type="button" @click="goRegist" style="margin: 10px">{{$t('m.Question_regist')}}</Button>
        <Button type="button" @click="goDetail" style="margin: 10px">DetailPage</Button>
        <Button type="button" @click="goAnswer" style="margin: 10px">답변 등록</Button>
        <Button type="button" @click="goAnswerDet" style="margin: 10px">Detail_Answer</Button>
      </div>
  </panel>
  
</template>

<script>
  import { mapGetters } from 'vuex'
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
        currentPage: 1,
        pageSize: 10,
        keyword: '',
        QuestionTableColumns: [
          {
            title: this.$i18n.t('m.Class'),
            align: 'center',
            width: 300,
            render: (h, params) => {
              return h('Button', {
                props: {
                  type: 'text',
                  size: 'large'
                },
                on: {
                  click: () => {
                    this.$router.push({name: 'contest-details', params: {questionID: params.row.class_id}})
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
            width: 300,
            render: (h, params) => {
              return h('Button', {
                props: {
                  type: 'text',
                  size: 'large'
                },
                on: {
                  click: () => {
                    this.$router.push({name: 'problem-details', params: {questionID: params.row.problem_id}})
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
            width: 300,
            render: (h, params) => {
              return h('Button', {
                props: {
                  type: 'text',
                  size: 'large'
                },
                on: {
                  click: () => {
                    this.$router.push({name: 'questionDetail', params: {questionID: params.row.id}})
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
            render: (h, params) => {
              return h('Button', {
                props: {
                  type: 'text',
                  size: 'large'
                },
                on: {
                  click: () => {
                    this.$router.push({name: 'questionDetail', params: {questionID: params.row.id}})
                  }
                },
                style: {
                  textAlign: 'center'
                }
              }, this.printAnswer(params.row.answer))
            }
          }
        ],
        loadings: true,
        routeName: '',
        total: 0,
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
        listVisible: true
      }
    },
    mounted () {
      this.getQuestionList(this.currentPage)
      // this.getQuestionList1()
    },
    methods: {
      printAnswer (answer) {
        if (answer === '') return 'No Answer'
        else return answer
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
      getQuestionList (page) {
        this.loadings = true
        api.getQuestionList((page - 1) * this.pageSize, this.pageSize, this.keyword).then(res => {
          this.loadings = false
          this.total = res.data.data.total
          this.questionList = res.data.data.results
        }, res => {
          this.loadings.table = false
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