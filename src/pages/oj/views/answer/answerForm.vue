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
              <div slot="title">Answer</div>
  
              <div class="answer_container">
                  <el-form ref='form'  size="samll" label-position='left'>
                    <el-row :gutter='15'>
                      <el-col :span='12'>
                        <el-form-item :label="$t('m.Class_ID')" label-width="120px" prop="_class_id">
                          <div id="class_id" class="content"><p>answer ID</p></div>
                        </el-form-item>
                      </el-col>
                    </el-row>
                  </el-form>
                  <el-form ref='form'  size="samll" label-position='left'>
                    <el-row :gutter='15'>
                      <el-col :span='12'>
                        <el-form-item :label="$t('m.Problem_ID')" label-width="120px" prop="_problem_id">
                          <div id="problem_id" class="content"><p>problem ID</p></div>
                        </el-form-item>
                      </el-col>
                    </el-row>
                  </el-form>
                  <el-form ref='form'  size="samll" label-position='left'>
                    <el-row :gutter='15'>
                      <el-col :span='12'>
                        <el-form-item :label="$t('m.Title')" label-width="120px" prop="title">
                          <div id="title" class="content"><p>Title</p></div>
                        </el-form-item>
                      </el-col>
                    </el-row>
                  </el-form>
                  <el-form ref='form'  size="samll" label-position='top'>
                    <el-row :gutter='15'>
                      <el-col :span='24'>
                          <el-form-item :label="$t('m.Question_Content')" label-width="80px" prop="question_content">
                            <div id="question" class="content"><p>Question Content</p></div>
                          </el-form-item>
                      </el-col>
                    </el-row>
                  </el-form>
                  <el-form ref='form'  size="samll" label-position='top'>
                    <el-row :gutter='15'>
                      <el-col :span='24'>
                          <el-form-item :label="$t('m.Answer_Content')" label-width="80px" prop="answer_content">
                            <div id="answer" class="content"><p>Answer Content</p></div>
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
            languages: []
          }
        }
      },
      mounted () {
      },
      methods: {
        backPage () {
          this.$router.go(-1)
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
  