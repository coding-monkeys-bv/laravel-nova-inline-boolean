<template>
    <PanelItem :index="index" :field="field">
        <template #value>
            <!-- If editable on detail screen -->
            <template v-if="field.inlineDetail">
                <checkbox
                    @input="toggle"
                    :id="field.attribute"
                    :name="field.name"
                    :checked="checked"
                    :disabled="isReadonly"
                />
            </template>

            <!-- If not editable on detail screen -->
            <template v-else>
                <div class="inline-flex items-center">
                    <span class="inline-block rounded-full w-2 h-2 mr-3" :class="{ 'bg-green-500': field.value, 'bg-red-500': !field.value }"></span>
                    <span>{{ displayValue }}</span>
                </div>
            </template>

        </template>
    </PanelItem>
</template>

<script>
import { Errors, FormField, HandlesValidationErrors } from 'laravel-nova'
import InlineInit from './mixins/init';
import InlineMixin from './mixins/inline';

export default {

    mixins: [FormField, HandlesValidationErrors, InlineInit, InlineMixin],

    props: ['index', 'resource', 'resourceName', 'resourceId', 'field'],

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
            this.submit()
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
}
</script>
