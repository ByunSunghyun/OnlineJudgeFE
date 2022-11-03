<template>
  <Panel shadow :padding="10">
    <div slot="title">
        {{$t('m.Question_regist')}}
    </div>
  </panel>
  <!--

    <div class="question">
      <div slot="title">
          {{$t('m.Question_regist')}}
      </div>
      <Panel :title="title">
        <el-form ref="form" :model="problem" :rules="rules" label-position="top" label-width="70px">
          <el-row :gutter="20">
            <el-col :span="6">
              <el-form-item prop="_id" :label="$t('m.Display_ID')"
                            :required="this.routeName === 'create-contest-problem' || this.routeName === 'edit-contest-problem'">
                <el-input :placeholder="$t('m.Display_ID')" v-model="problem._id"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="18">
              <el-form-item prop="title" :label="$t('m.Title')" required>
                <el-input :placeholder="$t('m.Title')" v-model="problem.title"></el-input>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row :gutter="20">
            <el-col :span="24">
              <el-form-item prop="description" :label="$t('m.Description')" required>
                <Simditor v-model="problem.description"></Simditor>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row :gutter="20">
            <el-col :span="24">
              <el-form-item prop="input_description" :label="$t('m.Input_Description')" required>
                <Simditor v-model="problem.input_description"></Simditor>
              </el-form-item>
            </el-col>
            <el-col :span="24">
              <el-form-item prop="output_description" :label="$t('m.Output_Description')" required>
                <Simditor v-model="problem.output_description"></Simditor>
              </el-form-item>
            </el-col>
          </el-row>
        </el-form>
      </Panel>
    </div>
  -->
</template>

<script>
export default {
  name: 'QuestionRegister',
  components: {
  },
  data () {
    return {
      title: ''
    }
  }
}
</script>

<style lang="less" scoped>
  .problem {
    .difficulty-select {
      width: 120px;
    }
    .spj-radio {
      margin-left: 10px;
      &:last-child {
        margin-right: 20px;
      }
    }
    .input-new-tag {
      width: 78px;
    }
    .button-new-tag {
      height: 24px;
      line-height: 22px;
      padding-top: 0;
      padding-bottom: 0;
    }
    .tags {
      .el-tag {
        margin-right: 10px;
      }
    }
    .accordion {
      margin-bottom: 10px;
    }
    .add-samples {
      width: 100%;
      background-color: #fff;
      border: 1px dashed #aaa;
      outline: none;
      cursor: pointer;
      color: #666;
      height: 35px;
      font-size: 14px;
      &:hover {
        background-color: #f9fafc;
      }
      i {
        margin-right: 10px;
      }
    }
    .add-sample-btn {
      margin-bottom: 10px;
    }

  }
</style>

<style>
  .problem-tag-poper {
    width: 200px !important;
  }
  .dialog-compile-error {
    width: auto;
    max-width: 80%;
    overflow-x: scroll;
  }
</style>



<!--
-->