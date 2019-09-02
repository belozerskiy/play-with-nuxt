<script>
export default {
  name: "FetchApi",
  props: {
    method: {
      type: String,
      required: true
    },
    endpoint: {
      type: String,
      required: true
    },
    body: {
      type: Object,
      default: () => ({})
    },
    params: {
      type: Object,
      default: () => ({})
    },
    ssr: {
      type: Boolean,
      default: true,
    },
    client: {
      type: Boolean,
      default: true,
    }
  },
  data() {
    return {
      data: null,
      error: null,
      loading: false,
    }
  },
  async serverPrefetch() {
    if(this.ssr) { await this.fetch(); console.log('Server') }
  },
  async mounted() {
    const cachedData = JSON.parse(this.$el.getAttribute('v-data'))
    if(cachedData) {
      this.data = cachedData
      this.$el.removeAttribute('v-data')
    } else {
      this.data = await this.fetch();
    }
  },
  render(createElement) {
    const { data, error, loading } = this
    return  createElement('div', { attrs: { 'v-data': JSON.stringify(data) } }, this.$scopedSlots.default({ data, error, loading }))
  },
  methods: {
    async fetch() {
      this.loading = true
      const { method, endpoint, params } = this
      try {
        if(method.toUpperCase() === 'GET') {
          this.data = await this.$axios.$get(endpoint, { params })
        }
        if(method.toUpperCase() === 'POST') {
          this.data = await this.$axios.$post(endpoint, { data: params })
        }
      } catch(RequestException) {
        this.error = RequestException
        console.log(RequestException)
      } finally {
        this.loading = false
      } 
    }
  }
};
</script>
