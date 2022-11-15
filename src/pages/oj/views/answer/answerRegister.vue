<template>
    <panel>
      <div slot="title">
          {{$t('m.Answer_regist')}}
      </div>
      
      <div class = "answer_register">
        <el-form :model='answer' ref='form'  size="samll" label-position='left'>
          <el-row :gutter='15'>
            <el-col :span='12'>
              <el-form-item :label="$t('m.Class_ID')" label-width="120px" prop="class_id">
                <!--
                  <el-input :placeholder="$t('m.Class_ID')" v-model="class_id"></el-input>
                -->
                <div class="output"><p>{{this.answer.class_id}}</p></div>
              </el-form-item>
            </el-col>
          </el-row>
        </el-form>
        <el-form :model='answer' ref='form'  size="samll" label-position='left'>
          <el-row :gutter='15'>
            <el-col :span='12'>
              <el-form-item :label="$t('m.Problem_ID')" label-width="120px" prop="problem_id">
                <!--
                  <el-input :placeholder="$t('m.Problem_ID')" v-model="problem_id"></el-input>
                -->
                <div class="output"><p>{{this.answer.problem_id}}</p></div>
              </el-form-item>
            </el-col>
          </el-row>
        </el-form>
        <el-form ref='form'  size="samll" label-position='left'>
        <el-row :gutter='15'>
          <el-col :span='12'>
            <el-form-item :label="$t('m.Submission_ID')" label-width="120px" prop="submission_id">
              <!--
                <el-input :placeholder="$t('m.Submission_ID')" v-model="submission_id"></el-input>
              -->
              <div class="output"><p>{{this.answer.submission_id}}</p></div>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
        <el-form :model='answer' ref='form'  size="samll" label-position='left'>
          <el-row :gutter='15'>
            <el-col :span='12'>
              <el-form-item :label="$t('m.username')" label-width="120px" prop="username">
                <div class="output"><p>{{this.name}}</p></div>
              </el-form-item>
            </el-col>
          </el-row>
        </el-form>
        <el-form :model='answer' ref='form'  size="samll" label-position='top'>
          <el-row :gutter='15'>
            <el-col :span='24'>
              <el-form-item :label="$t('m.question_content')" label-width="80px" prop="question_content">
                <div class="output"><p>{{answer.question_content}}</p></div>
                <!--
                  <el-button plain type="primary" :span='8' @click="goStatus">{{$t('m.Go_Code')}}</el-button>
                  <Simditor v-model="answer.question_content"></Simditor>
                -->
              </el-form-item>
            </el-col>
          </el-row>
        </el-form>
        <el-form :model='answer' ref='form'  size="samll" label-position='top'>
          <el-row :gutter='15'>
            <el-col :span='24'>
              <el-form-item :label="$t('m.input_description')" label-width="80px" prop="answer_contents">
                <!--
                <el-input :placeholder="$t('m.input_description')" v-model="answer.input_description"></el-input>
                -->
                <Simditor v-model="answer.answer_contents"></Simditor>
              </el-form-item>
            </el-col>
          </el-row>
        </el-form>
        <span slot="footer" class="dialog-footer">
          <save type="primary" @click.native="submitAnswer"></save>
          <cancel @click.native="backPage"></cancel>
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
          id: '',
          answer: {
            class_id: '',
            problem_id: '',
            submission_id: '',
            question_id: '',
            question_content: '',
            answer_contents: ''
          },
          // class_id: '',
          // problem_id: '',
          // question_content: 'Question 으로부터 Question Content를 호출!',
          // answer_contents: '',
          //
          // submission_id: '',
          username: '',
          profile: {},
          name: '',
          //
          mode: 'create',
          loading: true
        }
      },
      mounted () {
        this.init()
      },
      methods: {
        goStatus () {
          this.$router.push({
            name: 'answerRegister'
          })
        },
        backPage () {
          this.$router.go(-1)
        },
        init () {
          this.username = this.$route.query.username
          api.getUserInfo(this.username).then(res => {
            this.profile = res.data.data
            this.name = res.data.data.user.username
          })
        },
        getAnswer () {
          this.loading = true
          // this.$route.params.id - 이걸로 가능한가?
          api.getAnswer(this.$route.params.id).then(res => {
            this.loading = true
            let data = res.data.data
            this.question = data
          }, () => {
            this.loading = false
          })
        },
        submitAnswer (data = undefined) {
          //
          let funcName = ''
          if (!data.title) {
            data = {
              class_id: this.class_id,
              problem_id: this.problem_id,
              submission_id: this.submission_id,
              answer_contents: this.answer_contents,
              username: this.name
            }
            //
            funcName = this.mode === 'edit' ? 'updateAnswer' : 'createAnswer'
            //
            api.createAnswer(data).then(res => {
              this.init()
              this.$router.push({name: 'questionDetails', params: {questionID: this.answer.question_id}})
            }).catch()
          }
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
  .bg-puple-dark{
    background: #99a9bf;
  }
  .dialog-footer{
    float: right;
  }
  .output{
      height: 40px;
      padding-left: 15px;
      padding-right: 10px;
      border-radius: 5px;
      background: #f1f2f4;
  }
  .output_content{
      padding-left: 15px;
      padding-right: 10px;
      border-radius: 5px;
      background: #f1f2f4;
  }
  </style>
  