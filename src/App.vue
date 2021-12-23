<template>
  <div class="app-container">
    <!-- header  头部区域 -->
    <Header :title="cars"></Header>
    <Goods 
    v-for="item in list" 
    :key="item.id"
    :id="item.id"
    :title="item.goods_name" 
    :pic="item.goods_img" 
    :price="item.goods_price"
    :state="item.goods_state"
    :count='item.goods_count'
    @state-change="getNewState"
    >
    <template>
      <!-- 在goods组件里面预留了一个插槽位置，我们把goods渲染给app，最后在goods组件里写上counter组件达到在app里直接渲染了counter组件，这样的好处是counter相当于在goods组件里app可以父传子直接传值 -->
      <Counter :num="item.goods_count" @num-change="getNewNum(item,$event)">
    </Counter>
    </template>
    </Goods>
    <Footer 
    :isfull='fullState' 
    :amount='amt' 
    :all="total" 
    @ull-change="getFullState" 
    ></Footer>
  </div>
</template>

<script>
// 导入axios请求库
import axios from 'axios'
// 导入需要的组件
import Header from './components/Header/Header'
import Goods from './components/Goods/Goods'
import Footer from './components/Footer/Footer'
// import bus from './components/eventBus'
import Counter from './components/Counter/Counter'
export default {
  components: {
    Header,
    Goods,
    Footer,
    Counter
  },
  data () {
    return {
      cars: '购物车案例',
      list: []
    }
  },
  computed: {
    // every方法是筛选数组里面符合条件的值，如果数组里面都符合那么返回true
    fullState() {
      return this.list.every(item => item.goods_state)
    },
    amt() {
      // 先筛选符合条件的
      return this.list
      .filter(item => item.goods_state === true)
      // 在计算商品数量乘以单价相加
      .reduce((total,item) => total += item.goods_price * item.goods_count,0) 
    },
    // 最后计算一共购买多少件把值传给footer组件
   total () {
     return this.list.filter(item => item.goods_state === true).reduce((t,item) => {
       return t += item.goods_count
     },0)
   }
      
    
  },
  methods: {
    // 请求数据
    async initCartList () {
       const {data: res} = await axios.get('https://www.escook.cn/api/cart')
      //  console.log(res);
      // 把请求中的要渲染的数据都要转存到data中
      if(res.status === 200) {
        this.list = res.list
      }
    },
    getNewState(e) {
      // 过滤获取的商品总数据提取符合条件的
      this.list.some(item => {
        if(item.id === e.id) {
          item.goods_state = e.value
          // 终止循环
          return true
        }
      })
    },
    // 接受footer子组件传过来的全选按钮的状态
    getFullState (val) {
      this.list.forEach(item => item.goods_state = val)
    },
    getNewNum(item,e) {
      item.goods_count = e

    },
    getNewNumer () {
      item.goods_count = e
    }
    



  }, 
  created () {
    this.initCartList ()
    // 兄弟组件传值要在created里面用$on 接收
    // bus.$on('share',val => {
      // console.log(val);
    //   this.list.some(item => {
    //     if(item.id === val.id) {
    //       item.goods_count = val.value
    //       return true
    //     }
    //   })
    // })

  }
   
}
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
