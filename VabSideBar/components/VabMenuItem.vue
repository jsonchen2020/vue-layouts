<template>
  <el-menu-item :index="handlePath(routeChildren.path)" @click="handleLink">
    <!-- {{ routeChildren }} -->
    <vab-icon
      v-if="routeChildren.meta.icon"
      :icon="['fas', routeChildren.meta.icon]"
      :class="[
        { aa: routeChildren.meta.icon == 'circle' || routeChildren == '' },
        { bb: routeChildren == '' || !routeChildren },
        'vab-fas-icon',
      ]"
    />
    <vab-remix-icon
      v-if="routeChildren.meta.remixIcon"
      :icon-class="routeChildren.meta.remixIcon"
      class="vab-remix-icon"
    />
    <span>{{ routeChildren.meta.title }}</span>
    <el-tag
      v-if="routeChildren.meta && routeChildren.meta.badge"
      type="danger"
      effect="dark"
    >
      {{ routeChildren.meta.badge }}
    </el-tag>
  </el-menu-item>
</template>

<script>
import { isExternal } from '@/utils/validate'
import path from 'path'

export default {
  name: 'VabMenuItem',
  props: {
    routeChildren: {
      type: Object,
      default() {
        return null
      },
    },
    item: {
      type: Object,
      default() {
        return null
      },
    },
    fullPath: {
      type: String,
      default: '',
    },
  },
  methods: {
    handlePath(routePath) {
      if (isExternal(routePath)) {
        return routePath
      }
      if (isExternal(this.fullPath)) {
        return this.fullPath
      }
      // console.log(this.fullPath, routePath, 'ccc')
      return path.resolve(this.fullPath, routePath)
    },
    handleLink() {
      const routePath = this.routeChildren.path
      const target = this.routeChildren.meta.target

      if (target === '_blank') {
        if (isExternal(routePath)) {
          window.open(routePath)
        } else if (isExternal(this.fullPath)) {
          window.open(this.fullPath)
        } else if (
          this.$route.path !== path.resolve(this.fullPath, routePath)
        ) {
          let routeData = this.$router.resolve(
            path.resolve(this.fullPath, routePath)
          )
          window.open(routeData.href)
        }
      } else {
        if (isExternal(routePath)) {
          window.location.href = routePath
        } else if (isExternal(this.fullPath)) {
          window.location.href = this.fullPath
        } else if (
          this.$route.path !== path.resolve(this.fullPath, routePath)
        ) {
          this.$router.push(path.resolve(this.fullPath, routePath))
        }
      }
    },
  },
}
</script>
<style>
.aa {
  font-size: 5px;
  margin-right: 15px;
}
.bb {
  width: 100px !important;
}
.el-scrollbar__view>.el-menu--collapse div>.el-submenu .el-submenu__icon-arrow.el-icon-arrow-right {
   display: none !important;
}
</style>
