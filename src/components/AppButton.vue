<template>
    <button
        @click="addTask"
        class="cursor-pointer border-2 border-l-0  px-3 md:px-6 py-1 md:py-3 font-medium text-white rounded
        rounded-l-none duration-500 h-10 md:h-auto"
    >
        <slot></slot>
    </button>
</template>

<script>
export default {
    name: "AppButton",
    props: {
        type: {type: String, default: 'button'},
    },
    methods: {
        addTask() {
            this.$emit('addToDo')
        }
    }
}
</script>

<style scoped>

</style>