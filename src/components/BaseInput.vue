<template>
    <div class="input">
        <label :for="inputname" class="block text-sm font-medium text-gray-700 mb-1">{{ label }}</label>
        <Field :name="inputname" :type="inputtype" :class="['block w-full px-2 py-2 rounded-md border-2 border-gray-300 shadow-sm mb-1 focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm', inputstyle]" />
        <ErrorMessage class="text-red-500" :name="inputname"/>
    </div>
</template>

<script setup>
import {Field, ErrorMessage } from 'vee-validate';
const props = defineProps({
    label:{
        type: String
    }
    ,inputname:{
        type: String,
        required: true
    },
    inputtype:{
        type: String,
        required: true
    },
    inputstyle:{

    }
})
</script>

<style lang="scss" scoped>

</style>