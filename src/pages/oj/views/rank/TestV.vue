<template>
    <div id="table"> 
      <p>ggg</p>
    </div>
  </template>
  
  <script>
    /* import api from '@oj/api' */

    export default {
      name: 'test-v',
      components: {
      },
      data () {
        return {
        }
      },
      mounted () {
        this.init()
      },
      methods: {
        init () {
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
  