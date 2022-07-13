<template>
    <wwElement
        class="ww-input-verification-code"
        v-for="(_, index) of Array(content.nbrOfCode)"
        :key="index"
        :ref="`ww-input-verification-code-${index}`"
        v-bind="content.input"
        @keydown.backspace="backspace(index)"
        :ww-props="{ value: localValue[index] }"
        @element-event="onChange(index, $event)"
    />
</template>

<script>
export default {
    props: {
        uid: { type: String, required: true },
        content: { type: Object, required: true },
        /* wwEditor:start */
        wwEditorState: { type: Object, required: true },
        /* wwEditor:end */
    },
    emits: ['trigger-event'],
    setup(props) {
        const { value, setValue } = wwLib.wwVariable.useComponentVariable({
            uid: props.uid,
            name: 'value',
            defaultValue: '',
            componentType: 'element',
            type: 'string',
            readonly: true,
            resettable: true,
        });
        return { value, setValue };
    },
    data() {
        return {
            localValue: '',
        };
    },
    computed: {
        isEditing() {
            /* wwEditor:start */
            return this.wwEditorState.editMode === wwLib.wwEditorHelper.EDIT_MODES.EDITION;
            /* wwEditor:end */
            // eslint-disable-next-line no-unreachable
            return false;
        },
        isComplete() {
            return this.localValue.split('').filter(char => char !== ' ').length >= this.content.nbrOfCode;
        },
    },
    watch: {
        isComplete(value) {
            if (!value) return;
            this.$emit('trigger-event', { name: 'complete', event: { value: this.localValue.trim() } });
        },
    },
    methods: {
        replaceAt(str, index, replacement) {
            if (!replacement.length) replacement = ' ';
            for (const i in Array(index).fill(0)) {
                if (str[i] === undefined && index !== i) {
                    console.log(i);
                    str = str.substring(0, i) + ' ' + str.substring(i + 1);
                    console.log(str);
                }
            }
            return str.substring(0, index) + replacement + str.substring(index + replacement.length);
        },
        onChange(index, { value, type }) {
            if (type !== 'update:value') return;
            if (value !== '') {
                if (index < this.content.nbrOfCode - 1)
                    this.$refs[`ww-input-verification-code-${index + 1}`].$el.nextElementSibling.children[0].focus();
                else this.$refs[`ww-input-verification-code-${index}`].$el.nextElementSibling.children[0].blur();
            }
            this.localValue = this.replaceAt(`${this.localValue}`, index, `${value}`).substring(
                0,
                this.content.nbrOfCode
            );
            this.setValue(this.localValue.trim());
            this.$emit('trigger-event', { name: 'change', event: { value: this.localValue.trim() } });
        },
        backspace(index) {
            if (index && (this.localValue[index] === ' ' || this.localValue[index] === undefined))
                this.$nextTick(() =>
                    this.$refs[`ww-input-verification-code-${index - 1}`].$el.nextElementSibling.children[0].focus()
                );
        },
    },
};
</script>

<style lang="scss">
.ww-input-verification-code {
    /* Chrome, Safari, Edge, Opera */
    input::-webkit-outer-spin-button,
    input::-webkit-inner-spin-button {
        -webkit-appearance: none;
        margin: 0;
    }
    /* Firefox */
    input[type='number'] {
        -moz-appearance: textfield;
    }
}
</style>
