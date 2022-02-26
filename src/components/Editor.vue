<template>
  <div class="is-editor-wrapper">
        <transition name="fade" mode="out-in">
            <span v-if="!preview" :key="1">
                <button
                    @click="createMarkdown"
                    class="button is-primary mt-5 pl-4"
                >
                    Preview
                    <i class="fas fa-eye pl-1 pr-2" /> 
                </button>
                <button
                    v-if="!preview"
                    :key="1"
                    class="button is-primary ml-4 pl-4"
                    @click="saveFileModalOpen = true"
                >
                    Save File
                    <i class="fas fa-download pl-1 pr-2" /> 
                </button>
            </span>
            <span v-else :key="2">
                <button
                    @click="createMarkdown"
                    class="button is-primary mt-5 pl-4"
                >
                    Edit
                    <i class="fas fa-highlighter pl-1 pr-2" /> 
                </button>
                <button
                    class="button is-primary ml-3 pl-4" 
                    @click="saveFileModalOpen = true"
                >
                    Save File
                    <i class="fas fa-download pl-1 pr-2" /> 
                </button>
            </span>
        </transition>
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
                <p>
                    <span v-html="markdownText">
                    </span>
                </p>
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
const markdown = require( "markdown" ).markdown;

export default {
    name: 'Editor',
    components: {
        SaveModal
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
            markdownText: '',
            preview: false,
            markdownWrapper: '',
            saveFileModalOpen: false
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
    methods: {
        createMarkdown () {
            if (!this.preview) {
                this.markdownText = markdown.toHTML(this.inputText)
            }
            this.preview = !this.preview
        },
        saveFileModal () {
            this.saveFileModalOpen = true
        },
        downloadFile (fileName) {
            const file = new Blob([this.inputText], { type: "data:text/csv;charset=utf-8" }, `${fileName}.md`)
            const link = window.URL.createObjectURL(file)

            let hiddenElement = document.createElement('a')
            hiddenElement.href = link
            hiddenElement.download = `${fileName}.md`
            hiddenElement.click();  
            this.saveFileModalOpen = false
        },
        sharFile () {
            if (this.inputText.length) {
                navigator.share({
                    "title": 'Markdown File',
                    "text": this.inputText
                })
            }
        },
    }
}
</script>

<style>

</style>