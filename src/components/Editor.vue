<template>
  <div class="is-editor-wrapper">
        <div class="buttonsWrapper">
            <transition name="fade" mode="out-in">
                <span v-if="!preview" :key="1">
                    <button
                        @click="preview = !preview"
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
                        @click="preview = !preview"
                        class="button is-primary mt-5 pl-4 is-center"
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
                <textarea
                    v-model="inputText"
                    class="is-editor pt-5 pb-5 is-primary mt-1"
                    placeholder="Type your markdown here"
                />
            </div>
            <div v-else :key="2" class="is-markdown-content">
                <VueMarkdown :source="inputText" />
            </div>
        </transition>
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

export default {
    name: 'Editor',
    components: {
        SaveModal,
        VueMarkdown
    },
    props: {
        inputFile: {
            type: String,
            required: false,
            default: ''
        },
        share: {
            type: Boolean,
            required: true
        }
    },
    data () {
        return  {
            inputText: '',
            preview: false,
            markdownWrapper: '',
            saveFileModalOpen: false,
            shareAvailable: false
        }
    },
    watch: {
        inputFile (val) {
            if (val != '') this.inputText = val
        },
        share (val) {
            if (val) this.shareFile()
        }
    },
    computed: {
        iOS () {
            return this.$store.state.iOS
        }
    },
    created () {
        this.shareAvailable = window.navigator.share
    },
    methods: {
        saveFileModal () {
            if (!this.iOS) {
                this.saveFileModalOpen = true
                return
            }
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
        }
    }
}
</script>