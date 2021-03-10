<template>
  <div>
    <template v-if="!collapse">
      <div>
        <el-submenu
          ref="subMenu"
          :index="handlePath(item.path)"
          :popper-append-to-body="true"
        >
          <template slot="title">
            <vab-icon
              v-if="item.meta && item.meta.icon"
              :icon="['fas', item.meta.icon]"
              class="vab-fas-icon"
            />
            <vab-remix-icon
              v-if="item.meta && item.meta.remixIcon"
              :icon-class="item.meta.remixIcon"
              class="vab-remix-icon"
            />
            <span>{{ item.meta.title }}</span>
          </template>

          <slot />
        </el-submenu>
      </div>
    </template>
    <template v-else>
      <div style="overflow-y: scroll">
        <el-submenu
          ref="subMenu"
          :index="handlePath(item.path)"
          :popper-append-to-body="false"
        >
          <div
            v-if="item.level == 1"
            style="
              font-size: 18px;
              font-family: Microsoft YaHei;
              font-weight: bold;
              color: #ffffff;
              position: absolute;
              top: 20px;
              left: 20px;
            "
          >
            企货宝·智慧云平台
          </div>
          <template slot="title" v-if="item.level == 1">
            <template v-if="item.meta && item.meta.icon">
              <div>
                <div>
                  <vab-icon
                    style="line-height: 0; transform: translateY(-10px)"
                    :icon="['fas', item.meta.icon]"
                    class="vab-fas-icon"
                  />
                  <div
                    style="line-height: 0; transform: translate(-6px, -14px)"
                  >
                    {{ item.meta.title }}
                  </div>
                </div>
              </div>
            </template>
            <template v-if="item.meta && item.meta.remixIcon">
              <div>
                <vab-remix-icon
                  :icon-class="item.meta.remixIcon"
                  class="vab-remix-icon"
                />
                <span>{{ item.meta.title }}</span>
              </div>
            </template>
          </template>
          <template slot="title" v-else>
            <div>
              <vab-icon
                v-if="item.meta && item.meta.icon"
                :icon="['fas', item.meta.icon]"
                class="vab-fas-icon"
              />
              <vab-remix-icon
                v-if="item.meta && item.meta.remixIcon"
                :icon-class="item.meta.remixIcon"
                class="vab-remix-icon"
              />
              <span style="float: right">
                <i
                  style="font-size: 12px; transform: translateY(3px)"
                  class="el-icon-arrow-right"
                ></i>
              </span>
              <span>{{ item.meta.title }}</span>
            </div>
          </template>
          <slot style="overflow-y: scroll" />
        </el-submenu>
      </div>
    </template>
  </div>
</template>






<script>
import { isExternal } from '@/utils/validate'
import path from 'path'
import Vue from 'vue'
import { mapGetters } from 'vuex'
import VabIcon from 'vue-my-icon'
import { dependencies, repository } from '/package.json'

export default {
  created() {
    
  },
  name: 'VabSubmenu',
  data() {
    return {}
  },
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
      return path.resolve(this.fullPath, routePath)
    },
  },
  watch: {},
}
</script>

<style>
.titbox_pt {
  font-size: 18px !important;
  font-family: Microsoft YaHei !important;
  font-weight: bold !important;
  color: #ffffff !important;
  text-align: center !important;
  position: absolute;
}
</style>
