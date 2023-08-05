<template>
    <input
        :type="type"
        :placeholder="placeholder"
        :value="taskName"
        @input="$emit('update:taskName', $event.target.value)"
    >
</template>

<script>
export default {
    name: "AppInput",
    props: {
        type: {type: String, default: 'text'},
        placeholder: {type: String},
        taskName: String
    },
    emits: ['update:taskName']
}
</script>

<style scoped>

</style>