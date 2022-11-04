<template>
  <panel>
    <div slot="title">
        {{$t('m.Question_regist')}}
    </div>
    <div class="content markdown-body">
      <ul>
        <li>{{$t('m.Where_is_the_input_and_the_output')}} 
          <p>{{$t('m.Where_is_the_input_and_the_output_answer_part_1')}} <code>stdin</code> ('{{$t('m.Standard_Input')}}') {{$t('m.Where_is_the_input_and_the_output_answer_part_3')}} <code>stdout</code>
            ('{{$t('m.Standard_Output')}}') {{$t('m.Where_is_the_input_and_the_output_answer_part_5')}} <code>scanf</code> {{$t('m.Where_is_the_input_and_the_output_answer_part_6')}} <code>cin</code>
            {{$t('m.Where_is_the_input_and_the_output_answer_part_7')}} <code>printf</code> {{$t('m.Where_is_the_input_and_the_output_answer_part_8')}} <code>cout</code> {{$t('m.Where_is_the_input_and_the_output_answer_part_9')}} <code>{{$t('m.Runtime_Error')}}</code>.
          </p>
        </li>
        <li>{{$t('m.What_is_the_meaning_of_submission_execution_time')}} 
        <p>{{$t('m.What_is_the_meaning_of_submission_execution_time_answer')}} 
        </p>
        </li>
        <li>{{$t('m.How_Can_I_use_CPP_Int64')}} 
          <p>{{$t('m.How_Can_I_use_CPP_Int64_answer_part_1')}}<code>long long</code> {{$t('m.How_Can_I_use_CPP_Int64_answer_part_2')}} <code>cin/cout</code> {{$t('m.or')}} <code>%lld</code>, {{$t('m.using')}}<code> __int64</code> {{$t('m.How_Can_I_use_CPP_Int64_answer_part_3')}} <code>{{$t('m.Compile_Error')}}</code>.</p>
        </li>
        <li>{{$t('m.Java_specifications')}}
          <p>{{$t('m.Java_specifications_answer_part_1')}} <code>Main</code> {{$t('m.Java_specifications_answer_part_2')}} <code>Main</code> {{$t('m.Java_specifications_answer_part_3')}}</p>
        </li>
        <li>{{$t('m.About_presentation_error')}}
          <p>{{$t('m.About_presentation_error_answer_part_1')}} <b>{{$t('m.last')}}</b> {{$t('m.About_presentation_error_answer_part_2')}} <code> {{$t('m.Wrong_Answer')}}</code>.</p>
        </li>
        <li>{{$t('m.How_to_report_bugs')}}
          <p>{{$t('m.How_to_report_bugs_answer_part_1')}} <a href="https://github.com/QingdaoU/OnlineJudge">Github</a>
            {{$t('m.How_to_report_bugs_answer_part_2')}}
          </p>
        </li>
      </ul>
    </div>
  </panel>
</template>

<script>
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
</style>


<!--
<template>
  <Panel shadow :padding="10">
      <div slot="title">
        {{$t('m.My_question')}}
        <i class="fa-solid fa-circle-question"></i>
      </div>
      <div slot="extra">
        <Button v-if="listVisible" type="info" @click="init" :loading="btnLoading">{{$t('m.Refresh')}}</Button>
        <Button v-else type="ghost" icon="ios-undo" @click="goBack">{{$t('m.Back')}}</Button>
      </div>

      <div>
        <Button type="button" @click="goRegist" style="margin: 10px">{{$t('m.Question_Regist')}}</Button>
      </div>
  </panel>

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
  
  </template>
  
-->

<!--

  <script>
  import api from '@oj/api'
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
  
  .announcements-container {
      margin-top: -10px;
      margin-bottom: 10px;
      li {
        padding-top: 15px;
        list-style: none;
        padding-bottom: 15px;
        margin-left: 20px;
        font-size: 16px;
        border-bottom: 1px solid rgba(187, 187, 187, 0.5);
        &:last-child {
          border-bottom: none;
        }
        .flex-container {
          .title {
            flex: 1 1;
            text-align: left;
            padding-left: 10px;
            a.entry {
              color: #495060;
              &:hover {
                color: #2d8cf0;
                border-bottom: 1px solid #2d8cf0;
              }
            }
          }
          .creator {
            flex: none;
            width: 200px;
            text-align: center;
          }
          .date {
            flex: none;
            width: 200px;
            text-align: center;
          }
        }
      }
    }
  
    .content-container {
      padding: 0 20px 20px 20px;
    }
  
    .no-announcement {
      text-align: center;
      font-size: 16px;
    }changeLocale
  
    .announcement-animate-enter-active {
      animation: fadeIn 1s;
    }
  
    /*   */
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
-->