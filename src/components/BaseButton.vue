<template>
        <button class="inline-flex items-center rounded border border-transparent bg-indigo-600 px-2.5 py-1.5 text-xs font-medium text-white shadow-sm hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2">{{ btnlabel }}</button>

</template>

<script setup>
const props = defineProps({
    btnlabel:{
        type: String,
    }
})
</script>

<style lang="scss" scoped>

</style>