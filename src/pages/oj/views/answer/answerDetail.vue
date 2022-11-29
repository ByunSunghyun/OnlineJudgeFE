<template>
  <panel>
    <div slot="title">
        {{$t('m.Answer_regist')}}
    </div>
    
    <div class = "answer_register">
      <el-form ref='form'  size="samll" label-position='left'>
        <el-row :gutter='15'>
          <el-col :span='12'>
            <el-form-item :label="$t('m.Class_ID')" label-width="120px" prop="class_id">
              <div class="output"><p>{{this.answer.id}}</p></div>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
      <el-form ref='form'  size="samll" label-position='left'>
        <el-row :gutter='15'>
          <el-col :span='12'>
            <el-form-item :label="$t('m.Problem_ID')" label-width="120px" prop="problem_id">
              <div class="output"><p>{{this.answer.problem_id}}</p></div>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
      <el-form ref='form'  size="samll" label-position='left'>
        <el-row :gutter='15'>
          <el-col :span='12'>
            <el-form-item :label="$t('m.Submission_ID')" label-width="120px" prop="submission_id">
              <div class="output"><p>{{this.answer.submission_id}}</p></div>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
      <el-form ref='form'  size="samll" label-position='left'>
        <el-row :gutter='15'>
          <el-col :span='12'>
            <el-form-item :label="$t('m.username')" label-width="120px" prop="username">
              <div class="output"><p>{{this.name}}</p></div>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
      <el-form ref='form'  size="samll" label-position='top'>
        <el-row :gutter='15'>
          <el-col :span='24'>
            <el-form-item :label="$t('m.question_content')" label-width="80px" prop="question_content">
              <el-button plain type="primary" :span='8' @click="goStatus">{{$t('m.Go_Code')}}</el-button>
              <div class="output">{{answer.content}}</div>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
      <el-form ref='form'  size="samll" label-position='top'>
        <el-row :gutter='15'>
          <el-col :span='24'>
            <el-form-item :label="$t('m.input_description')" label-width="80px" prop="input_description">
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
        answer: {
          id: '', // question ID
          class_id: '',
          problem_id: '',
          submission_id: '',
          question_id: '',
          content: '',
          answer_contents: ''
        },
        username: '',
        name: '',
        profile: {},
        //
        mode: 'create',
        loading: true,
        pageSize: 15,
        totoal: 0,
        answerList: [],
        error: {
        }
      }
    },
    mounted () {
      this.init()
    },
    methods: {
      /*
      this.$router.push({name: 'answerRegister', params: {questionID: 'this.question.id'}}) 로 in
      */
      goStatus () {
        this.$router.push({name: 'submission-details', params: {id: this.answer.submission_id}})
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
        //
        this.getQuestion()
      },
      getQuestion () {
        this.loading = true
        api.getQuestion(this.$route.params.questionID).then(res => {
          this.loding = true
          let data = res.data.data
          this.answer = data
        }, () => {
          this.loding = false
        })
      },
      submitAnswer (data = undefined) {
        //
        data = {
          submission_id: this.answer.submission_id,
          question_id: this.answer.id,
          content: this.answer.answer_contents,
          username: this.name
        }
        //
        
        funcName = this.mode === 'edit' ? 'updateAnswer' : 'createAnswer'
        //
        api.createAnswer(data).then(res => {
          this.$router.push({name: 'questionDetail', params: {questionID: this.answer.id}})
        }).catch()
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