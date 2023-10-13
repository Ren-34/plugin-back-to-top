<template>
  <transition name="fade">
    <div v-if="show" @click="scrollToTop" class="go-to-top" >
    </div>
  </transition>
</template>

<script>
import debounce from 'lodash.debounce'

export default {
  name: 'BackToTop',

  props: {
    threshold: {
      type: Number,
      default: 300
    }
  },

  data () {
    return {
      scrollTop: null
    }
  },

  computed: {
    show () {
      return this.scrollTop > this.threshold
    }
  },

  mounted () {
    this.scrollTop = this.getScrollTop()
    window.addEventListener('scroll', debounce(() => {
      this.scrollTop = this.getScrollTop()
    }, 100))
  },

  methods: {
    getScrollTop () {
      return window.pageYOffset
        || document.documentElement.scrollTop
        || document.body.scrollTop || 0
    },

    scrollToTop () {
      window.scrollTo({ top: 0, behavior: 'smooth' })
      this.scrollTop = 0
    }
  }
}
</script>

<style lang='stylus' scoped>
.go-to-top {
  cursor: pointer;
  position: fixed;
  right: 15px;
  bottom: 50px;
  width: 60px;
  height: 60px;
  z-index: 100;
  color: $accentColor;
  z-index: 1;

  background: url(/rocket-icon.png) center center/contain no-repeat;
  animation: .4s linear infinite alternate sway;
}

@keyframes sway {
    0% {
      bottom: 13%
    }
    100% {
      bottom: 10%
    }
}

.go-to-top:hover {
  color: lighten($accentColor, 30%);
}

@media (max-width: 959px) {
  .go-to-top {
    display: none;
  }
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.3s;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
