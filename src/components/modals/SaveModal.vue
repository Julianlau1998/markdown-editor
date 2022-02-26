<template>
    <div>
        <div class="is-modal-wrapper"></div>
        <div class="modal">
            <form>
                <h2>
                    Save File
                </h2>
                <br>
                <input
                    v-model="fileName"
                    type="text"
                    placeholder="File Name"
                    class="input"
                    max="255"
                    min="1"
                    required
                    autofocus
                >
                <br>
                <button @click="close" class="button is-secondary">
                    Close
                </button>
                <button type="submit" @click="onSubmit" class="button is-primary">
                    Save
                </button>
            </form>
        </div>
    </div>
</template>

<script>
export default {
    data () {
        return {
            fileName: ''
        }
    },
    methods: {
        onSubmit () {
            this.$emit('save', this.fileName)
        },
        close () {
            this.$emit('close')
        }
    }
}
</script>