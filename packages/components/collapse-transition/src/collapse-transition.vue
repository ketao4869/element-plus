<template>
  <transition name="el-collapse-transition" v-on="on">
    <slot></slot>
  </transition>
</template>
<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'ElCollapseTransition',
  setup() {
    return {
      on: {
        beforeEnter(el) {
          if (!el.dataset) el.dataset = {}

          el.dataset.oldPaddingTop = el.style.paddingTop
          el.dataset.oldPaddingBottom = el.style.paddingBottom

          el.style.maxHeight = 0
          el.style.paddingTop = 0
          el.style.paddingBottom = 0
        },

        enter(el) {
          el.dataset.oldOverflow = el.style.overflow
          if (el.scrollHeight !== 0) {
            el.style.maxHeight = `${el.scrollHeight}px`
            el.style.paddingTop = el.dataset.oldPaddingTop
            el.style.paddingBottom = el.dataset.oldPaddingBottom
          } else {
            el.style.maxHeight = 0
            el.style.paddingTop = el.dataset.oldPaddingTop
            el.style.paddingBottom = el.dataset.oldPaddingBottom
          }

          el.style.overflow = 'hidden'
        },

        afterEnter(el) {
          el.style.maxHeight = ''
          el.style.overflow = el.dataset.oldOverflow
        },

        beforeLeave(el) {
          if (!el.dataset) el.dataset = {}
          el.dataset.oldPaddingTop = el.style.paddingTop
          el.dataset.oldPaddingBottom = el.style.paddingBottom
          el.dataset.oldOverflow = el.style.overflow

          el.style.maxHeight = `${el.scrollHeight}px`
          el.style.overflow = 'hidden'
        },

        leave(el) {
          if (el.scrollHeight !== 0) {
            el.style.maxHeight = 0
            el.style.paddingTop = 0
            el.style.paddingBottom = 0
          }
        },

        afterLeave(el) {
          el.style.maxHeight = ''
          el.style.overflow = el.dataset.oldOverflow
          el.style.paddingTop = el.dataset.oldPaddingTop
          el.style.paddingBottom = el.dataset.oldPaddingBottom
        },
      },
    }
  },
})
</script>
