<template>
  <div>
    <table class="tbl">
      <thead class="thead">
        <tr>
          <th>{{"Class No."}}</th>
          <th>{{"Class name"}}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in hihihi.data" :key="hihihi.data">
          <td class="td1"><span>{{index+1}}</span></td> 
          <td><span @click="goContestl(item.contest_id_list)">{{item.contest_name}}</span></td>
        </tr>
      </tbody>
    </table>
    <!-- <Row type="flex" justify="space-around">
    <Col :span="22">
    <panel shadow v-if="contests.length" class="contest">
      <div slot="title">
        <Button type="text"  class="contest-title" @click="goContest">{{contests[index].title}}</Button>
      </div>
      <Carousel v-model="index" trigger="hover" autoplay :autoplay-speed="6000" class="contest">
        <CarouselItem v-for="(contest, index) of contests" :key="index">
          <div class="contest-content">
            <div class="contest-content-tags">
              <Button type="info" shape="circle" size="small" icon="calendar">
                {{contest.start_time | localtime('YYYY-M-D HH:mm') }}
              </Button>
              <Button type="success" shape="circle" size="small" icon="android-time">
                {{getDuration(contest.start_time, contest.end_time)}}
              </Button>
              <Button type="warning" shape="circle" size="small" icon="trophy">
                {{contest.rule_type}}
              </Button>
            </div>
            <div class="contest-content-description">
              <blockquote v-html="contest.description"></blockquote>
            </div>
          </div>
        </CarouselItem>
      </Carousel>
    </panel>
    <Announcements class="announcement"></Announcements>
    </Col>
    </Row>
    <p>{{hihihi.data}}</p>
    -->
  </div>
</template>

<script>
  import Announcements from './Announcements.vue'
  import api from '@oj/api'
  import time from '@/utils/time'
  import { CONTEST_STATUS } from '@/utils/constants'

  export default {
    name: 'home',
    components: {
      Announcements
    },
    data () {
      return {
        contests: [],
        hihihi: [],
        index: 0,
        username: '',
        profile: {},
        userID: ''
      }
    },
    mounted () {
      this.username = this.$route.query.username
      let params = {status: CONTEST_STATUS.NOT_START}
      api.getContestList(0, 5, params).then(res => {
        this.contests = res.data.data.results
      })
      this.init()
    },
    methods: {
      getDuration (startTime, endTime) {
        return time.duration(startTime, endTime)
      },
      init () {
        api.getUserInfo(this.username).then(res => {
          this.profile = res.data.data
          this.userID = this.profile.user.username
          this.getClass()
        })
      },
      getClass () {
        api.getClassList(this.userID).then(res => {
          this.hihihi = res.data
        })
      },
      goContestl (contest) {
        this.$router.push({name: 'contest-details', params: {contestID: contest}})
      },
      goContest () {
        this.$router.push({
          name: 'contest-details',
          params: {contestID: this.contests[this.index].id}
        })
      }
    }
  }
</script>

<style lang="less" scoped>
  .contest {
    &-title {
      font-style: italic;
      font-size: 21px;
    }
    &-content {
      padding: 0 70px 40px 70px;
      &-description {
        margin-top: 25px;
      }
    }
  }

  .announcement {
    margin-top: 20px;
  }
  .tbl {
    border: 1px solid black; 
    background-color: white;
    font-style: italic;
    font-size: 21px;
    text-align: center;
  }
  
  .thead {
    border: 1px black;
    background-color: lightblue;
  }
  .tr, td{
    border-bottom: 1px solid black;
    padding: 10px;
  }
  .td1{
    width: 200px;
  }
</style>
