<template>
  <div class="is-editor-wrapper">
        <div class="buttonsWrapper">
            <transition name="fade" mode="out-in">
                <span v-if="!preview" :key="1">
                    <button
                        @click="previewToggle"
                        class="button is-primary mt-5 pl-4"
                    >
                        Preview
                        <i class="fas fa-eye pl-1 pr-2" />
                    </button>
                    <button
                        v-if="!preview && (!iOS || shareAvailable)"
                        :key="1"
                        class="button is-primary ml-4 pl-4"
                        @click="saveFileModal"
                    >
                        Save File
                        <i class="fas fa-download pl-1 pr-2" />
                    </button>
                </span>
                <span v-else :key="2">
                    <button
                        @click="previewToggle"
                        class="button is-primary mt-5 pl-4"
                    >
                        Edit
                        <i class="fas fa-highlighter pl-1 pr-2" />
                    </button>
                </span>
            </transition>
        </div>
        <br>
        <br>
        <transition name="fade" mode="out-in">
            <div v-if="!preview" :key="1">
              <prism-editor
                  class="my-editor"
                  v-model="inputText"
                  :highlight="highlighter"
                  line-numbers />
            </div>
            <div v-else :key="2" class="is-markdown-content">
                <VueMarkdown :source="inputText" />
            </div>
        </transition>

        <button
            v-if="iosLiteApp"
            @click="openAppStore"
            class="button is-ads-button is-border-secondary mt-5"
        >
          Get Rid Of Ads
        </button>

        <SaveModal
            v-if="saveFileModalOpen"
            @save="downloadFile"
            @close="saveFileModalOpen=false"
        />
  </div>
</template>

<script>
import SaveModal from '@/components/modals/SaveModal'
import VueMarkdown from 'vue-markdown'
import { PrismEditor } from 'vue-prism-editor'
import 'vue-prism-editor/dist/prismeditor.min.css'
import { highlight } from "prismjs/components/prism-core"
import Prism from "prismjs"
import 'prismjs/components/prism-clike'
import 'prismjs/components/prism-markdown'
import 'prismjs/components/prism-css'
import 'prismjs/components/prism-javascript'
import 'prismjs/themes/prism-tomorrow.css'

export default {
    name: 'Editor',
    components: {
        SaveModal,
        VueMarkdown,
        PrismEditor
    },
    props: {
        share: {
            type: Boolean,
            required: true
        }
    },
    data () {
        return  {
            inputText: '\n\n\n\n\n\n\n\n\n\n\n\n\n\n',
            preview: false,
            markdownWrapper: '',
            saveFileModalOpen: false,
            shareAvailable: false,
            clicks: 0
        }
    },
    watch: {
        inputFile (val) {
            if (val != '') {
                this.inputText = val
                this.$store.state.inputFile = ''
            }
        },
        share (val) {
            if (val) this.shareFile()
        }
    },
    computed: {
        iOS () {
            return this.$store.state.iOS
        },
        inputFile () {
            return this.$store.state.inputFile
        },
        iosLiteApp () {
          return window.webkit && window.webkit.messageHandlers.openAppStore
        }
    },
    created () {
        this.shareAvailable = window.navigator.share
        this.clicks = parseInt(localStorage.getItem('clickedGenerate'))
        if(this.clicks == null || isNaN(this.clicks)) {
            this.clicks = 0
        }
    },
    methods: {
        previewToggle () {
            this.preview = !this.preview
            this.addClick()
        },
        saveFileModal () {
            if (!this.iOS) {
                this.saveFileModalOpen = true
                return
            }
            this.addClick()
            this.shareFile()
        },
        createFileLink (fileName) {
            const file = new Blob([this.inputText], { type: "data:text/csv;charset=utf-8" }, `${fileName}.md`)
            return window.URL.createObjectURL(file)
        },
        downloadFile (fileName) {
            const link = this.createFileLink (fileName)

            let hiddenElement = document.createElement('a')
            hiddenElement.href = link
            hiddenElement.download = `${fileName}.md`
            hiddenElement.click();
            this.saveFileModalOpen = false
        },
        shareFile () {
            if (this.inputText.length) {
                navigator.share({
                    "title": 'Markdown File',
                    "text": this.inputText
                })
            }
        },
        openAppStore () {
            window.webkit.messageHandlers.openAppStore.postMessage({
              "message": 'openAppStore'
            })
        },
        addClick () {
            this.clicks++
            localStorage.setItem('clicked', this.clickedGenerate)

            if(this.clicks > 5) {
                this.clicks = 0
                localStorage.setItem('clicked', 0)
                this.showInterstitial()
            }
        },
        showInterstitial () {
          if (this.iosLiteApp) {
            window.webkit.messageHandlers.showInterstitial.postMessage({
              "message": 'showInterstitial'
            })
          }
        },
        highlighter(code) {
          if (Prism.languages.markdown) {
            this.javascript = Prism.languages.markdown
          }
          return highlight(code, this.javascript, ''); // languages.<insert language> to return html with markup
        }
    }
}
</script>
