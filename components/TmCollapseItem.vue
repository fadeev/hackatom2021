<template>
  <div
    :class="{
      'open-item': maxHeight != '0px',
      disabled: disabled,
      light: light,
      'mobile-light': mobileLight,
      'hide-border': hideBorder,
    }"
    class="tm-collapse-item"
    @mouseover="mouseover"
    @mouseout="mouseout"
  >
    <button
      class="tm-collapse-item--header"
      variant="text"
      @click="toggleContent"
    >
      <span
        v-if="!notArrow"
        class="tm-collapse-item--icon-header"
        :class="[iconTop && '_top', mobileBottom && '_mobile-bottom']"
      />
      <slot name="header" />
    </button>
    <div ref="content" :style="styleContent" class="tm-collapse-item--content">
      <div class="con-content--item">
        <slot />
      </div>
    </div>
  </div>
</template>

<script>
// import IconPlus16 from '~/components/icons/IconPlus16.vue'

export default {
  components: {
    // IconPlus16,
  },
  props: {
    open: {
      default: false,
      type: Boolean,
    },
    disabled: {
      default: false,
      type: Boolean,
    },
    hideBorder: {
      default: false,
      type: Boolean,
    },
    light: {
      default: false,
      type: Boolean,
    },
    mobileLight: {
      default: false,
      type: Boolean,
    },
    mobileBottom: {
      default: false,
      type: Boolean,
    },
    notArrow: {
      default: false,
      type: Boolean,
    },
    iconTop: {
      default: false,
      type: Boolean,
    },
    iconArrow: {
      default: 'keyboard_arrow_down',
      type: String,
    },
    iconPack: {
      default: 'material-icons',
      type: String,
    },
    sst: {
      default: false,
      type: Boolean,
    },
  },
  data: () => ({
    maxHeight: '0px',
    // only used for sst
    dataReady: false,
  }),
  computed: {
    accordion() {
      return this.$parent.accordion
    },
    openHover() {
      return this.$parent.openHover
    },
    styleContent() {
      return {
        maxHeight: this.maxHeight,
      }
    },
  },
  watch: {
    maxHeight() {
      this.$parent.emitChange()
    },
    ready(newVal, oldVal) {
      if (oldVal !== newVal && newVal) {
        this.initMaxHeight()
      }
    },
  },
  mounted() {
    window.addEventListener('resize', this.changeHeight)
    const maxHeightx = this.$refs.content.scrollHeight
    if (this.open) {
      this.maxHeight = `${maxHeightx}px`
    }
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.changeHeight)
  },
  methods: {
    changeHeight() {
      const maxHeightx = this.$refs.content.scrollHeight
      if (this.maxHeight !== '0px') {
        this.maxHeight = `${maxHeightx}px`
      }
    },
    toggleContent() {
      if (this.openHover || this.disabled) return
      if (this.accordion) {
        this.$parent.closeAllItems(this.$el)
      }
      if (this.sst && !this.dataReady) {
        this.$emit('fetch', {
          done: () => {
            this.initMaxHeight()
            this.dataReady = true
          },
        })
      } else {
        this.initMaxHeight()
      }
    },
    initMaxHeight() {
      const maxHeightx = this.$refs.content.scrollHeight
      if (this.maxHeight === '0px') {
        this.maxHeight = `${maxHeightx}px`
      } else {
        this.maxHeight = `0px`
      }
    },
    mouseover() {
      if (this.disabled) return
      const maxHeightx = this.$refs.content.scrollHeight
      if (this.openHover) {
        this.maxHeight = `${maxHeightx}px`
      }
    },
    mouseout() {
      if (this.openHover) {
        this.maxHeight = `0px`
      }
    },
  },
}
</script>
