<template>
    <div id="acm-rank">
      <vue-csv-import
        v-model="csv"
        :autoMatchFields="true"
        :autoMatchIgnoreCase="true"
        :map-fields="['student_id_list']"
      >
      </vue-csv-import>
      <div style="padding-top: 50px">
        <p>Results</p>
        {{ csv }}
      </div>
      <Button @click="pushStudentList">{{$t('학생 추가')}}</Button>
      <p>{{hihihi.data}}</p>
      <p>{{idid}}</p>
      <p>{{gana}}</p>
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
    .echarts {
      margin: 0 auto;
      width: 95%;
      height: 400px;
    }
  </style>
  