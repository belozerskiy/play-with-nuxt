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
    if (this.ssr) { await this.fetch(); console.log('Server') }
  },
  render(h) {
    const { data, error, loading } = this

    if (!process.server) {
      const vnode = h('div', [])
      vnode.asyncFactory = {}
      vnode.isComment = true
      return vnode
    } else {
      return h('div', this.$scopedSlots.default({ data, error, loading }))
    }
  },

  methods: {
    async fetch() {
      this.loading = true
      const { method, endpoint, params } = this

      let options = method.toUpperCase() === 'GET' ? { params } : { data: params }
      
      try {
        this.data = await this.$axios[`$${method.toLowerCase()}`](endpoint, { data: params })
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
