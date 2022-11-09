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
                  <el-form :model='answer' ref='form'  size="samll" label-position='left'>
                    <el-row :gutter='15'>
                      <el-col :span='12'>
                        <el-form-item :label="$t('m.Class_ID')" label-width="120px" prop="_class_id">
                          <div id="class_id" class="content">answer ID</div>
                        </el-form-item>
                      </el-col>
                    </el-row>
                  </el-form>
                  <el-form :model='answer' ref='form'  size="samll" label-position='left'>
                    <el-row :gutter='15'>
                      <el-col :span='12'>
                        <el-form-item :label="$t('m.Problem_ID')" label-width="120px" prop="_problem_id">
                          <div id="problem_id" class="content">problem ID</div>
                        </el-form-item>
                      </el-col>
                    </el-row>
                  </el-form>
                  <el-form :model='answer' ref='form'  size="samll" label-position='left'>
                    <el-row :gutter='15'>
                      <el-col :span='12'>
                        <el-form-item :label="$t('m.Title')" label-width="120px" prop="title">
                          <div id="title" class="content">Title</div>
                        </el-form-item>
                      </el-col>
                    </el-row>
                  </el-form>
                  <el-form :model='answer' ref='form'  size="samll" label-position='top' :rules='rules'>
                    <el-row :gutter='15'>
                      <el-col :span='24'>
                          <el-form-item :label="$t('m.Question_Content')" label-width="80px" prop="question_content">
                            <div id="question" class="content">Question Content</div>
                          </el-form-item>
                      </el-col>
                    </el-row>
                  </el-form>
                  <el-form :model='answer' ref='form'  size="samll" label-position='top' :rules='rules'>
                    <el-row :gutter='15'>
                      <el-col :span='24'>
                          <el-form-item :label="$t('m.Answer_Content')" label-width="80px" prop="answer_content">
                            <div id="question" class="content">Answer Content</div>
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
                      <cancel @click.native="showEditAnnouncementDialog = false"></cancel>
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
          rules: {
            _class_id: {required: true, message: 'Class ID is required', trigger: 'blur'},
            _problem_id: {required: true, message: 'Problem ID is required', trigger: 'blur'},
            title: {required: true, message: 'Title is required', trigger: 'blur'},
            question_content: {required: true, message: 'Question Content is required', trigger: 'blur'},
            answer_content: {required: true, message: 'Answer Content is required', trigger: 'blur'}
          },
          answer: {
            languages: []
          }
        }
      },
      mounted () {
      },
      methods: {
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
      background: #f1f2f4;
      border-radius: 4px;
      padding-left: 15px;
  
      #question{
      }
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
  