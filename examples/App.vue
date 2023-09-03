<template>
  <div class="super-flow-base-demo">
    <super-flow
      ref="superFlow"
      
      :node-list="nodeList"
      :link-list="linkList"
      :link-desc="linkDesc"
      
      :graph-menu="graphMenuList" 
      :node-menu="nodeMenuList"
      :link-menu="linkMenuList"
      
      :relation-mark="relationMark"
      :start-mark="startMark"
      :end-mark="endMark"
      :origin="origin"
      :enter-intercept="enterIntercept"
      :output-intercept="outputIntercept">
      
      <template v-slot:node="{meta, node}">
        <div :class="`flow-node flow-node-${meta.prop}`">
          <header>
            <!-- (공통) 노드 헤더 영역에 보여줄 내용  -->
            {{ meta.name }}
          </header>
          <section>
            <!-- (공통) 노드 섹션 영역에 보여줄 내용  -->
            {{ meta.desc }}
          </section>
        </div>
      </template>
    </super-flow>
  </div>
</template>

<script >
const relationMark = 'a'
const startMark = 'b'
const endMark = 'c'
const drawerType = {
  node: 0,
  link: 1
}

export default {
  data () {
    return {
      drawerType,
      relationMark,
      startMark,
      endMark,
      drawerConf: {
        title: '',
        visible: false,
        type: null,
        info: null,

        cancel: () => {
          this.drawerConf.visible = false
          if (this.drawerConf.type === drawerType.node) {
            this.$refs.nodeSetting.clearValidate()
          } else {
            this.$refs.linkSetting.clearValidate()
          }
        }
      },
      linkSetting: {
        desc: '',
        resetFields: () => null
      },
      nodeSetting: {
        name: '',
        desc: '',
        clearValidate: () => null
      },

      origin: [681, 465],
      nodeList: [],
      linkList: [],

// 배경에 우클릭했을때 나오는 메뉴
      graphMenuList: [ 
  [
    // {
    //   label: '시작 노드',
    //   disable (graph) {
    //     return !!graph.nodeList.find(node => node.meta.prop === 'start')
    //   },
    //   selected: (graph, coordinate) => {
    //     const start = graph.nodeList.find(node => node.meta.prop === 'start')
    //     if (!start) {
    //       graph.addNode({
    //         width: 100,
    //         height: 80,
    //         coordinate: coordinate,
    //         meta: {
    //           prop: 'start',
    //           name: '시작'
    //         }
    //       })
    //     }
    //   }
    // },
    {
      label: '조건 노드',
      disable: false,
      selected: (graph, coordinate) => {
        graph.addNode({
          width: 160,
          height: 80,
          coordinate: coordinate,
          meta: {
            prop: 'condition',
            name: '조건'
          }
        })
      }
    },
    {
      label: '승인 노드',
      disable: false,
      selected: (graph, coordinate) => {
        graph.addNode({
          width: 160,
          height: 80,
          coordinate: coordinate,
          meta: {
            prop: 'approval',
            name: '승인'
          }
        })
      }
    },
    {
      label: '참조 노드',
      disable: false,
      selected: (graph, coordinate) => {
        graph.addNode({
          width: 160,
          height: 80,
          coordinate: coordinate,
          meta: {
            prop: 'cc',
            name: '참조'
          }
        })
      }
    },
    {
      label: '종료 노드',
      disable (graph) {
        return !!graph.nodeList.find(point => point.meta.prop === 'end')
      },
      selected: (graph, coordinate) => {
        graph.addNode({
          width: 80,
          height: 50,
          coordinate: coordinate,
          meta: {
            prop: 'end',
            name: '종료 노드'
          }
        })
      }
    }
  ],
  [
    {
      label: '모든 데이터 출력',
      selected: (graph, coordinate) => {
        // console.log(JSON.stringify(graph.toJSON(), null, 2))
        console.log(JSON.stringify(graph.toJSON(), null, 2))
      }
    },
    {
      label: '모두 선택',
      selected: (graph, coordinate) => {
        graph.selectAll()
      }
    }
  ]
],

// 노드에 마우스 우클릭했을때 나오는 메뉴
      nodeMenuList: [ 
        [
          {
            label: '삭제',
            disable: false,
            hidden (node) {
              return node.meta.prop === 'start'
            },
            selected (node, coordinate) {
              node.remove()
            }
          }
        ],
        [
          {
            label: '로그 확인 (개발자용)',
            selected: (node, coordinate) => {
              // console.log(node, coordinate)
              console.log("=================")
              console.log(node.meta.desc)
              node.meta.desc = prompt('내용 입력')              
              console.log("=================")
            }
          }
        ]
      ],

// 노드간 연결되는 링크에 우클릭했을때 
      linkMenuList: [ 
        [
          {
            label: '삭제',
            disable: false,
            selected: (link, coordinate) => {
              link.remove()
            }
          }
        ],
        // [
        //   {
        //     label: '로그 확인 (개발자용)',
        //     disable: false,
        //     selected: (link, coordinate) => {
        //       console.log(link, coordinate)
        //     }
        //   }
        // ]
      ]
    }
  },





  created () {
    
    // ( 더미 데이터 ) 노드 및 노드간 링크들



    const nodeList = [
      {
        [relationMark]: 'nodeS3WgFnzCI15X58Qw',
        'width': 100,
        'height': 80,
        'coordinate': [-644, -148],
        'meta': {
          'prop': 'start',
          'name': '시작'
        }
      },
      {
        [relationMark]: 'nodefHsy9uJObPtdHZv1',
        'width': 160,
        'height': 80,
        'coordinate': [-200, -148],
        'meta': {
          'prop': 'approval',
          'name': '승인',
          'desc': '내용 아무거나',
        }
      },
      {
        [relationMark]: 'nodeni9QOqT3mI7hsMau',
        'width': 160,
        'height': 80,
        'coordinate': [-442, -275],
        'meta': {
          'prop': 'condition',
          'name': '조건',
        }
      },
      {
        [relationMark]: 'nodeZBK0ZPpgMe1exezE',
        'width': 160,
        'height': 80,
        'coordinate': [-200, -275],
        'meta': {
          'prop': 'approval',
          'name': '승인'
        }
      },
      {
        [relationMark]: 'nodeqkK9zjcDz53xKRlK',
        'width': 160,
        'height': 80,
        'coordinate': [34, -209],
        'meta': {
          'prop': 'cc',
          'name': '참조'
        }
      },
      {
        [relationMark]: 'nodeDhVU6w2KbEnJCjZs',
        'width': 80,
        'height': 50,
        'coordinate': [286, -133],
        'meta': {
          'prop': 'end',
          'name': '종료지점'
        }
      },
      {
        [relationMark]: 'nodesyxisLH1hJDdPsxx',
        'width': 160,
        'height': 80,
        'coordinate': [34, -75],
        'meta': {
          'prop': 'cc',
          'name': '참조'
        }
      },
      {
        [relationMark]: 'node0aiA9VuhjkiAdZCs',
        'width': 160,
        'height': 80,
        'coordinate': [-200, -2],
        'meta': {
          'prop': 'approval',
          'name': '승인'
        }
      },
      {
        [relationMark]: 'nodeG3WeFnzCI15X58Qw',
        'width': 160,
        'height': 80,
        'coordinate': [-442, -2],
        'meta': {
          'prop': 'condition',
          'name': '조건'
        }
      },
      {
        [relationMark]: 'node7WXbwOR6kSFD53Hf',
        'width': 160,
        'height': 80,
        'coordinate': [-442, -148],
        'meta': {
          'prop': 'condition',
          'name': '조건'
        }
      }
    ]
    const linkList = [
      {
        [relationMark]: 'linkcs9ZhumWeTHrtUy8',
        [startMark]: 'nodeS3WgFnzCI15X58Qw',
        [endMark]: 'nodeni9QOqT3mI7hsMau',
        'startAt': [100, 40],
        'endAt': [0, 40],
        'meta': null
      },
      {
        [relationMark]: 'linkBDld5rDBw4C6kiva',
        [startMark]: 'nodefHsy9uJObPtdHZv1',
        [endMark]: 'nodeqkK9zjcDz53xKRlK',
        'startAt': [160, 40],
        'endAt': [0, 40],
        'meta': null
      },
      {
        [relationMark]: 'linkA0ZZxRlDI9AOonuq',
        [startMark]: 'node7WXbwOR6kSFD53Hf',
        [endMark]: 'nodefHsy9uJObPtdHZv1',
        'startAt': [160, 40],
        'endAt': [0, 40],
        'meta': null
      },
      {
        [relationMark]: 'linkhCKTpRAf89gcujGS',
        [startMark]: 'nodeni9QOqT3mI7hsMau',
        [endMark]: 'nodeZBK0ZPpgMe1exezE',
        'startAt': [160, 40],
        'endAt': [0, 40],
        'meta': null
      },
      {
        [relationMark]: 'link2o7VZ7DRaSFKtB0g',
        [startMark]: 'nodeqkK9zjcDz53xKRlK',
        [endMark]: 'nodeDhVU6w2KbEnJCjZs',
        'startAt': [160, 40],
        'endAt': [0, 25],
        'meta': null
      },
      {
        [relationMark]: 'linkII013ovDctUDuPLu',
        [startMark]: 'nodeS3WgFnzCI15X58Qw',
        [endMark]: 'nodeG3WeFnzCI15X58Qw',
        'startAt': [100, 40],
        'endAt': [0, 40],
        'meta': null
      },
      {
        [relationMark]: 'link6MOmsq1EqzlWcG1n',
        [startMark]: 'nodeZBK0ZPpgMe1exezE',
        [endMark]: 'nodeqkK9zjcDz53xKRlK',
        'startAt': [160, 40],
        'endAt': [0, 40],
        'meta': null
      },
      {
        [relationMark]: 'link52SczSXHmuyKDzRU',
        [startMark]: 'nodesyxisLH1hJDdPsxx',
        [endMark]: 'nodeDhVU6w2KbEnJCjZs',
        'startAt': [160, 40],
        'endAt': [0, 25],
        'meta': null
      },
      {
        [relationMark]: 'link2hBQDTuIG4ZFYyE0',
        [startMark]: 'node0aiA9VuhjkiAdZCs',
        [endMark]: 'nodesyxisLH1hJDdPsxx',
        'startAt': [160, 40],
        'endAt': [0, 40],
        'meta': null
      },
      {
        [relationMark]: 'linkrwdW87FmOma5rPVo',
        [startMark]: 'nodeG3WeFnzCI15X58Qw',
        [endMark]: 'node0aiA9VuhjkiAdZCs',
        'startAt': [160, 40],
        'endAt': [0, 40],
        'meta': null
      },
      {
        [relationMark]: 'linknL75dQV0AWZA85sq',
        [startMark]: 'nodeS3WgFnzCI15X58Qw',
        [endMark]: 'node7WXbwOR6kSFD53Hf',
        'startAt': [100, 40],
        'endAt': [0, 40],
        'meta': null
      }
    ]



    
    setTimeout(() => {
      this.nodeList = nodeList
      this.linkList = linkList
    }, 100)
  },




  methods: {
    enterIntercept (formNode, toNode, graph) {
      const formType = formNode.meta.prop
      switch (toNode.meta.prop) {
        case 'start':
          return false
        case 'approval':
          return [
            'start',
            'approval',
            'condition',
            'cc'
          ].includes(formType)
        case 'condition':
          return [
            'start',
            'approval',
            'condition',
            'cc'
          ].includes(formType)
        case 'end':
          return [
            'approval',
            'cc'
          ].includes(formType)
        default:
          return true
      }
    },
    outputIntercept (node, graph) {
      return !(node.meta.prop === 'end')
    },
    linkDesc (link) {
      return link.meta ? link.meta.desc : ''
    }
  }
}
</script>

<style lang="less">
// 화면에 보여지는 너비
.super-flow-base-demo {
  width            : 100%;
  height           : 800px;
  margin           : 0 auto;
  background-color : #f5f5f5;
  overflow         : scroll;

  // 실제 사용 가능한 최대 너비
  .super-flow {
    width  : 4000px;
    height : 4000px;
  }

  .super-flow__node {

// 모든 노드들의 헤더 스타일 ( 공용 )
    .flow-node {
    > header {
        font-size   : 14px;
        height      : 32px;
        line-height : 32px;
        padding     : 0 12px;
        color       : #ffffff;
      }

// 모든 노드들의 섹션 스타일 ( 공용 )
      > section {
        text-align  : center;
        line-height : 20px;
        overflow    : hidden;
        padding     : 6px 12px;
        word-break  : break-all;
      }

// 시작 노드의 스타일 ( 개별 )      
      &.flow-node-start {
        > header {
          background-color : #55abfc;
        }
      }

// 조건 노드의 스타일 ( 개별 )            
      &.flow-node-condition {
        > header {
          background-color : #BC1D16;
        }
      }

// 승인 노드의 스타일 ( 개별 )            
      &.flow-node-approval {
        > header {
          background-color : rgba(188, 181, 58, 0.76);
        }
      }

// 참조 노드의 스타일 ( 개별 )            
      &.flow-node-cc {
        > header {
          background-color : #30b95c;
        }
      }

// 종료 노드의 스타일 ( 개별 )      
      &.flow-node-end {
        > header {
          height           : 50px;
          line-height      : 50px;
          background-color : rgb(0, 0, 0);
        }
      }
    }
  }
}
</style>