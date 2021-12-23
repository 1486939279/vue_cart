<template>
  <div class="number-container d-flex justify-content-center align-items-center">
    <!-- 减 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="sub">-</button>
    <!-- 购买的数量 -->
    <span class="number-box">{{ num }}</span>
    <!-- 加 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="add">+</button>
  </div>
  
</template>

<script>
// import bus from '../eventBus'
export default {
  props: {
    // id: {
    //   // 这个意思是必选项，必须要填不传就会报错
    //   required: true,   
    //   type: Number
    // },
    // 接收到的数量值，从app到goods再到counter
    num: {
      type: Number,
      default: 1
    }
  },
  methods: {
    // 点击按钮数值加一
    add() {
      // this.num += 1 这样不对因为自定义属性不能修改,使this.num + 1就行，因为没有修改props自定义的属性
      // 因为要传给app的值是一个对象我们可以生命一个对象来接受这个对象
      // const obj = {id: this.id, value: this.num + 1}
      // // console.log(obj);
      // bus.$emit('share',obj)
      // 第二种方法插槽
      this.$emit('num-change',this.num + 1)

    },
    sub () {
      if (this.num === 0) {
        return 
      } else {
        // const obj = {id: this.id, value: this.num - 1}
      // console.log(obj);
      // bus.$emit('share',obj)}
      // 使用插槽再子传父达到传值，不用bus太复杂
      this.$emit('num-change',this.num - 1)
      }
    }
  }
}
</script>

<style lang="less" scoped>
.number-box {
  min-width: 30px;
  text-align: center;
  margin: 0 5px;
  font-size: 12px;
}

.btn-sm {
  width: 30px;
}
</style>
