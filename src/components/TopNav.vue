<template>
    <div class="is-topnav p-3 pb-4 pt-4">
        <div class="columns is-mobile">
            <div class="column ml-4">
                <span>
                    md-Editor
                </span>
            </div>
            <!-- <div class="column settingsWrapper" v-if="!shareAvailable">
                <span v-if="settings" class="settings is-pointer p-2 mt-6 ">
                    Recommend
                </span>
                <i
                    class="fas fa-ellipsis-v settings-icon is-pointer"
                    @click="settings=!settings"
                />
            </div> -->
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
        recommend () {
            // todo: Uncomment after release to play store

            // navigator.share({
            //     "title": 'Simply Create Markdown files on your Phone with this app',
            //     "text": 'https://play.google.com/store/apps/details?id=xyz.appmaker.fdfdjd&gl=DE'
            // })
        }
    }
}
</script>