<template>
  <div id="table"> 
    <p>비주얼라이징 테스트</p>
    <p>{{"max step: "}}{{maxStep}}</p>
    <p>{{"now step: "}}{{prevStep}}</p>
    <Button @click="prevStep">{{"prev_step"}}</Button>
    <Button @click="nextStep">{{"next_step"}}</Button>
    <table>
      <td>name</td>
      <td>value</td>
      <td>index</td>
      <tr v-if="index<prevStep" v-for="(item, index) in items" :key="items.name">
        <td><span v-if="index<prevStep" v-html="item.name"></span></td>
        <td><span v-if="index<prevStep" v-html="item.value"></span></td>
        <td><span v-if="index<prevStep" v-html="index"></span></td>
      </tr>
    </table>
    <v-stage ref="stage" :config="stageSize">
      <v-layer>
      <v-text :config="{text: 'Some text on canvas', fontSize: 15}"/>
      <v-rect :config="{
          x: 20,
          y: 50,
          width: 100,
          height: 100,
          fill: 'red',
          shadowBlur: 10
        }"
      />
      <v-circle :config="{
          x: 200,
          y: 100,
          radius: 50,
          fill: 'green'
        }"
      /></v-layer>
      <v-layer ref="dragLayer"></v-layer>
    </v-stage>
  </div>
</template>

<script>
  import Pagination from '@oj/components/Pagination'
  const width = window.innerWidth
  const height = window.innerHeight
  /* import api from '@oj/api'
  
  //import utils from '@/utils/utils'
  //import { RULE_TYPE } from '@/utils/constants' */

  export default {
    name: 'acm-rank',
    components: {
      Pagination
    },
    data () {
      return {
        prevStep: 0,
        maxStep: 0,
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
    },
    methods: {
      init () {
        this.maxStep = this.items.length
      },
      prevStep () {
        if (this.prevStep <= 0) {
          this.prevStep = 0
        } else {
          this.prevStep = this.prevStep - 1
        }
      },
      nextStep () {
        if (this.items.length === this.prevStep) {
          this.prevStep = this.items.length
        } else {
          this.prevStep = this.prevStep + 1
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
