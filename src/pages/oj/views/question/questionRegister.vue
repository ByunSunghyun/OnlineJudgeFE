<template>
  <panel>
    <div slot="title">
        {{$t('m.Question_regist')}}
    </div>
    
    <div class = "register">
      <el-form ref='form'  size="samll" label-position='left'>
        <el-row :gutter='15'>
          <el-col :span='12'>
            <el-form-item :label="$t('m.Class_ID')" label-width="120px" prop="class_id">
              <!--
                <el-input :placeholder="$t('m.Class_ID')" v-model="class_id"></el-input>
              -->
              <div class="output"><p>{{this.question.class_id}}</p></div>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
      <el-form ref='form'  size="samll" label-position='left'>
        <el-row :gutter='15'>
          <el-col :span='12'>
            <el-form-item :label="$t('m.Problem_ID')" label-width="120px" prop="problem_id">
              <!--
                <el-input :placeholder="$t('m.Problem_ID')" v-model="problem_id"></el-input>
              -->
              <div class="output"><p>{{this.question.problem_id}}</p></div>
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
              <div class="output"><p>{{this.question.submission_id}}</p></div>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
      <el-form ref='form'  size="samll" label-position='left'>
        <el-row :gutter='15'>
          <el-col :span='12'>
            <el-form-item :label="$t('m.username')" label-width="120px" prop="username">
              <!--
                <div class="output"><p>{{profile.user.username}}</p></div>
              -->
              <div class="output"><p>{{this.name}}</p></div>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
      <el-form ref='form'  size="samll" label-position='left'>
        <el-row :gutter='15'>
          <el-col :span='24'>
            <el-form-item :label="$t('m.Title')" label-width="80px" prop="question.title">
              <el-input :placeholder="$t('m.Title')" v-model="question.title"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
      <el-form ref='form'  size="samll" label-position='top'>
        <el-row :gutter='15'>
          <el-col :span='24'>
            <el-form-item :label="$t('m.question_contents')" label-width="80px" prop="question.question_contents">
              <!--
              <el-input :placeholder="$t('m.question_contents')" v-model="question.question_contents"></el-input>
              -->
              <Simditor v-model="question.question_contents"></Simditor>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <save type="primary" @click.native="submitQuestion"></save>
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
    name: 'QuestionRegister',
    components: {
      Simditor
    },
    data () {
      return {
        id: '',
        question: {
          class_id: '',
          problem_id: '',
          submission_id: '',
          title: '',
          question_contents: ''
        },
        //
        // class_id: '',
        // problem_id: '',
        // title: '',
        // question_contents: '',
        // submission_id: '',
        username: '',
        name: '',
        profile: {},
        //
        mode: 'create',
        loading: true
      }
    },
    mounted () {
      this.init()
    },
    methods: {
      init () {
        this.username = this.$route.query.username
        api.getUserInfo(this.username).then(res => {
          this.profile = res.data.data
          this.name = res.data.data.user.username
        })
        this.getQuestion()
      },
      backPage () {
        this.$router.go(-1)
      },
      getQuestion () {
        this.loading = true
        // this.$route.params.id - 이걸로 가능한가?
        api.getQuestion(this.$route.params.id).then(res => {
          this.loading = true
          let data = res.data.data
          this.question = data
        }, () => {
          this.loading = false
        })
      },
      submitQuestion (data = undefined) {
        //
        let funcName = ''
        if (!data.title) {
          data = {
            contest_id: this.question.class_id,
            problem_id: this.question.problem_id,
            submission_id: this.question.submission_id,
            title: this.question.title,
            content: this.question.question_contents,
            username: this.name
          }
          //
          funcName = this.mode === 'edit' ? 'updateQuestion' : 'createQuestion'
          //
          api.createQuestion(data).then(res => {
            // questionID value 수정 필요
            this.$router.push({name: 'questionDetails', params: {questionID: data.result.question_id}})
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
  .register{
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
</style>
