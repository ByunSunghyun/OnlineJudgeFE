<template>
  <div id="tree">
    <p>트리 구현 test</p>
    <TreeChart :json='treeData' />
  </div>
</template>
  
<script>
// import VNetworkGraph from 'v-network-graph'
// import 'v-network-graph/lib/style.css'
/* import api from '@oj/api' */
import TreeChart from 'vue-tree-chart'
export default {
  name: 'test-v',
  components: {
    TreeChart
  },
  data () {
    return {
      landscape: [],
      treeData: {
        name: '할아버지',
        image_url:
          'https://2.bp.blogspot.com/-2Ksv3eNd8rU/VaMNmDYJhnI/AAAAAAAAvZ8/beeIFBTV1yI/s800/man_55.png',
        mate: {
          name: '할머니',
          image_url:
            'https://3.bp.blogspot.com/-FftTdIRficc/VaMN2XZPX4I/AAAAAAAAvfE/nfhfaBzUQO4/s800/woman_70.png'
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
    }
  }
}
</script>
<style scoped lang="less">
#tree {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
  