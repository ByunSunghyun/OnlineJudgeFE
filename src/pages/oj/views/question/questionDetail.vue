<template>
  <div class="flex-container">
    <div id="question-main">
      <!--
          <div class="flex-container" v-if="route_name === 'question-details'">
    -->
      <div class="flex-container">
        <div id="question-desc">
          <Panel :padding="20" shadow>
            <!--
                <div slot="title">
                  {{question.title}}
                </div>
            -->
            <div slot="title">Question [{{question.id}}]</div>

            <div class="question_container">
                <el-form ref='form'  size="samll" label-position='left'>
                  <el-row :gutter='15'>
                    <el-col :span='12'>
                      <el-form-item :label="$t('m.Class_ID')" label-width="120px" prop="class_id">
                        <div id="class_id" class="content"><p>{{question.class_id}}</p></div>
                      </el-form-item>
                    </el-col>
                  </el-row>
                </el-form>
                <el-form ref='form'  size="samll" label-position='left'>
                  <el-row :gutter='15'>
                    <el-col :span='12'>
                      <el-form-item :label="$t('m.Problem_ID')" label-width="120px" prop="problem_id">
                        <div id="problem_id" class="content"><p>{{question.problem_id}}</p></div>
                      </el-form-item>
                    </el-col>
                  </el-row>
                </el-form>
                <el-form ref='form'  size="samll" label-position='left'>
                  <el-row :gutter='15'>
                    <el-col :span='12'>
                      <el-form-item :label="$t('m.Submission_ID')" label-width="120px" prop="submisssion_id">
                        <div id="submission_id" class="content"><p>{{question.submission_id}}</p></div>
                      </el-form-item>
                    </el-col>
                  </el-row>
                </el-form>
                <el-form ref='form'  size="samll" label-position='left'>
                  <el-row :gutter='15'>
                    <el-col :span='12'>
                      <el-form-item :label="$t('m.Title')" label-width="120px" prop="title">
                        <div id="title" class="content"><p>{{question.title}}</p></div>
                      </el-form-item>
                    </el-col>
                  </el-row>
                </el-form>
                <el-form ref='form'  size="samll" label-position='top' :rules='rules'>
                  <el-row :gutter='15'>
                    <el-col :span='24'>
                        <el-form-item :label="$t('m.Question_Content')" label-width="80px" prop="question_content">
                          <div id="question" class="content">{{deleteTag(question.content)}}</div>
                        </el-form-item>
                    </el-col>
                  </el-row>
                </el-form>
                <el-form v-if="this.hasAnswer" ref='form'  size="samll" label-position='top'>
                  <el-row :gutter='15'>
                    <el-col :span='24'>
                        <el-form-item :label="$t('m.Answer_Content')" label-width="80px" prop="answer_content">
                          <div id="answer" class="content"><p>{{deleteTag(answer.content)}}</p></div>
                        </el-form-item>
                    </el-col>
                  </el-row>
                </el-form>
                <!---
                    <div v-html="question.description" class="markdown-body"></div>
                    <div v-if="passwordFormVisible" class="question-password">
                        <Input v-model="questionPassword" type="password"
                        placeholder="question password" class="question-password-input"
                        @on-enter="checkPassword"/>
                        <Button type="info" @click="checkPassword">Enter</Button>
                    </div>
                -->
                <div class="btnfooter">
                  <el-button v-if="this.hasAnswer" plain type="primary" @click="goAnswer">답변 수정</el-button>
                  <el-button v-else plain type="primary" @click="goAnswer">답변 등록</el-button>
                  <cancel @click.native="backPage"></cancel>
                </div>
            </div>
            <!--
              <p>[{{this.$route.params.questionID}}]</p>
              <p>[{{this.question.answer_id}}]</p>
            -->
        </Panel>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import api from '@oj/api'
  export default {
    name: 'QuestionDetail',
    components: {},
    data () {
      return {
        question: {
          id: '',
          class_id: '',
          problem_id: '',
          submission_id: '',
          answer_id: '',
          title: '',
          content: ''
        },
        answer: {
          id: '',
          submisssion_id: '',
          question_id: '',
          content: ''
        },
        loading: false,
        hasAnswer: false,
        check: ''
      }
    },
    mounted () {
      /*
      this.$router.push({name: 'questionDetail', params: {questionID: 'id_value'}})로 in
      */
      this.init()
      // this.getQuestion()
      // if (this.question.answer_id === '') this.hasAnswer = false
      // else this.hasAnswer = true
      // this.hasAnswer = true
      // if (this.hasAnswer) this.getAnswer()
    },
    methods: {
      init () {
        this.getQuestion()
        // if (this.question.answer_id === '') this.hasAnswer = false
        // else this.hasAnswer = true
        // this.hasAnswer = true
        // if (this.hasAnswer) this.getAnswer()
      },
      deleteTag (str) {
        let len = str.length
        return str.substr(3, len - 7)
      },
      backPage () {
        this.$router.push({name: 'question'})
      },
      goAnswer () {
        this.$router.push({name: 'answerRegister', params: {questionID: this.question.id}})
      },
      getQuestion () {
        this.loading = true
        api.getQuestion(this.$route.params.questionID).then(res => {
          this.loading = true
          let data = res.data.data
          this.question = data
          //
          if (this.question.answer_id === null) this.hasAnswer = false
          else {
            this.hasAnswer = true
            //
            this.getAnswer()
          }
        }, () => {
          this.loading = false
        })
      },
      getAnswer () {
        this.loading = true
        api.getAnswer(this.question.answer_id).then(res => {
          this.loading = true
          let data = res.data.data
          this.answer = data
        }, () => {
          this.loading = false
        })
        if (this.hasAnswer) {
        }
      }
    }
  }
</script>

<style scoped lang="less">
  .flex-container {
    #question-main {
      flex: 1 1;
      width: 0;
      #question-desc {
        flex: auto;
      }
    }
  }
  .content{
    height: 40px;
    padding-left: 15px;
    padding-right: 10px;
    border-radius: 5px;
    background: #f1f2f4;
  }
  .btnfooter {
    // display: inline-block;
    margin: 0 5px;
    float: right;
  }
  .question_container{
    padding-left: 20px;
    padding-right: 20px;
    margin-bottom: 50px;
  }
</style>