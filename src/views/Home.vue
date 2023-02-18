<template>
  <div>
    <topNav @fileInput="fileInput" @share="shareFile" />
    <Editor :share="share" />
    <AdCard v-if="!iOS" class="mt-5" />
  </div>
</template>

<script>
import Editor from '@/components/Editor.vue'
import topNav from '@/components/TopNav.vue'
import AdCard from "@/components/ads/AdCard";

export default {
  name: 'Home',
  components: {
    Editor,
    topNav,
    AdCard
  },
  data () {
    return {
      inputFile: '',
      share: false
    }
  },
  computed: {
    iOS () {
      return this.$store.state.iOS
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
