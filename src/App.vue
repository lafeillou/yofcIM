<template>
  <div class="layout">
    <Layout>
      <Sider ref="side1" hide-trigger collapsible :collapsed-width="78" v-model="isCollapsed">
        <Menu active-name="1-1" theme="dark" width="auto" :class="menuitemClasses">
          <MenuItem name="1-1">
            <Icon type="md-map"></Icon>
            <span>警务地图</span>
          </MenuItem>
          <MenuItem name="1-2">
            <Icon type="ios-medal"></Icon>
            <span>警卫保障</span>
          </MenuItem>
          <MenuItem name="1-3">
            <Icon type="md-grid"></Icon>
            <span>应用管理

            </span>
          </MenuItem>
        </Menu>
      </Sider>
      <Layout>
        <Header :style="{padding: 0}" class="layout-header-bar">
          <Icon
            @click.native="collapsedSider"
            :class="rotateIcon"
            :style="{margin: '0 20px'}"
            type="md-menu"
            size="24"
          ></Icon>
        </Header>
        <Content
          :style="{margin: '0px', borderTop:'1px solid #ddd',background: '#fff', minHeight: '260px',width:'100%',height:'100%'}"
        >
          <baidu-map
            class="bm-view"
            :center="center"
            :zoom="zoom"
            :scroll-wheel-zoom="true"
            @ready="handler"
          >
            <bm-navigation anchor="BMAP_ANCHOR_TOP_RIGHT"></bm-navigation>
            <bm-map-type
              :map-types="['BMAP_NORMAL_MAP', 'BMAP_HYBRID_MAP']"
              anchor="BMAP_ANCHOR_TOP_LEFT"
            ></bm-map-type>
            <bm-panorama anchor="BMAP_ANCHOR_BOTTOM_RIGHT"></bm-panorama>
            <bm-overview-map
              anchor="BMAP_ANCHOR_BOTTOM_LEFT"
              :isOpen="true"
              :size="{width:'500',height:'280'}"
            ></bm-overview-map>
            <!-- <bm-tile
              :isTransparentPng="true"
              tileUrlTemplate="//developer.baidu.com/map/jsdemo/demo/tiles/{Z}/tile{X}_{Y}.png"
            ></bm-tile>-->
            <bm-marker
              v-if="isShowAllPolice"
              v-for="p in peoples"
              :key="p.id"
              :position="{lng: p.position.lng,lat:p.position.lat}"
              :dragging="true"
              animation="BMAP_ANIMATION_BOUNCE"
              :icon="{url: p.iconUrl, size: {width: 70, height: 84}}"
              @click="clickHandler(p)"
            ></bm-marker>

            <bm-marker
              v-if="isShowAllCar"
              v-for="car in cars"
              :key="car.id"
              :position="{lng: car.position.lng,lat:car.position.lat}"
              :dragging="true"
              animation="BMAP_ANIMATION_BOUNCE"
              :icon="{url: car.iconUrl, size: {width: 62, height: 122}}"
            ></bm-marker>
             <!-- @close="infoWindowClose" @open="infoWindowOpen" -->
            <bm-info-window :position="{lng: position.lng, lat: position.lat}" title="" :show="infoWindow.show" @close="infoWindowClose"  @open="infoWindowOpen"> 
                <p style="margin-bottom: 20px;" @click="audioCall()">音频通话</p>
                <p @click="videoCall()">视频通话</p>
            </bm-info-window>
          </baidu-map>

          <div style="position:fixed;top:120px;left:210px;">
            <Card title="人员车辆定位" icon="md-locate" :padding="0" shadow style="width: 200px;">
              <CellGroup>
                <Cell title="人员" label="各级各警种人员">
                  <Icon type="ios-people"></Icon>人员
                  <iSwitch v-model="isShowAllPolice" slot="extra"/>
                </Cell>
                <Cell title="车辆" label="各用途车辆">
                  <Icon type="md-car"></Icon>车辆
                  <iSwitch v-model="isShowAllCar" slot="extra"/>
                </Cell>
                <!-- <Cell title="Display label content" label="label content"/>
                <Cell title="Display right content" extra="details"/>
                <Cell title="Link" extra="details" to="/components/button"/>
                <Cell title="Open link in new window" to="/components/button" target="_blank"/>
                <Cell title="Disabled" disabled/>
                <Cell title="Selected" selected/>
                <Cell title="With Badge" to="/components/badge">
                  <Badge :count="10" slot="extra"/>
                </Cell>
                <Cell title="With Switch">
                  <Switch slot="extra"/>
                </Cell>-->
              </CellGroup>
            </Card>
          </div>
        </Content>
         <div class="incomingCall">
            <incoming-call></incoming-call>
        </div>
      </Layout>
    </Layout>
  </div>
</template>

<script lang="ts">
import { Prop, Vue } from 'vue-property-decorator'
import peopleData from './data/people'
import carData from './data/car'
import Component from 'vue-class-component'
import IncomingCall from '@/components/IncomingCall.vue'

// import BaiduMap from "vue-baidu-map/components/map/Map.vue";

@Component({
  components: { IncomingCall }
})
export default class App extends Vue {
  // @Prop() private msg!: string;
  @Prop() private size!: any

  infoWindow: any = {
    show: false,
    contents:
      'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.'
  }
  position: any = {
    lng: 0,
    lat: 0
  }
  isCollapsed: Boolean = false
  center: any = { lng: 0, lat: 0 }
  zoom: Number = 3

  // 全部警员
  isShowAllPolice: Boolean = false
  isShowAllCar: Boolean = false
  peoples: any[] = peopleData
  cars: any = carData
  // computed
  get rotateIcon(): string[] {
    return ['menu-icon', this.isCollapsed ? 'rotate-icon' : '']
  }
  get menuitemClasses(): string[] {
    return ['menu-item', this.isCollapsed ? 'collapsed-menu' : '']
  }

  // components: any = {
  //   BaiduMap
  // };
  // methods
  collapsedSider(): void {
    this.isCollapsed = !this.isCollapsed
  }

  // handler({ BMap, map }): void
  handler(): void {
    // console.log(BMap, map);
    // 114.217379,30.776258
    // 114.219919,30.773363
    this.center.lng = 114.219919
    this.center.lat = 30.773363
    this.zoom = 19
  }

  // 显示所有警员
  showAllPolice(): void {
    // alert("123");
    this.isShowAllPolice = !this.isShowAllPolice
  }

  // 显示所有车辆
  showAllCar(): void {
    // alert(123);
    this.isShowAllCar = !this.isShowAllCar
  }

  //弹框显示
  clickHandler(p) {
    this.position.lng = p.position.lng
    this.position.lat = p.position.lat + 0.0001
    // alert(this.position.lat)
    this.infoWindow.show = true
  }

  //弹框关闭
  infoWindowClose() {
    this.infoWindow.show = false
  }

  //弹框打开
  infoWindowOpen() {
    this.infoWindow.show = true
  }

  //音频通话
  audioCall() {}

  //视频通话
  videoCall() {}
}
</script>
<style>
html,
body {
  height: 100%;
  width: 100%;
}
</style>
<style scoped>
.bm-view {
  width: 100%;
  height: 100%;
}
.layout {
  /* border: 1px solid #d7dde4; */
  height: 100%;
  background: #f5f7f9;
  position: relative;
  /* border-radius: 4px; */
  overflow: hidden;
}

.ivu-layout {
  height: 100%;
}
.layout-header-bar {
  background: #fff;
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.1);
}
.layout-logo-left {
  width: 90%;
  height: 30px;
  background: #5b6270;
  border-radius: 3px;
  margin: 15px auto;
}
.menu-icon {
  transition: all 0.3s;
}
.rotate-icon {
  transform: rotate(-90deg);
}
.menu-item span {
  display: inline-block;
  overflow: hidden;
  width: 69px;
  text-overflow: ellipsis;
  white-space: nowrap;
  vertical-align: bottom;
  transition: width 0.2s ease 0.2s;
}
.menu-item i {
  transform: translateX(0px);
  transition: font-size 0.2s ease, transform 0.2s ease;
  vertical-align: middle;
  font-size: 16px;
}
.collapsed-menu span {
  width: 0px;
  transition: width 0.2s ease;
}
.collapsed-menu i {
  transform: translateX(5px);
  transition: font-size 0.2s ease 0.2s, transform 0.2s ease 0.2s;
  vertical-align: middle;
  font-size: 22px;
}
.incomingCall {
  position: absolute;
  bottom: 70px;
  right: 20px;
  padding: 5px 10px;
  border: 1px solid #ddd;
  background-color: white;
}
</style>
