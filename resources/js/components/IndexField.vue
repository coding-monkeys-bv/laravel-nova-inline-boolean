<template>
    <!-- When editable on index -->
    <div v-if="field.inlineIndex" slot="field">
        <div class="flex items-center">
            <div class="py-2 mr-3">
                <checkbox
                    @input="toggle"
                    :id="field.attribute"
                    :name="field.name"
                    :checked="checked"
                    :disabled="isReadonly"
                />
            </div>
            <div v-if="field.textOnIndex" class="py-2">
                {{ displayValue }}
            </div>
        </div>
    </div>
    <!-- When not editable on index -->
    <div v-else>
        <div class="flex items-center">
            <span class="inline-block rounded-full w-2 h-2 mr-3"
                :class="{ 'bg-green-500': field.value, 'bg-red-500': !field.value }">
            </span>
            <div v-if="field.textOnIndex" class="py-2">
                {{ displayValue }}
            </div>
        </div>
    </div>
</template>

<script>
import InlineInit from './mixins/init';
import InlineMixin from './mixins/inline';

export default {
    props: ['resourceName', 'field'],

    mixins: [
        InlineInit, 
        InlineMixin,
    ],

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
        
        resourceId() {
            return this.$parent.resource.id.value;
        }
    },
}
</script>
