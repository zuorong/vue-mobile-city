<template>
  <div class="home" style="height: 2000px;">
    <button @click="openCascader">open city</button>
    <Cascader
      :visible.sync="isShow"
      columns="2"
      @confirm="onConfirm"
      @cancel="onCancel"
    />
  </div>
</template>

<script>
import Cascader from '../components/cascader'
export default {
  name: 'home',
  components: {
    Cascader
  },
  data () {
    return {
      isShow: false
    }
  },
  methods: {
    openCascader () {
      this.isShow = true
    },
    onConfirm (d) {
      console.log(d)
    },
    onCancel () {
      console.log('cancel')
    }
  }
}
</script>
