<template>
  <panel>
    <div slot="title">
        {{$t('m.Question_regist')}}
    </div>
    
    <div class = "register">
      <el-form ref='form' size="samll" label-position='left'>
        <el-row :gutter='15'>
          <el-col :span='24'>
            <el-form-item :label="$t('m.Class_ID')" label-width="120px" prop="class_id">
              <el-input :placeholder="$t('m.Class_ID')" v-model="class_id"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
      <el-form ref='form'  size="samll" label-position='left'>
        <el-row :gutter='15'>
          <el-col :span='24'>
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
            <el-form-item :label="$t('m.Title')" label-width="80px" prop="title">
              <el-input :placeholder="$t('m.Title')" v-model="title"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
      <el-form ref='form'  size="samll" label-position='top'>
        <el-row :gutter='15'>
          <el-col :span='24'>
            <el-form-item :label="$t('m.Question_contents')" label-width="80px" prop="question_contents">
              <!--
              <el-input :placeholder="$t('m.question_contents')" v-model="question.question_contents"></el-input>
              -->
              <Simditor v-model="question_contents"></Simditor>
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
      <p>{{class_id}}</p>
      <p>{{title}}</p>
      <p>{{question_contents}}</p>
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
        rules: {
          class_id: {required: true, message: 'Class ID is required', trigger: 'blur'},
          problem_id: {required: true, message: 'Problem ID is required', trigger: 'blur'},
          submission_id: {required: true, message: 'Submission ID is required', trigger: 'blur'},
          title: {required: true, message: 'Title is required', trigger: 'blur'},
          question_contents: {required: true, message: 'Input Description is required', trigger: 'blur'}
        },
        question: {
          languages: [],
          io_mode: {'io_mode': 'Standard IO', 'input': 'input.txt', 'output': 'output.txt'}
        },
        //
        class_id: '',
        problem_id: '',
        title: '',
        question_contents: '',
        //
        submission_id: '',
        username: '',
        name: '',
        profile: {},
        //
        mode: 'create',
        loading: true,
        pageSize: 15,
        totoal: 0,
        questionList: [],
        error: {
        }
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
      },
      backPage () {
        this.$router.go(-1)
      },
      submitQuestion (data = undefined) {
        //
        if (!data.title) {
          data = {
            question_id: this.class_id,
            contest_id: 2,
            problem_id: 2,
            submission_id: 2,
            title: this.title,
            content: this.question_contents,
            username: this.name
          }
          api.createQuestion(data).then(res => {
            this.init()
            this.$router.push({name: 'questionDetail'})
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