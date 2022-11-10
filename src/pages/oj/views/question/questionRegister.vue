<template>
  <panel>
    <div slot="title">
        {{$t('m.Question_regist')}}
    </div>
    
    <div class = "register">
      <el-form ref='form'  size="samll" label-position='left' :rules='rules'>
        <el-row :gutter='15'>
          <el-col :span='12'>
            <el-form-item :label="$t('m.Class_ID')" label-width="120px" prop="_class_id">
              <el-input :placeholder="$t('m.Class_ID')" v-model="_class_id"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
      <el-form ref='form'  size="samll" label-position='left' :rules='rules'>
        <el-row :gutter='15'>
          <el-col :span='12'>
            <el-form-item :label="$t('m.Problem_ID')" label-width="120px" prop="_problem_id">
              <el-input :placeholder="$t('m.Problem_ID')" v-model="_problem_id"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
      <el-form ref='form'  size="samll" label-position='left' :rules='rules'>
        <el-row :gutter='15'>
          <el-col :span='12'>
            <el-form-item :label="$t('m.Submission_ID')" label-width="120px" prop="_submission_id">
              <el-input :placeholder="$t('m.Submission_ID')" v-model="_submission_id"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
      <el-form ref='form'  size="samll" label-position='left' :rules='rules'>
        <el-row :gutter='15'>
          <el-col :span='24'>
            <el-form-item :label="$t('m.Title')" label-width="80px" prop="title">
              <el-input :placeholder="$t('m.Title')" v-model="title"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
      <el-form ref='form'  size="samll" label-position='top' :rules='rules'>
        <el-row :gutter='15'>
          <el-col :span='24'>
            <el-form-item :label="$t('m.question_contents')" label-width="80px" prop="question_contents">
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
          _class_id: {required: true, message: 'Class ID is required', trigger: 'blur'},
          _problem_id: {required: true, message: 'Problem ID is required', trigger: 'blur'},
          _submission_id: {required: true, message: 'Submission ID is required', trigger: 'blur'},
          title: {required: true, message: 'Title is required', trigger: 'blur'},
          question_contents: {required: true, message: 'Input Description is required', trigger: 'blur'}
        },
        question: {
          languages: [],
          io_mode: {'io_mode': 'Standard IO', 'input': 'input.txt', 'output': 'output.txt'}
        },
        //
        _class_id: '',
        _problem_id: '',
        title: '',
        question_contents: '',
        //
        _submission_id: '',
        username: '',
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
        this.username = ''
      },
      backPage () {
        this.$router.go(-1)
      },
      submitQuestion (data = undefined) {
        //
        let funcName = ''
        if (!data.title) {
          data = {
            contest_id: this._class_id,
            problem_id: this._problem_id,
            submission_id: this._submission_id,
            title: this.title,
            content: this.question_contents,
            username: this.username
          }
          //
          funcName = this.mode === 'edit' ? 'updateQuestion' : 'createQuestion'
          //
          api.createQuestion(data).then(res => {
            this.init()
            this.$router.push({name: 'questionDetails'})
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

</style>
