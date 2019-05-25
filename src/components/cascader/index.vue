<template>
  <transition name="area-fade">
    <div class="cascader-wrap" v-if="visible" @touchmove.prevent="onPrevent">
      <div class="opacity" @click="onCancel"></div>
      <div class="cascader">
        <div class="header">
          <i @click="onCancel"></i>
          <span>{{title || '请选择地址'}}</span>
        </div>
        <ul class="tab">
          <li v-for="item in tab" :key="item.id" :class="{active: item.active}" @click="onToggleTab(item)">{{item.text}}</li>
        </ul>
        <div class="area-wrap" @touchmove.stop="onPrevent">
          <keep-alive>
            <component :is="currentView" :province="province" :city="city" :district="district" @changeProvince="changeProvince" @changeCity="changeCity" @changeDistrict="changeDistrict" @touchmove="onPrevent"></component>
          </keep-alive>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
/* eslint-disable */
import areaData from './data.json'
import ProvinceComponent from './province'
import CityComponent from './city'
import DistrictComponent from './district'
let municipality = {
  110000: 110100,
  120000: 120100,
  500000: 500100,
  310000: 310100
}
export default {
  props: ['visible', 'title', 'columns'],
  components: {
    ProvinceComponent,
    CityComponent,
    DistrictComponent
  },
  data () {
    return {
      tab: [
        {
          id: 0,
          text: '省',
          active: true
        },
        {
          id: 1,
          text: '市',
          active: false
        },
        {
          id: 2,
          text: '区',
          active: false
        }
      ],
      province: null,
      city: null,
      district: null,
      currentView: 'ProvinceComponent',
      selectedProvince: null,
      selectedCity: null,
      selectedDistrict: null,
      selectedArea: null
    }
  },
  created () {
    this.initData()
    if (this.columns * 1 === 2) {
      // 双列联动
      this.tab.splice(2, 1)
    }
  },
  watch: {
    visible (val) {
      let body = document.body
      if (val) {
        body.style.position = 'fixed'
        this.initData()
      } else {
        body.style.position = 'static'
      }
    }
  },
  methods: {
    initData () {
      this.tab.forEach((n, i) => {
        if (i === 0) {
          n.active = true
        } else {
          n.active = false
        }
      })
      this.currentView = 'ProvinceComponent'
      this.province = areaData[86]
      this.city = []
      this.district = []
    },
    onPrevent () {
    },
    changeProvince (d) {
      this.currentView = 'CityComponent'
      this.city = []
      this.district = []
      this.selectedProvince = d
      this.city = areaData[d.code]
      if (this.columns * 1 === 2) {
        // 双列联动
        let code = municipality[d.code]
        if (code) {
          this.city = areaData[code]
        } else {
          this.city = areaData[d.code]
        }
      }
      this.tab.forEach(n => {
        if (n.text === '市') {
          n.active = true
        } else {
          n.active = false
        }
      })
    },
    changeCity (d) {
      if (this.columns * 1 === 2) {
        this.selectedCity = d
        this.selectedArea = {
          province: this.selectedProvince,
          city: this.selectedCity
        }
        this.$emit('confirm', this.selectedArea)
        this.$emit('update:visible', false)
        this.initData()
        return false
      }
      this.currentView = 'DistrictComponent'
      this.district = []
      this.selectedCity = d
      this.district = areaData[d.code]
      this.tab.forEach(n => {
        if (n.text === '区') {
          n.active = true
        } else {
          n.active = false
        }
      })
    },
    changeDistrict (d) {
      this.selectedDistrict = d
      this.selectedArea = {
        province: this.selectedProvince,
        city: this.selectedCity,
        district: this.selectedDistrict
      }
      this.$emit('confirm', this.selectedArea)
      this.$emit('update:visible', false)
      this.initData()
    },
    onToggleTab (item) {
      this.tab.forEach(n => {
        if (n.id === item.id) {
          n.active = true
        } else {
          n.active = false
        }
      })
      let text = item.text
      if (text === '省') {
        this.currentView = 'ProvinceComponent'
      } else if (text === '市') {
        this.currentView = 'CityComponent'
      } else {
        this.currentView = 'DistrictComponent'
      }
    },
    onCancel () {
      this.$emit('update:visible', false)
      this.$emit('cancel')
    }
  }
}
</script>
<style scoped>
ul{
  margin: 0;
  padding: 0;
  list-style: none;
}
.opacity{
  width: 100%;
  height: 100%;
  position: fixed;
  left: 0;
  top: 0;
  background: rgba(0, 0, 0, 0.6);
  -webkit-tap-highlight-color: transparent;
}
.cascader-wrap{
  width: 100%;
  height: 100%;
  position: fixed;
  left: 0;
  top: 0;
  font-size: 16px;
  color: #444;
}
.area-wrap{
  flex: 1;
  overflow: auto;
  -webkit-overflow-scrolling: touch;
}
.cascader{
  width: 100vw;
  height: 55vh;
  background: #fff;
  position: absolute;
  left: 0;
  bottom: 0;
  z-index: 2;
  display: flex;
  flex-direction: column;
}
.tab{
  display: flex;
  height: 35px;
  border-bottom: 1px solid #ddd;
  -webkit-tap-highlight-color: transparent;
}
.tab li{
  height: 100%;
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #666;
}
.tab li.active{
  color: #29d1b4;
  border-bottom: 2px solid #29d1b4;
}
.header{
  height: 45px;
  display: flex;
  padding: 0px 20px;
  box-sizing: border-box;
}
.header i{
  width: 40px;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
.header i:after{
  width: 10px;
  height: 10px;
  display: block;
  content: "";
  border: 2px solid #444;
  border-right: none;
  border-bottom: none;
  transform: rotate(-45deg);
}
.header span{
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 18px;
}
.area-fade-enter-active, .area-fade-leave-active{
  transition: all .5s;
}
.area-fade-enter, .area-fade-leave-to{
  opacity: 0;
  transform: translateY(20px);
}
</style>
