<template>
    <panel>
      <div slot="title">
          {{$t('m.Answer_regist')}}
      </div>
      
      <div class = "answer_register">
        <el-form :model='answer' ref='form'  size="samll" label-position='left' :rules='rules'>
          <el-row :gutter='15'>
            <el-col :span='12'>
              <el-form-item :label="$t('m.Class_ID')" label-width="120px" prop="_class_id">
                <el-input :placeholder="$t('m.Class_ID')" v-model="answer._class_id"></el-input>
              </el-form-item>
            </el-col>
          </el-row>
        </el-form>
        <el-form :model='answer' ref='form'  size="samll" label-position='left' :rules='rules'>
          <el-row :gutter='15'>
            <el-col :span='12'>
              <el-form-item :label="$t('m.Problem_ID')" label-width="120px" prop="_problem_id">
                <el-input :placeholder="$t('m.Problem_ID')" v-model="answer._problem_id"></el-input>
              </el-form-item>
            </el-col>
          </el-row>
        </el-form>
        <el-form :model='answer' ref='form'  size="samll" label-position='left' :rules='rules'>
          <el-row :gutter='15'>
            <el-col :span='24'>
              <el-form-item :label="$t('m.Title')" label-width="80px" prop="title">
                <el-input :placeholder="$t('m.Title')" v-model="answer.title"></el-input>
              </el-form-item>
            </el-col>
          </el-row>
        </el-form>
        <el-form :model='answer' ref='form'  size="samll" label-position='top' :rules='rules'>
          <el-row :gutter='15'>
            <el-col :span='24'>
              <el-form-item :label="$t('m.question_content')" label-width="80px" prop="question_content">
                <el-button plain type="primary" :span='8' @click="goStatus">{{$t('m.Go_Code')}}</el-button>
                <div class="question">Question Code</div>
                <!--
                  <Simditor v-model="answer.question_content"></Simditor>
                -->
              </el-form-item>
            </el-col>
          </el-row>
        </el-form>
        <el-form :model='answer' ref='form'  size="samll" label-position='top' :rules='rules'>
          <el-row :gutter='15'>
            <el-col :span='24'>
              <el-form-item :label="$t('m.input_description')" label-width="80px" prop="input_description">
                <!--
                <el-input :placeholder="$t('m.input_description')" v-model="answer.input_description"></el-input>
                -->
                <Simditor v-model="answer.input_description"></Simditor>
              </el-form-item>
            </el-col>
          </el-row>
        </el-form>
        <span slot="footer" class="dialog-footer">
          <save type="primary" @click.native="submitAnnouncement"></save>
          <cancel @click.native="showEditAnnouncementDialog = false"></cancel>
        </span>
          <!--
            <el-row>
              <el-col :span="12">
                <div class="grid-content bg-puple-dark">Element-ui</div>
              </el-col>
            </el-row>
          
        -->
      </div>
    </panel>
  </template>
  
  <script>
    import Simditor from '../../components/Simditor'
    import api from '../../api'
    export default {
      name: 'AnswerRegister',
      components: {
        Simditor
      },
      data () {
        return {
          rules: {
            _class_id: {required: true, message: 'Class ID is required', trigger: 'blur'},
            _problem_id: {required: true, message: 'Problem ID is required', trigger: 'blur'},
            title: {required: true, message: 'Title is required', trigger: 'blur'},
            question_content: {required: true, message: 'Question_Content is required', trigger: 'blur'},
            input_description: {required: true, message: 'Input Description is required', trigger: 'blur'}
          },
          answer: {
            languages: [],
            io_mode: {'io_mode': 'Standard IO', 'input': 'input.txt', 'output': 'output.txt'}
          },
          routeName: ''
        }
      },
      mounted () {
        this.init()
      },
      methods: {
        goStatus () {
          this.$router.push({
            name: 'questionRegister'
          })
        },
        init () {
          this.routeName = this.$route.name
        }
      }
    }
  </script>
  
  <style lang="less" scoped>
    .content {
      font-size: 16px;
      margin: 0 50px 40px 50px;
      > ul {
        list-style: disc;
        li {
          font-size: 16px;
          margin-top: 20px;
          &:first-child {
            margin-top: 0;
          }
          p {
            font-size: 14px;
            margin-top: 5px;
          }
        }
      }
    }
    .answer_register{
      padding-left: 50px;
      padding-right: 50px;
      margin-bottom: 50px;
    }
    .el-row {
        margin-bottom: 50px;
        &:last-child {
          margin-bottom: 0;
        }
    }
    .el-col {
        border-radius: 4px;
    }
    .question{
      margin-top: 15px;
      padding: 10px;
      background: #f1f2f4;
    }
    .dialog-footer{
      float: right;
    }
  </style>
  