<template>
  <el-menu-item :index="handlePath(routeChildren.path)" @click="handleLink">
    <template v-if="collapse&&item.level == 1">
      <div>
        <template v-if="item.level == 1">
          <vab-icon style="line-height: 0; transform: translateY(-10px)" :icon="['fas', item.meta.icon]"
            class="vab-fas-icon" />
          <div style="line-height: 0; transform: translate(-6px, -14px)">
            {{ item.meta.title }}
          </div>
          <vab-remix-icon v-if="routeChildren.meta.remixIcon" :icon-class="routeChildren.meta.remixIcon"
            class="vab-remix-icon" />
          <el-tag v-if="routeChildren.meta && routeChildren.meta.badge" type="danger" effect="dark">
            {{ item.meta.badge }}
          </el-tag>
        </template>
      </div>
    </template>
    <template v-else>
      <template v-if="item.level == 1">
        <div>
          <vab-icon v-if="item.meta.icon" :icon="['fas', item.meta.icon]" class="vab-fas-icon" />
          <vab-remix-icon v-if="item.meta.remixIcon" :icon-class="item.meta.remixIcon" class="vab-remix-icon" />
          <span>{{ item.meta.title }}</span>
          <el-tag v-if="item.meta && item.meta.badge" type="danger" effect="dark">
            {{ item.meta.badge }}
          </el-tag>
        </div>
      </template>
      <template v-else>
        <div>
          <vab-icon v-if="routeChildren.meta.icon" :icon="['fas', routeChildren.meta.icon]" :class="[
        { aa: routeChildren.meta.icon == 'circle' || routeChildren == '' },
        { bb: routeChildren == '' || !routeChildren },
        'vab-fas-icon',
      ]" />
          <vab-remix-icon v-if="routeChildren.meta.remixIcon" :icon-class="routeChildren.meta.remixIcon"
            class="vab-remix-icon" />
          <span>{{ routeChildren.meta.title }}</span>
          <el-tag v-if="routeChildren.meta && routeChildren.meta.badge" type="danger" effect="dark">
            {{ routeChildren.meta.badge }}
          </el-tag>
        </div>
      </template>
    </template>
  </el-menu-item>
</template>

<script>
  import { isExternal } from '@/utils/validate'
  import path from 'path'
  import Vue from 'vue'
  import { mapGetters } from 'vuex'
  import VabIcon from 'vue-my-icon'
  import { dependencies, repository } from '/package.json'

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
    computed: {
      ...mapGetters({
        collapse: 'settings/collapse',
      }),
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