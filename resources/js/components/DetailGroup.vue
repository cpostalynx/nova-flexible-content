<template>
    <div class="relative flex bg-white mb-4 pb-1" :id="group.key">
        <div class="z-10 bg-white border-t border-l border-b border-60 h-auto pin-l pin-t rounded-l self-start w-8">
            <button
                dusk="expand-group"
                type="button"
                class="group-control btn border-r border-40 w-8 h-8 block"
                :title="__('Expand')"
                @click.prevent="expand"
                v-if="collapsed">
                <icon class="align-top" type="plus-square" width="16" height="16" view-box="0 0 24 24"/>
            </button>
            <div v-if="!collapsed">
                <button
                    dusk="collapse-group"
                    type="button"
                    class="group-control btn border-r border-40 w-8 h-8 block"
                    :title="__('Collapse')"
                    @click.prevent="collapse">
                    <icon class="align-top" type="minus-square" width="16" height="16" view-box="0 0 24 24"/>
                </button>
            </div>
        </div>
        <div class="-mb-1 flex flex-col min-h-full w-full fields-container">
            <div :class="titleStyle">
                <div class="leading-normal py-1 px-8"
                     :class="{'border-b border-40': !collapsed}">
                    <p class="text-80">
                        <span class="mr-4 font-semibold">#{{ index + 1 }}</span>
                        <template v-if="group.title">{{ group.title }}</template>
                        <template v-else-if="group.fields.length > 0">
                            <b>{{ group.fields[0].name }}:</b> {{ group.fields[0].value }}
                        </template>
                    </p>
                </div>
            </div>
            <div :class="containerStyle">
                <component
                    v-for="(item, index) in group.fields"
                    :key="index"
                    :is="'detail-' + item.component"
                    :resource-name="resourceName"
                    :resource-id="resourceId"
                    :resource="resource"
                    :field="item"
                    :errors="validationErrors"
                    :class="{ 'remove-bottom-border': index == group.fields.length - 1 }"
                />
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: ['group', 'index', 'last', 'resource', 'resourceName', 'resourceId'],

    data() {
        return {
            collapsed: this.group.collapsed
        };
    },

    computed: {
        titleStyle() {
            let classes = ['border-t', 'border-r', 'border-60', 'rounded-tr-lg'];
            if (this.collapsed) {
                classes.push('border-b rounded-br-lg');
            }
            return classes;
        },
        containerStyle() {
            let classes = ['flex-grow', 'border-b', 'border-r', 'border-l', 'border-60', 'rounded-b-lg', 'fields'];
            if (this.collapsed) {
                classes.push('hidden');
            }
            return classes;
        }
    },

    methods: {
        /**
         * Expand fields
         */
        expand() {
            this.collapsed = false;
        },

        /**
         * Collapse fields
         */
        collapse() {
            this.collapsed = true;
        }
    }
}
</script>

<style>
.group-control:focus {
    outline: none;
}

.group-control path {
    fill: #B7CAD6;
    transition: fill 200ms ease-out;
}

.group-control:hover path {
    fill: var(--primary);
}

.fields-container .fields > div {
    margin-left: 0;
    margin-right: 0;
}
</style>
