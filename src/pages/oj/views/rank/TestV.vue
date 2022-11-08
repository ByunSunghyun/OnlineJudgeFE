<template>
  <div id="whole">
    <div id="result">
      <p>Result</p>
    </div>
    <div id="question_request">
      <Button @click="goQuestion">{{ "질문 등록하기" }}</Button>
    </div>
    <div id="split3">
      <div id="showCode">
        <p>Code</p>
        <span>{{$t('m.Lang')}}: {{submission.data.language}}</span>
        <span>{{$t('m.Author')}}: {{submission.data.username}}</span>
        <p>{{submission.data.code}}</p>
      </div>
      <div id="debugList">
        <p>{{ "max step: " }}{{ maxStep }}</p>
        <p>{{ "now step: " }}{{ nowStep }}</p>
        <Button @click="prevStep">{{ "prev_step" }}</Button>
        <Button @click="nextStep">{{ "next_step" }}</Button>
        <table>
          <td>name</td>
          <td>value</td>
          <td>type</td>
          <td>Check</td>
          <tr v-if="index < nowStep" v-for="(item, index) in items" :key="items.name">
            <td><span v-if="index < nowStep" v-html="item.name"></span></td>
            <td><span v-if="index < nowStep" v-html="item.value"></span></td>
            <td><span v-if="index < nowStep" v-html="index"></span></td>
            <td><span v-if="index < nowStep" v-html="index"></span></td>
          </tr>
        </table>
      </div>
      <div id="tree">
        <p>트리 구현 test</p>
        <TreeChart :json='treeData' />
      </div>
    </div>
  </div>
</template>
  
<script>
// import VNetworkGraph from 'v-network-graph'
// import 'v-network-graph/lib/style.css'
// import api from '@oj/api'
import TreeChart from 'vue-tree-chart'
export default {
  name: 'test-v',
  components: {
    TreeChart
  },
  data () {
    return {
      userID: 0,
      userP: [],
      landscape: [],
      nowStep: 0,
      maxStep: 0,
      submission: {
        'data': {
          'id': 'a1ae53b0b238a89d46df1648c25456e3',
          'problem': '1',
          'create_time': '2022-11-08T05:04:40.983545Z',
          'user_id': 9137,
          'username': '12345654',
          'code': '#include <stdio.h>    \nint main(){\n    int a, b;\n    scanf(\\"%d%d\\", &a, &b);\n    printf(\\"%d\\n\\", a+b);\n    return 0;\n}',
          'result': 0,
          'language': 'C',
          'shared': false,
          'statistic_info': {
            'time_cost': 1,
            'memory_cost': 1736704
          }
        }
      },
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
      ],
      treeData: {
        name: '할아버지',
        image_url: 'https://2.bp.blogspot.com/-2Ksv3eNd8rU/VaMNmDYJhnI/AAAAAAAAvZ8/beeIFBTV1yI/s800/man_55.png',
        mate: {
          name: '할머니',
          image_url: 'https://3.bp.blogspot.com/-FftTdIRficc/VaMN2XZPX4I/AAAAAAAAvfE/nfhfaBzUQO4/s800/woman_70.png'
        },
        children: [
          {
            name: '엄마',
            image_url:
              'https://3.bp.blogspot.com/-IHUKjJDgpoA/VbNBI6Yl5oI/AAAAAAAAv-0/Om-qYQS8HK8/s800/hair_curler.png',
            mate: {
              name: '아빠',
              image_url:
                'https://1.bp.blogspot.com/-YRqIo9Ha9bo/VZ-O3A_W2jI/AAAAAAAAu-U/8iRjzHwIPqc/s300/business01_laugh.png'
            },
            children: [
              {
                name: '나',
                image_url:
                  'https://1.bp.blogspot.com/-r0SdJLU4eWk/X1CK7MQfwvI/AAAAAAABaws/QoCcnan3kO42i4Gmhpjil0Iy0BvIGzY4wCNcBGAsYHQ/s400/hair_okappa_boy.png'
              }
            ]
          },
          {
            name: '삼촌',
            image_url:
              'https://3.bp.blogspot.com/-HlyNojAwQ_E/WGnPR_1sd-I/AAAAAAABA28/-nTxgf0ImnMvOaC8Fvrnw5hOPNdq90_5gCLcB/s800/hair_bouzu.png'
          },
          {
            name: '고모',
            image_url:
              'https://2.bp.blogspot.com/-214b8CKHI9M/VpjKd3RgirI/AAAAAAAA3IM/Ooil9YOS0Ds/s800/hair_okappa.png'
          }
        ]
      },
      node: {
        node1: { name: 'Node 1' },
        node2: { name: 'Node 2' },
        node3: { name: 'Node 3' },
        node4: { name: 'Node 4' }
      },
      edge: {
        edge1: { source: 'node1', target: 'node2' },
        edge2: { source: 'node2', target: 'node3' },
        edge3: { source: 'node3', target: 'node4' }
      }
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
    }
  }
}
</script>
<style scoped lang="less">
#split3 {
  display: flex;
  border: 1px solid rgb(0, 0, 0);
}
#showCode {
  flex: 1;
  border: 1px solid rgb(0, 0, 0);
}
#debugList{
  flex: 1;
  border: 1px solid rgb(0, 0, 0);
}
#tree {
  flex: 1;
  /* font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;*/
  text-align: center;
  color: #2c3e50;
  //margin-top: 60px;
  border: 1px solid rgb(0, 0, 0);
}
</style>
  