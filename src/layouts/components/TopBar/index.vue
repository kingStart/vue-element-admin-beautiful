<template>
  <div class="top-bar-container">
    <byui-main>
      <el-row>
        <el-col :span="5">
          <logo />
        </el-col>
        <el-col :span="19">
          <el-menu
            :background-color="variables.menuBackground"
            :text-color="variables.menuText"
            :active-text-color="variables.menuTextAcive"
            :default-active="activeMenu"
            :menu-trigger="menuTrigger"
            mode="horizontal"
          >
            <tab-item
              v-for="route in routes"
              :key="route.path"
              :base-path="route.path"
              :item="route"
            />
          </el-menu>
        </el-col>
      </el-row>
    </byui-main>
  </div>
</template>

<script>
import Logo from "@/layouts/components/Logo";
import tabItem from "./TopBarItem";
import variables from "@/styles/variables.scss";
import ByuiMain from "@/components/ByuiMain";
import { mapGetters } from "vuex";

export default {
  components: { tabItem, Logo, ByuiMain },
  data() {
    return {
      menuTrigger: "hover",
    };
  },
  computed: {
    ...mapGetters(["routes"]),
    activeMenu() {
      const route = this.$route;
      const { meta, path } = route;
      if (meta.activeMenu) {
        return meta.activeMenu;
      }
      return path;
    },
    showLogo() {
      return this.$store.state.settings.logo;
    },
    variables() {
      return variables;
    },
  },
};
</script>
<style lang="scss" scoped>
.top-bar-container {
  transition: all 0.3s;
  background: $base-color-header;

  .el-menu.el-menu--horizontal {
    border-bottom: solid 0 transparent;
  }

  ::v-deep {
    .byui-main {
      background: $base-color-header;

      .el-menu {
        &--horizontal {
          float: right;
          border-bottom: solid 0 transparent !important;
        }

        > .top-bar-item-container {
          display: inline-block;
        }
      }
    }
  }
}
</style>
