<template>
    <DefaultField :field="field" :errors="errors" :show-help-text="showHelpText">
        <template #field>
            <checkbox
                class="py-2"
                @input="toggle"
                :id="field.attribute"
                :name="field.name"
                :checked="checked"
                :disabled="isReadonly"
            />
        </template>
    </DefaultField>
</template>

<script>
import { FormField, HandlesValidationErrors } from 'laravel-nova'

export default {
    mixins: [FormField, HandlesValidationErrors],

    props: ['resourceName', 'resourceId', 'field'],

    methods: {

        data: () => ({
            value: false,
        }),

        mounted() {
            this.value = this.field.value || false

            this.field.fill = formData => {
                formData.append(this.field.attribute, this.trueValue)
            }
        },

        methods: {
            toggle() {
                this.value = !this.value
            },
        },

        computed: {
            checked() {
                return Boolean(this.value)
            },

            trueValue() {
                return +this.checked
            },
        },

        /*
        * Set the initial, internal value for the field.
        */
        setInitialValue() {
            this.value = this.field.value || ''
        },

        /**
         * Fill the given FormData object with the field's internal value.
         */
        fill(formData) {
            formData.append(this.field.attribute, this.value || '')
        },
    },
}
</script>
