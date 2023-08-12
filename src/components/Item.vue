<template>
    <div class="col-span-1 flex rounded-md shadow-sm" v-if="activity && color">
        <div
            class="flex-shrink-0 flex items-center justify-center w-16 text-white text-sm font-medium rounded-l-md uppercase" :style="`background-color:${color};`">
            <span >
                {{ activity.slice(0,1) }}
            </span>
        </div>
        <div
            class="flex max-h-12 py-2 flex-1 items-center justify-between truncate rounded-r-md border-t border-r border-b border-gray-200 bg-white">
            <div class="flex-1 truncate px-4 py-2 text-sm">
                <p class="text-gray-500">{{ activity }}</p>
            </div>
            <div class="flex-shrink-0 pr-2" v-if="actionButton">
              <button
                type="button"
                class="inline-flex h-8 w-8 items-center justify-center rounded-full bg-white bg-transparent text-gray-400 hover:text-gray-500"
                @click="$emit('handlePopOver')"
              >
                <EllipsisVerticalIcon class="h-5 w-5" aria-hidden="true" />
              </button>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ArrowLongDownIcon, EllipsisVerticalIcon } from "@heroicons/vue/20/solid";

const props = defineProps({
    activity:{
        type: String,
    },
    actionButton:Boolean,
    color:{
        type: String,
    }
})

</script>

<style lang="scss" scoped></style>
