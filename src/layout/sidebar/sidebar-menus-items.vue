
<script lang="ts">
import { defineComponent, PropType } from "vue";
import classNames from "classnames";
import SvgIcon from "@/components/base/svg-icon";
import { RouteRecordRaw } from "vue-router";

export default defineComponent({
  name: "SidebarMenusItems",
  components: { SvgIcon },
  props: {
    menus: Array as PropType<RouteRecordRaw[]>,
    hiddenIndex: Number,
    className: String
  },
  setup(props) {
    const childrenMenu:Array<any> =[];
    const menuIndex=0;
    const getStyle = (index: number): string => {
      const styles: Array<any> = [];
      const isHidden = props.hiddenIndex ? props.hiddenIndex > -1 && index > props.hiddenIndex : false;
      styles.push("display:" + (isHidden ? "none" : "block"));
      return styles.join(";");
    };
    return { props, classNames, getStyle,childrenMenu,menuIndex };
  },
  created(){
    // this.childrenMenu=this.props.menus[this.menuIndex].children
  },
  methods:{
    selectChildren(d:any):void{
      console.log(d)
      this.$router.push({
        path:d.path
      })
    },
    selectParent(x:any,i:number):void{
      this.menuIndex=i
      // this.childrenMenu=this.props.menus[i].children
      // this.$forceUpdate()
    },
  }
});
</script>

<template>
  <div class="row page">
    <div class="left_wrap" >
      <div class="column" v-for="(x, index) in props.menus || []" :key="x.path" @click="selectParent(x,index)">
        <div class="menu_wrap column cur_po"  v-if="x.children && x.children.length > 0">
            <i v-if="x.meta?.icon !== false" class="el-icon-dot">
              <svg-icon :name="`${x.meta?.icon || 'icon-file-fill'}`"></svg-icon>
            </i>
            <span>
              <a>{{ x.meta?.title }}</a>
            </span>
        </div>
      </div>
    </div>
    
    <!-- <el-submenu v-if="x.children && x.children.length > 0" :index="x.path" :popper-class="props.className" :class="classNames({ isMore: x.meta?.isMore })" :style="getStyle(index)">
      <template #title>
        <i v-if="x.meta?.icon !== false" class="el-icon-dot">
          <svg-icon :name="`${x.meta?.icon || 'icon-file-fill'}`"></svg-icon>
        </i>
        <span>
          <a>{{ x.meta?.title }}</a>
        </span>
      </template>
      <sidebar-menus-items :menus="x.children"></sidebar-menus-items>
    </el-submenu> -->

    <!-- <el-menu-item v-else :index="x.meta?.isNewPage ? x.path : x.path" :class="classNames({ isLink: !!x.meta?.isNewPage, isMore: x.meta?.isMore })" :style="getStyle(index)">
      <template #title>
        <a v-if="x.meta?.isNewPage" :href="`${x.meta.url}`" target="_blank" rel="opener">
          {{ x.meta.title }}
        </a>
        <a v-else>{{ x.meta?.title }}</a>
      </template>
      <i v-if="x.meta?.icon !== false" class="el-icon-dot">
        <svg-icon :name="`${x.meta?.icon || 'icon-file-fill'}`"></svg-icon>
      </i>
    </el-menu-item> -->


    <div class="children_wrap">
        <div class="child_menu" v-for="(d,i) in childrenMenu" :key="i" @click="selectChildren(d)">
          {{d.meta.title}}
        </div>
    </div>
  </div>
</template>
<style lang="less" scoped>
@import "@/assets/theme/base.less";
.page{
  height: 100%;
}
.left_wrap{
  width: 50%;
  height: 100%;
  background: @dark-bg;
}
.menu_wrap{
  width: 100%;
  align-items: center;
  background: @dark-bg;
  padding: 10px 0;
  a{
    color: #fff;
  }
  .iconfont{
    width: 18px;
    height: 18px;
    color: #fff;
    margin-bottom:5px;
  }
}
.children_wrap{
  padding: 10px;
  .child_menu{
    width: 110px;
    padding: 10px;
    border-radius: 5px;
    margin: 10px;
    font-size: 13px;
    cursor: pointer;
    text-align: center;
    background-color: #eee;
  }
}
</style>
