<template lang="pug">
div(@scroll='handleElementScroll')
  slot
</template>

<script>
export default {
  name: 'v-infinite-scroll',
  props: {
    loading: {type: Boolean},
    offset: {type: Number, default: 0},
    onTopScrollsToBottom: {type: Boolean, default: true}
  },
  data () {
    return {
      target: null,
      scrollPosition: 0,
      lastDirection: ''
    }
  },
  methods: {
  	handleElementScroll: function(e) {
      if (!this.target) this.target = e.target
      let currentScrollPosition = e.target.scrollTop

      if (currentScrollPosition > this.scrollPosition && currentScrollPosition + e.target.offsetHeight >= e.target.scrollHeight - this.offset) {
        this.emitEvents('bottom')
      } else if (currentScrollPosition < this.scrollPosition && currentScrollPosition <= this.offset) {
        this.emitEvents('top')
      }
      this.scrollPosition = currentScrollPosition
		},
    emitEvents (name) {
      if (!this.loading) {
        this.lastDirection = name
        this.$emit(name)
      }
    }
	},
  watch: {
    loading () {
      setTimeout(() => {
        if (this.onTopScrollsToBottom && !this.loading && this.lastDirection == 'top') {
          this.target.scrollTop = this.target.scrollHeight - this.target.offsetHeight - this.offset - 2
        }
      }, 5)
    }
  }
}
</script>

<style lang="stylus">
</style>
