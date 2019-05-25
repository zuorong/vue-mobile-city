<template>
  <div class="area" ref="areaRef">
    <div :class="{cell: true, active: false}" v-for="(item, index) in city" :id="index" :key="index" @click="onSelectCity($event)">{{item}}</div>
  </div>
</template>

<script>
/* eslint-disable */
export default {
  props: ['city'],
  methods: {
    onSelectCity (evt) {
      let target = evt.target
      if (!target.classList.contains('cell')) {
        return false
      }
      let node = this.$refs.areaRef.querySelector('.active')
      if (node) {
        node.classList.remove('active')
      }
      target.classList.add('active')
      this.$emit('changeCity', {
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
