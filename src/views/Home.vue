<template>
  <div>
    <topNav @fileInput="fileInput" @share="share=true" />
    <Editor :inputFile="inputFile" :share="share" />
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
      this.inputFile = file
    }
  }
}
</script>
