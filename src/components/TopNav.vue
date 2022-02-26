<template>
    <div class="is-topnav p-3 pb-4 pt-4">
        <div class="columns is-mobile">
            <div class="column ml-4">
                <span>
                    md-Editor
                </span>
            </div>
            <div class="column settingsWrapper">
                <div v-if="settings" class="settingsItems settings">
                    <label class="label is-pointer setting noselect">
                        <input @change="fileInput" accept=".txt, .md" type="file" required/>
                        <span>
                            Import File
                        </span>
                    </label>
                    <span v-if="shareAvailable" @click="share" class="is-pointer mt-6 setting noselect">
                        Share File
                    </span>
                </div>
                <i
                    class="fas fa-ellipsis-v settings-icon is-pointer"
                    @click="settings=!settings"
                />
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'TopNav',
    data () {
        return {
            settings: false,
            shareAvailable: false
        }
    },
    created () {
        if(navigator.share !== undefined) {
            this.shareAvailable = true
        }
    },
    methods: {
        share () {
            this.$emit('share')
        },
        fileInput (event) {
            const file = event.target.files[0];
            const reader = new FileReader();
            reader.onload = e => this.$emit('fileInput', e.target.result);
            reader.readAsText(file);
            this.settings = false
        }
    }
}
</script>