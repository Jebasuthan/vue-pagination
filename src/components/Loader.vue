<template>
  <div tabindex="0" class="loader-overlay is-active" :class="{ 'is-full-page': isFullPage }" aria-label="Loading" v-show="loading">
    <div class="loader-background"></div>
    <svg viewBox="0 0 38 38" xmlns="http://www.w3.org/2000/svg" :width="width" :height="height" :stroke="color">
      <g fill="none" fill-rule="evenodd">
        <g transform="translate(1 1)" stroke-width="2">
          <circle stroke-opacity=".25" cx="18" cy="18" r="18"/>
          <path d="M36 18c0-9.94-8.06-18-18-18">
            <animateTransform
              attributeName="transform"
              type="rotate"
              from="0 18 18"
              to="360 18 18"
              dur="0.8s"
              repeatCount="indefinite"/>
          </path>
        </g>
      </g>
    </svg>
  </div>
</template>

<script>
export default {
  name: 'loader',
  props: {
    loading: {
      type: Boolean,
      default: false
    },
    isActive: {
      type: Boolean,
      default: false
    },
    isFullPage: {
      type: Boolean,
      default: true
    },
    color: {
      type: String,
      default: '#39C6F2'
    },
    height: {
      type: Number,
      default: 64
    },
    width: {
      type: Number,
      default: 64
    }
  }
}
</script>
<style lang="scss" scoped>
  .loader-overlay {
    bottom: 0;
    left: 0;
    position: absolute;
    right: 0;
    top: 0;
    align-items: center;
    display: none;
    justify-content: center;
    overflow: hidden;
    z-index: 9999;
    &.is-active {
      display: flex;
    }
    &.is-full-page {
      z-index: 9999;
      position: fixed;
    }
    .loader-background {
      bottom: 0;
      left: 0;
      position: absolute;
      right: 0;
      top: 0;
      background: white;
      opacity: 0.5;
    }
  }
</style>
