<template>
  <div class="area" ref="areaRef">
    <div :class="{cell: true, active: false}" v-for="(item, index) in district" :id="index" :key="index" @click="onSelectDistrict($event)">{{item}}</div>
  </div>
</template>

<script>
/* eslint-disable */
export default {
  props: ['district'],
  methods: {
    onSelectDistrict (evt) {
      let target = evt.target
      if (!target.classList.contains('cell')) {
        return false
      }
      let node = this.$refs.areaRef.querySelector('.active')
      if (node) {
        node.classList.remove('active')
      }
      target.classList.add('active')
      this.$emit('changeDistrict', {
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
