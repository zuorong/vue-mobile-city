<template>
  <div class="area" ref="areaRef" @click="onSelectProvince($event)">
    <div :class="{cell: true, active: false}" v-for="(item, index) in province" :id="index" :key="index">{{item}}</div>
  </div>
</template>

<script>
/* eslint-disable */
export default {
  props: ['province'],
  methods: {
    onSelectProvince (evt) {
      let target = evt.target
      if (!target.classList.contains('cell')) {
        return false
      }
      let node = this.$refs.areaRef.querySelector('.active')
      if (node) {
        node.classList.remove('active')
      }
      target.classList.add('active')
      this.$emit('changeProvince', {
        code: target.id,
        text: target.textContent
      })
    }
  }
}
</script>
<style scoped>
  @import './index.css';
</style>
