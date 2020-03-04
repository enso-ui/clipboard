<template>
    <input class="clipboard-input"
        readonly>
</template>

<script>
export default {
    name: 'Clipboard',

    props: {
        maxLength: {
            type: Number,
            default: 99999,
        },
    },

    computed: {
        iOS() {
            return ['iPad', 'iPhone', 'iPod'].includes(navigator.platform);
        },
    },

    methods: {
        copy(value) {
            this.$el.value = value;
            this.select();

            return document.execCommand('copy');
        },
        select() {
            if (this.iOS) {
                this.selectOnTouch();
                return;
            }

            this.$el.select();
        },
        selectOnTouch() {
            const { contentEditable, readOnly } = this.$el;

            this.$el.contentEditable = true;
            this.$el.readOnly = false;

            const range = document.createRange();
            range.selectNodeContents(this.$el);

            const selection = window.getSelection();

            selection.removeAllRanges();
            selection.addRange(range);

            this.$el.setSelectionRange(0, this.maxLength);

            this.$el.contentEditable = contentEditable;
            this.$el.readOnly = readOnly;
        },
    },
};
</script>

<style lang="scss">
    .clipboard-input {
        position:fixed;
        pointer-events:none;
        z-index:-9999;
        opacity:0;
    }
</style>
