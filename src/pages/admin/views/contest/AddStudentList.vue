<template>
  <div class="acm-rank">
    <p>{{"This class id: "}}{{idid}}</p>
    <vue-csv-import 
      v-model="csv"
      :autoMatchFields="true"
      :autoMatchIgnoreCase="true"
      :map-fields="['student_id_list']"> </vue-csv-import>
    <div style="padding-top: 50px">
      <p>Results</p>
      {{ csv }}
    </div>
    <Button class="btn1" @click="pushStudentList">{{$t('학생 추가')}}</Button>
    <p>{{hihihi.data}}</p>
    <p>{{"upload success? "}}{{gana}}</p>
  </div>
</template>
  
  <script>
    import api from '@oj/api'
    import Pagination from '@oj/components/Pagination'
    import {VueCsvImport} from 'vue-csv-import'
    export default {
      name: 'acm-rank',
      components: {
        Pagination,
        VueCsvImport
      },
      data () {
        return {
          gana: false,
          idid: 0,
          limit: 10,
          hihihi: [],
          total: 10,
          btnLoading: false,
          testjsondata: [],
          csv: null
        }
      },
      created () {
        this.init()
      },
      mounted () {
        this.init()
      },
      methods: {
        init () {
          this.idid = this.$route.params.contestId
        },
        pushStudentList () {
          api.ContestStudentIdAPI(this.idid, this.csv).then(res => {
            this.gana = true
          })
        }
      }
    }
  </script>
  
  <style scoped lang="less">
    .btn1{
      background-color: rgb(114, 173, 172);
      border: none;
      color: black;
      padding: 15px 30px;
      text-align: center;
      text-decoration: none;
      display: inline-block;
      font-size: 20px;
      margin: 4px 2px;
      cursor: pointer;
      border-radius: 5%;
    }
    .acm-rank{
      background-color: white;
      font-size: 30px;
    }
  </style>
  