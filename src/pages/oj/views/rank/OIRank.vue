<template>
  <div id="table"> 
    <p>{{username}}</p>
    <p>{{profile}}</p>
    <p>비주얼라이징 테스트</p>
    <p>{{"max step: "}}{{maxStep}}</p>
    <p>{{"now step: "}}{{nowStep}}</p>
    <Button @click="prevStep">{{"prev_step"}}</Button>
    <Button @click="nextStep">{{"next_step"}}</Button>
    <table>
      <td>name</td>
      <td>value</td>
      <td>index</td>
      <tr v-if="index<nowStep" v-for="(item, index) in items" :key="items.name">
        <td><span v-if="index<nowStep" v-html="item.name"></span></td>
        <td><span v-if="index<nowStep" v-html="item.value"></span></td>
        <td><span v-if="index<nowStep" v-html="index"></span></td>
      </tr>
    </table>
    <p>{{"test: "}}{{hihihi.data}}</p>
    <li v-for="contest in hihihi.data" :key="hihihi.data">
      <p class="title">
        <a class="entry" @click.stop="goContest(contest.contest_id_list)">
          {{contest.contest_name}}
        </a>
      </p>
    </li>
    <li v-for="contest in hihihi.data" :key="hihihi.data">
      <button @click="goContest(contest.contest_id_list)">
        {{contest.contest_name}}
      </button>
    </li>
  </div>
</template>

<script>
  import Pagination from '@oj/components/Pagination'
  import api from '@oj/api'
  const width = window.innerWidth
  const height = window.innerHeight
  
  /* import utils from '@/utils/utils'
  import { RULE_TYPE } from '@/utils/constants' */

  export default {
    name: 'acm-rank',
    components: {
      Pagination
    },
    data () {
      return {
        username: '',
        profile: {},
        hihihi: [],
        nowStep: 0,
        maxStep: 0,
        testID: 18011668,
        width: width,
        height: height,
        items: [
          {
            'name': 'a',
            'value': '123'
          },
          {
            'name': 'b',
            'value': '234'
          },
          {
            'name': 'c',
            'value': '123'
          },
          {
            'name': 'd',
            'value': '456'
          }
        ]
      }
    },
    mounted () {
      this.init()
      this.getClass()
    },
    methods: {
      init () {
        this.username = this.$route.query.username
        this.maxStep = this.items.length
        api.getUserInfo(this.username).then(res => {
          this.profile = res.data.data
        })
      },
      goContest (contest) {
        this.$router.push({name: 'contest-details', params: {contestID: contest}})
      },
      getClass () {
        api.getClassList(this.testID).then(res => {
          this.hihihi = res.data
        })
      },
      prevStep () {
        if (this.nowStep <= 0) {
          this.nowStep = 0
        } else {
          this.nowStep = this.nowStep - 1
        }
      },
      nextStep () {
        if (this.items.length === this.nowStep) {
          this.nowStep = this.items.length
        } else {
          this.nowStep = this.nowStep + 1
        }
      },
      createRow () {
        this.prevStep = 0
      }
    }
  }
</script>
<style scoped lang="less">
  td, th, table {
      font-size: 30px;
      padding: 10px;
      border: 1px solid rgb(0, 0, 0);
      white-space: nowrap;
      width: fit-content;
    }
    td, th{
      text-align: center;
      white-space: nowrap;
      width: fit-content;
    }
    table{
      width: fit-content;
    }

</style>
