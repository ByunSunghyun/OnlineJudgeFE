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
    name: 'Announcement',
    components: {
      Pagination
    },
    data () {
      return {
        limit: 10,
        total: 10,
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
                style: {
                  textAlign: 'center'
                }
              }, params.row.class)
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
                style: {
                  textAlign: 'center'
                }
              }, params.row.problem)
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
                style: {
                  textAlign: 'center'
                }
              }, params.row.answer)
            }
          }
        ],
        loadings: [],
        routeName: '',
        query: {
          keyword: '',
          difficulty: '',
          tag: '',
          page: 1,
          limit: 10
        },
        questionList: [],
        listVisible: true
      }
    },
    mounted () {
      this.init()
    },
    methods: {
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
          name: 'answerDetails'
        })
      },
      init () {
        this.routeName = this.$route.name
        let query = this.$route.query
        this.query.difficulty = query.difficulty || ''
        this.query.keyword = query.keyword || ''
        this.query.tag = query.tag || ''
        this.query.page = parseInt(query.page) || 1
        if (this.query.page < 1) {
          this.query.page = 1
        }
        this.query.limit = parseInt(query.limit) || 10
        this.getQuestionList()
      },
      pushRouter () {
        this.$router.push({
          name: '/question',
          query: utils.filterEmptyValue(this.query)
        })
      },
      getQuestionList () {
        // getProblemList from ProblemList.vue
        let offset = (this.query.page - 1) * this.query.limit
        this.loadings.table = true
        api.getQuestionList(offset, this.limit, this.query).then(res => {
          this.loadings.table = false
          this.total = res.data.data.total
          this.questionList = res.data.data.results
          if (this.isAuthenticated) {
            this.addStatusColumn(this.QuestionTableColumns, res.data.data.results)
          }
        }, res => {
          this.loadings.table = false
        })
      },
      // announcement.vue
      goBack () {
        this.listVisible = true
        this.announcement = ''
      }
    },
    computed: {
      ...mapGetters(['isAuthenticated'])
    },
    watch: {
      '$route' (newVal, oldVal) {
        if (newVal !== oldVal) {
          this.init(true)
        }
      },
      'isAuthenticated' (newVal) {
        if (newVal === true) {
          this.init()
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