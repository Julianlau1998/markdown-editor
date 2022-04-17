<template>
  <div>
    <topNav @fileInput="fileInput" @share="shareFile" />
    <Editor :share="share" />
  </div>
</template>

<script>
import Editor from '@/components/Editor.vue'
import topNav from '@/components/TopNav.vue'

export default {
  name: 'Home',
  components: {
    Editor,
    topNav
  },
  data () {
    return {
      inputFile: '',
      share: false
    }
  },
  methods: {
    fileInput (file) {
      this.$store.state.inputFile = file
    },
    shareFile () {
      this.share = true
      setTimeout(() => {
        this.share = false
      }, 300)
    }
  }
}
</script>
