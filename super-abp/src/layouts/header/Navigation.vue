<template>
  <a-menu
    id="nav"
    class="menu-site"
    :mode="menuMode"
    :selected-keys="[activeMenuItem]"
    disabled-overflow
  >
    <a-menu-item key="util">
      <router-link :to="'/utils'">工具</router-link>
    </a-menu-item>
    <a-menu-item key="modules">
      <router-link :to="'/modules'">模块</router-link>
    </a-menu-item>
    <template v-if="isMobile">
      <a-menu-item key="github">
        <a
          href="https://github.com/SuperAbp"
          target="_blank"
          rel="noopener noreferrer"
        >
          Github
        </a>
      </a-menu-item>
    </template>
  </a-menu>
</template>
<script lang="ts">
import { computed, defineComponent, inject, ref, watch } from "vue";
import { useRoute } from "vue-router";
export default defineComponent({
  emits: ["langChange"],
  setup() {
    const isMobile = false;
    const menuMode = computed(() => {
      return isMobile ? "inline" : "horizontal";
    });
    const route = useRoute();
    const activeMenuItem = ref("home");
    watch(
      () => route.path,
      (pathname) => {
        const modules = pathname.split("/");
        if (
          pathname === "/docs/resources" ||
          pathname === "/docs/resources-cn"
        ) {
          activeMenuItem.value = "docs/resources";
        } else if (modules[1] === "components") {
          activeMenuItem.value = "components";
        } else if (modules[1] === "docs") {
          activeMenuItem.value = `${modules[1]}/${modules[2]}`;
        } else {
          activeMenuItem.value = "home";
        }
      },
      { immediate: true }
    );
    return {
      isMobile: isMobile,
      menuMode,
      activeMenuItem,
    };
  },
});
</script>
<style lang="less">
@import "../../theme/static/theme.less";
@import "./index.less";

#nav {
  height: 100%;
  font-size: 14px;
  font-family: Avenir, @font-family, sans-serif;
  border: 0;

  &.ant-menu-horizontal {
    border-bottom: none;

    & > .ant-menu-item,
    & > .ant-menu-submenu {
      min-width: (40px + 12px * 2);
      height: @header-height;
      padding-right: 12px;
      padding-left: 12px;
      line-height: @header-height;

      &::after {
        top: 0;
        right: 12px;
        bottom: auto;
        left: 12px;
        border-width: @menu-item-border;
      }
    }

    & .ant-menu-submenu-title .anticon {
      margin: 0;
    }

    & > .ant-menu-item-selected {
      a {
        color: @primary-color;
      }
    }
  }

  & > .ant-menu-item,
  & > .ant-menu-submenu {
    text-align: center;
  }
}

.header-link {
  color: @site-text-color;
}

.ant-menu-item-active .header-link {
  color: @primary-color;
}

// Popover menu is only used for mobile
.popover-menu {
  width: 300px;

  .ant-popover-inner-content {
    padding: 0;

    #nav {
      .ant-menu-item,
      .ant-menu-submenu {
        text-align: left;
      }

      .ant-menu-item-group-title {
        padding-left: 24px;
      }

      .ant-menu-item-group-list {
        padding: 0 16px;
      }

      .ant-menu-item,
      a {
        color: #333;
      }
    }
  }
}
</style>
