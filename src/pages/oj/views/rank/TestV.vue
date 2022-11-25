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
        <TreeChart :json='treeData2' />
      </div>
    </div>
    <TreeChart :json='treeData2' />
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
        'name': 'A',
        image_url: 'https://i.ibb.co/jZPmQrc/circle.png',
        mate: {
          name: 'B',
          image_url: 'https://i.ibb.co/jZPmQrc/circle.png'
        },
        'children': [
          {
            name: 'C',
            image_url:
              'https://i.ibb.co/jZPmQrc/circle.png',
            mate: {
              name: 'D',
              image_url:
                'https://i.ibb.co/jZPmQrc/circle.png'
            },
            children: [
              {
                name: 'E',
                image_url:
                  'https://i.ibb.co/jZPmQrc/circle.png'
              }
            ]
          },
          {
            name: 'F',
            image_url:
              'https://i.ibb.co/jZPmQrc/circle.png'
          },
          {
            name: 'G',
            image_url:
              'https://i.ibb.co/jZPmQrc/circle.png'
          }
        ]
      },
      treeData2: {
        'name': '*tree',
        'data': '{lChild = 0x55555555a6f0, rChild = 0x55555555a720, parent = 0x0, \n  key = 3}',
        'children': [
          {
            'name': '(*treerChild)',
            'data': '{lChild = 0x55555555a750, rChild = 0x0, parent = 0x55555555a2b0, \n  key = 7}',
            'children': [
              {
                'name': '*(((*tree->rChild)->lChild))',
                'data': '{lChild = 0x55555555a780, rChild = 0x0, parent = 0x55555555a720, \n  key = 6}',
                'children': [
                  {
                    'name': '(*(((*tree->rChild)->lChild))->lChild)',
                    'data': '{lChild = 0x0, rChild = 0x0, parent = 0x55555555a750, key = 5}',
                    'children': []
                  }
                ]},
              {
                'name': '((*treerChild)lChild)',
                'data': '(struct node *) 0x55555555a750',
                'children': []
              }
            ]
          },
          {
            'name': '(*tree->lChild)',
            'data': '{lChild = 0x0, rChild = 0x0, parent = 0x55555555a2b0, key = 2}',
            'children': []
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
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  //margin-top: 60px;
  border: 1px solid rgb(0, 0, 0);
  background-color: white;
}
</style>
  