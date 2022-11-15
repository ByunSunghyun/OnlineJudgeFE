<template>
    <div class="flex-container">
      <div id="answer-main">
        <!--
            <div class="flex-container" v-if="route_name === 'answer-details'">
      -->
        <div class="flex-container">
          <div id="answer-desc">
            <Panel :padding="20" shadow>
              <!--
                  <div slot="title">
                    {{answer.title}}
                  </div>
              -->
              <div slot="title">Answer ({{this.answer.id}}) [Question : {{this.answer.question_id}}]</div>
  
              <div class="answer_container">
                  <el-form ref='form'  size="samll" label-position='left'>
                    <el-row :gutter='15'>
                      <el-col :span='12'>
                        <el-form-item :label="$t('m.Class_ID')" label-width="120px" prop="class_id">
                          <div id="class_id" class="content"><p>{{this.answer.class_id}}</p></div>
                        </el-form-item>
                      </el-col>
                    </el-row>
                  </el-form>
                  <el-form ref='form'  size="samll" label-position='left'>
                    <el-row :gutter='15'>
                      <el-col :span='12'>
                        <el-form-item :label="$t('m.Problem_ID')" label-width="120px" prop="problem_id">
                          <div id="problem_id" class="content"><p>{{this.answer.problem_id}}</p></div>
                        </el-form-item>
                      </el-col>
                    </el-row>
                  </el-form>
                  <el-form ref='form'  size="samll" label-position='left'>
                    <el-row :gutter='15'>
                      <el-col :span='12'>
                        <el-form-item :label="$t('m.Submission_ID')" label-width="120px" prop="submission_id">
                          <div id="submission_id" class="content"><p>{{this.answer.submisssion_id}}</p></div>
                        </el-form-item>
                      </el-col>
                    </el-row>
                  </el-form>
                  <el-form ref='form'  size="samll" label-position='top'>
                    <el-row :gutter='15'>
                      <el-col :span='24'>
                          <el-form-item :label="$t('m.Question_Content')" label-width="80px" prop="question_content">
                            <div id="question" class="content"><p>{{this.answer.question_content}}</p></div>
                          </el-form-item>
                      </el-col>
                    </el-row>
                  </el-form>
                  <div>
                    
                  </div>
                  <el-form ref='form'  size="samll" label-position='top'>
                    <el-row :gutter='15'>
                      <el-col :span='24'>
                          <el-form-item :label="$t('m.Answer_Content')" label-width="80px" prop="answer_content">
                            <div id="answer" class="content"><p>{{this.answer.content}}</p></div>
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
                      <cancel @click.native="backPage"></cancel>
                  </div>
              </div>
              
          </Panel>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
    import api from '@oj/api'
    export default {
      name: 'AnswerDetail',
      components: {},
      data () {
        return {
          answer: {
            id: '',
            class_id: '',
            problem_id: '',
            submisssion_id: '',
            question_id: '',
            question_content: '',
            content: ''
          },
          loading: false
        }
      },
      mounted () {
        this.getAnswer()
      },
      methods: {
        init () {
          this.getAnswer()
        },
        backPage () {
          this.$router.go(-1)
        },
        getAnswer () {
          this.loading = true
          api.getAnswer(this.$route.params.id).then(res => {
            this.loading = true
            let data = res.data.data
            this.answer = data
          }, () => {
            this.loading = false
          })
        }
      }
    }
  </script>
  
  <style scoped lang="less">
    .flex-container {
      #answer-main {
        flex: 1 1;
        width: 0;
        #answer-desc {
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
      display: inline-block;
      margin: 0 5px;
      float: right;
    }
    .answer_container{
      padding-left: 20px;
      padding-right: 20px;
      margin-bottom: 50px;
    }
  </style>
  