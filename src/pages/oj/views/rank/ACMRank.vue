<template>
  <!--<Row type="flex" justify="space-around">
    <Col :span="22">
    <Panel :padding="10">
      <div slot="title">{{$t('m.ACM_Ranklist')}}</div>
      <div class="echarts">
        <ECharts :options="options" ref="chart" auto-resize></ECharts>
      </div>
    </Panel>
    <Table :data="dataRank" :columns="columns" :loading="loadingTable" size="large"></Table>
    <Pagination :total="total" :page-size.sync="limit" :current.sync="page"
                @on-change="getRankData" show-sizer
                @on-page-size-change="getRankData(1)"></Pagination>
    </Col>
  </Row>
  -->
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
    <p>{{$t('m.No_Announcements')}}</p>
    <p>{{$t('Profile_Setting')}}</p>
    <p>{{total}}</p>
    <p>{{tfaRequired}}</p>
    <p>{{testcase}}</p>
    <Button @click="init">{{$t('m.Refresh')}}</Button>
    <li v-for="announcement in announcements" :key="announcement.title">
      <div class="creator"> {{announcement.title}} {{$t('m.By')}} {{announcement.created_by.username}}</div>
    </li>
    <p>{{"가나?"}}{{gana}}</p>
    <p>{{"get한거"}}{{hihihi}}</p>
    <p>{{hihihi}}</p>
  </div>
</template>

<script>
  import api from '@oj/api'
  import Pagination from '@oj/components/Pagination'
  import utils from '@/utils/utils'
  import { RULE_TYPE } from '@/utils/constants'
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
        limit: 10,
        hihihi: [],
        total: 10,
        btnLoading: false,
        testjsondata: [],
        announcements: [],
        announcement: '',
        csv: null,
        csv2: '{{ "student_id_list": "17010136" }, { "student_id_list": "17010524" }, { "student_id_list": "17010689" }, { "student_id_list": "17011511" }, { "student_id_list": "17011677" }, { "student_id_list": "17011691" }, { "student_id_list": "17013148" }, { "student_id_list": "17013237" }, { "student_id_list": "18011334" }, { "student_id_list": "18011646" }, { "student_id_list": "18011648" }, { "student_id_list": "18011659" }, { "student_id_list": "18011668" }, { "student_id_list": "18011681" }, { "student_id_list": "18011683" }, { "student_id_list": "18011684" }, { "student_id_list": "18011700" }, { "student_id_list": "18011727" }, { "student_id_list": "18011740" }, { "student_id_list": "18011771" }, { "student_id_list": "18013186" }, { "student_id_list": "19011654" }, { "student_id_list": "19011659" }, { "student_id_list": "19011675" }, { "student_id_list": "19013128" }, { "student_id_list": "19013137" }, { "student_id_list": "20003318" }, { "student_id_list": "20011161" }, { "student_id_list": "20011475" }, { "student_id_list": "20011733" }, { "student_id_list": "20011757" }, { "student_id_list": "20011761" }, { "student_id_list": "20011764" }, { "student_id_list": "20011770" }, { "student_id_list": "20011778" }, { "student_id_list": "20011788" }, { "student_id_list": "20011789" }, { "student_id_list": "20011790" }, { "student_id_list": "20012646" }}',
        testname: '1',
        testcase: '아!',
        tfaRequired: true,
        page: 1,
        loadingTable: false,
        dataRank: [],
        columns: [
          {
            align: 'center',
            width: 60,
            render: (h, params) => {
              return h('span', {}, params.index + (this.page - 1) * this.limit + 1)
            }
          },
          {
            title: this.$i18n.t('m.User_User'),
            align: 'center',
            render: (h, params) => {
              return h('a', {
                style: {
                  'display': 'inline-block',
                  'max-width': '200px'
                },
                on: {
                  click: () => {
                    this.$router.push(
                      {
                        name: 'user-home',
                        query: {username: params.row.user.username}
                      })
                  }
                }
              }, params.row.user.username)
            }
          },
          {
            title: this.$i18n.t('m.mood'),
            align: 'center',
            key: 'mood'
          },
          {
            title: this.$i18n.t('m.AC'),
            align: 'center',
            key: 'accepted_number'
          },
          {
            title: this.$i18n.t('m.Total'),
            align: 'center',
            key: 'submission_number'
          },
          {
            title: this.$i18n.t('m.Rating'),
            align: 'center',
            render: (h, params) => {
              return h('span', utils.getACRate(params.row.accepted_number, params.row.submission_number))
            }
          }
        ],
        options: {
          tooltip: {
            trigger: 'axis'
          },
          legend: {
            data: [this.$i18n.t('m.AC'), this.$i18n.t('m.Total')]
          },
          grid: {
            x: '3%',
            x2: '3%'
          },
          toolbox: {
            show: true,
            feature: {
              dataView: {show: true, readOnly: true},
              magicType: {show: true, type: ['line', 'bar', 'stack']},
              saveAsImage: {show: true}
            },
            right: '10%'
          },
          calculable: true,
          xAxis: [
            {
              type: 'category',
              data: ['root'],
              axisLabel: {
                interval: 0,
                showMinLabel: true,
                showMaxLabel: true,
                align: 'center',
                formatter: (value, index) => {
                  return utils.breakLongWords(value, 10)
                }
              }
            }
          ],
          yAxis: [
            {
              type: 'value'
            }
          ],
          series: [
            {
              name: this.$i18n.t('m.AC'),
              type: 'bar',
              data: [0],
              markPoint: {
                data: [
                  {type: 'max', name: 'max'}
                ]
              }
            },
            {
              name: this.$i18n.t('m.Total'),
              type: 'bar',
              data: [0],
              markPoint: {
                data: [
                  {type: 'max', name: 'max'}
                ]
              }
            }
          ]
        }
      }
    },
    created () {
      this.testcase(1)
    },
    mounted () {
      this.getRankData(1)
      this.init()
    },
    methods: {
      testChange (testchar) {
        this.testcase = 'testchar'
      },
      init () {
        api.tfaRequiredCheck(this.testname).then(res => {
          this.tfaRequired = res.data.data.result
          this.testcase = res.data.data.result
        })
        this.getAnnouncementList()
        api.ContestStudentIdAPI(1, this.csv2).then(res => {
          this.gana = true
        })
        api.getContestStudentIdAPI(1).then(res => {
          // this.gana = true
          this.hihihi = res.data.data.result
        })
      },
      getAnnouncementList (page = 1) {
        this.btnLoading = true
        api.getAnnouncementList((page - 1) * this.limit, this.limit).then(res => {
          this.btnLoading = false
          this.announcements = res.data.data.results
          this.total = res.data.data.total
        }, () => {
          this.btnLoading = false
        })
        this.testjsondata = {
          id: 1,
          student: this.csv
        }
      },
      getRankData (page) {
        let offset = (page - 1) * this.limit
        let bar = this.$refs.chart
        bar.showLoading({maskColor: 'rgba(250, 250, 250, 0.8)'})
        this.loadingTable = true
        api.getUserRank(offset, this.limit, RULE_TYPE.ACM).then(res => {
          this.loadingTable = false
          if (page === 1) {
            this.changeCharts(res.data.data.results.slice(0, 10))
          }
          this.total = res.data.data.total
          this.dataRank = res.data.data.results
          bar.hideLoading()
        }).catch(() => {
          this.loadingTable = false
          bar.hideLoading()
        })
      },
      changeCharts (rankData) {
        let [usernames, acData, totalData] = [[], [], []]
        rankData.forEach(ele => {
          usernames.push(ele.user.username)
          acData.push(ele.accepted_number)
          totalData.push(ele.submission_number)
        })
        this.options.xAxis[0].data = usernames
        this.options.series[0].data = acData
        this.options.series[1].data = totalData
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
