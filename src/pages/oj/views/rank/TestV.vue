<template>
  <div id="table">
    <p>ggg</p>
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
        name: '波平',
        image_url:
          'https://2.bp.blogspot.com/-2Ksv3eNd8rU/VaMNmDYJhnI/AAAAAAAAvZ8/beeIFBTV1yI/s800/man_55.png',
        mate: {
          name: 'フネ',
          image_url:
            'https://3.bp.blogspot.com/-FftTdIRficc/VaMN2XZPX4I/AAAAAAAAvfE/nfhfaBzUQO4/s800/woman_70.png'
        },
        children: [
          {
            name: 'サザエ',
            image_url:
              'https://3.bp.blogspot.com/-IHUKjJDgpoA/VbNBI6Yl5oI/AAAAAAAAv-0/Om-qYQS8HK8/s800/hair_curler.png',
            mate: {
              name: 'マスオ',
              image_url:
                'https://1.bp.blogspot.com/-YRqIo9Ha9bo/VZ-O3A_W2jI/AAAAAAAAu-U/8iRjzHwIPqc/s300/business01_laugh.png'
            },
            children: [
              {
                name: 'タラオ',
                image_url:
                  'https://1.bp.blogspot.com/-r0SdJLU4eWk/X1CK7MQfwvI/AAAAAAABaws/QoCcnan3kO42i4Gmhpjil0Iy0BvIGzY4wCNcBGAsYHQ/s400/hair_okappa_boy.png'
              }
            ]
          },
          {
            name: 'カツオ',
            image_url:
              'https://3.bp.blogspot.com/-HlyNojAwQ_E/WGnPR_1sd-I/AAAAAAABA28/-nTxgf0ImnMvOaC8Fvrnw5hOPNdq90_5gCLcB/s800/hair_bouzu.png'
          },
          {
            name: 'ワカメ',
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
td,
th,
table {
  font-size: 30px;
  padding: 10px;
  border: 1px solid rgb(0, 0, 0);
  white-space: nowrap;
  width: fit-content;
}

td,
th {
  text-align: center;
  white-space: nowrap;
  width: fit-content;
}

table {
  width: fit-content;
}
</style>
  