<template>
  <a-layout class="app-wapper">
    <!-- 侧边栏 -->
    <a-layout-sider
      class="app-sider"
      v-model="collapsed"
      :trigger="null"
      collapsible
    >
      <div class="logo">中台管理</div>
      <Sider />
    </a-layout-sider>
    <a-layout>
      <!-- 导航条 -->
      <a-layout-header class="app-header">
        <a-row type="flex" justify="space-between">
          <a-col>
            <a-icon
              class="trigger"
              :type="collapsed ? 'menu-unfold' : 'menu-fold'"
              @click="() => (collapsed = !collapsed)"
            />
          </a-col>
          <a-col>
            <Header class="header" />
          </a-col>
        </a-row>
      </a-layout-header>
      <!-- 内容 -->
      <a-layout-content class="app-content">
        <a-breadcrumb class="app-breadcrumb" :routes="routes">
          <template
            slot="itemRender"
            slot-scope="{ route, params, routes, paths }"
          >
            <span v-if="routes.length === 1">
              {{ route.meta.title }}
            </span>
            <router-link v-else :to="`${route.path}`">
              {{ route.meta.title }}
            </router-link>
          </template>
        </a-breadcrumb>
        <div class="spin" v-if="spinning">
          <a-spin :spinning="spinning" wrapperClassName="spinSon" size="large">
          </a-spin>
        </div>
        <router-view></router-view>
      </a-layout-content>
    </a-layout>
  </a-layout>
</template>
<script>
import Header from "./components/Header/index.vue";
import Sider from "./components/Sider/index.vue";
// import Sider from "@/components/MenuList";
export default {
  components: {
    Header,
    Sider,
  },
  created() {
    this.routes = this.$route.matched.filter((item) => item.meta.title);
  },
  data() {
    return {
      collapsed: false,
      routes: [],
    };
  },
  computed: {
    spinning() {
      console.log(this.$store.state.spinning);
      return this.$store.state.spinning;
    },
  },
  watch: {
    // 监听路由变化
    $route(e) {
      this.routes = e.matched.filter((items) => items.meta.title);
    },
  },
};
</script>

<style lang="less" scoped>
.app-wapper {
  height: 100vh;
  .app-sider {
    .logo {
      line-height: 46px;
      text-align: center;
      color: #fff;
      height: 46px;
      background: rgba(255, 255, 255, 0.3);
    }
    left: 0;
    height: 100vh;
    overflow: auto;
  }
  .app-header {
    padding: 0;
    background: #fff;
    .header {
      padding: 0 0.32rem;
    }
    .trigger {
      font-size: 18px;
      line-height: 64px;
      padding: 0 24px;
      cursor: pointer;
      transform: color 0.3s;
    }
  }
  .app-content {
    height: calc(100% - 102px);
    overflow: hidden;
    .app-breadcrumb {
      height: 32px;
      line-height: 32px;
      padding: 0 0.16rem;
      // padding: 0.16rem 0.16rem 0 0.16rem;
    }
    .spin {
      position: absolute;
      width: 100%;
      z-index: 100;
      top: 0;
      left: 0;
      height: 100%;
      display: flex;
      align-items: center;
      justify-content: center;
      background: #a7a3a3;
      background: rgba(0, 0, 0, 0.5);
      .ant-spin-container {
        height: 100% !important;
      }
      .spinSon {
        height: 100% !important;
      }
    }
  }
}
</style>
