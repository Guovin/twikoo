<template>
  <div class="tk-footer">
    <!-- Powered by <a href="https://twikoo.js.org" target="_blank">Twikoo</a>
    v{{ version }} -->
  </div>
</template>

<script>
import { version } from '../../../package.json'
import { call, getUrl } from '../../js/utils'

export default {
  data () {
    return {
      version,
      counter: {}
    }
  },
  methods: {
    async getCounter () {
      const url = getUrl(this.$twikoo.path)
      const counterEls = document.getElementsByClassName('twikoo_visitors_counter')
      if (!counterEls) return
      let counterEl, result
      const posts = Object.values(document.getElementsByClassName('post-title-link'))
      if (posts && posts.length > 0) {
        posts.forEach(async (item, index) => {
          result = await this.getCounterResult(item.attributes.href.value, item.href, item.innerHtml)
          this.counter = result.result
          if (this.counter.time || this.counter.time === 0) {
            counterEls[index].innerHTML = this.counter.time
          }
        })
      } else {
        counterEl = counterEls[0]
        result = await this.getCounterResult(url, window.location.href, document.title)
        this.counter = result.result
        if (this.counter.time || this.counter.time === 0) {
          counterEl.innerHTML = this.counter.time
        }
      }
    },
    async getCounterResult (url, href, title) {
      const result = await call(this.$tcb, 'COUNTER_GET', {
        url,
        href,
        title
      })
      return result
    }
  },
  mounted () {
    this.getCounter()
  }
}
</script>

<style scoped>
.tk-footer {
  width: 100%;
  text-align: end;
  font-size: 0.75em;
  color: #999999;
  margin-top: 1em;
}
</style>
