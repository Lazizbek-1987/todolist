<template>
    <header
        class="text-center text-lg md:text-2xl items-center font-bold p-2 md:p-4 rounded shadow-lg shadow-orange-200 uppercase
        tracking-widest text-orange-700"
    >
        {{ brand }} -
        <span class="text-blue-500">
            {{ title }}
        </span>
    </header>
</template>

<script>
export default {
    name: "TheHeader",
    props: {
        brand: {type: String},
        title: {title: String}
    }
}
</script>

<style scoped>

</style>