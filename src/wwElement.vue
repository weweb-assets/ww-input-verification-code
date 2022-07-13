<template>
    <wwElement
        class="ww-input-verification-code"
        v-for="(_, index) of Array(content.nbrOfCode)"
        :key="index"
        :ref="`ww-input-verification-code-${index}`"
        v-bind="content.input"
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
            defaultValue: null,
            componentType: 'element',
            type: 'number',
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
        value(value) {
            this.localValue = `${value}`;
        },
        isComplete(value) {
            if (!value) return;
            this.$emit('trigger-event', { name: 'complete', event: { value: parseInt(this.localValue) } });
        },
    },
    methods: {
        replaceAt(str, index, replacement) {
            return str.substring(0, index) + replacement + str.substring(index + replacement.length);
        },
        onChange(index, { value, type }) {
            if (type !== 'update:value') return;
            if (value !== '') {
                if (index < this.content.nbrOfCode - 1)
                    this.$refs[`ww-input-verification-code-${index + 1}`].$el.nextElementSibling.children[0].focus();
                else this.$refs[`ww-input-verification-code-${index}`].$el.nextElementSibling.children[0].blur();
            } else {
                if (index)
                    this.$refs[`ww-input-verification-code-${index - 1}`].$el.nextElementSibling.children[0].focus();
            }
            this.localValue = this.replaceAt(`${this.localValue}`, index, `${value} `);
            this.setValue(parseInt(this.localValue));
            this.$emit('trigger-event', { name: 'change', event: { value: parseInt(this.localValue) } });
        },
    },
};
</script>

<style lang="scss" scoped>
.ww-input-verification-code {
    /deep/ * {
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
}
</style>
