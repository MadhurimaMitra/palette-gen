<template>
    <section class="bg-white p-2 rounded-lg">
        <div class="colour h-60 w-40 rounded hover:cursor-pointer" @click="handleCopy()"></div>
        <p class="hexcode text-center font-medium"></p>
    </section>
</template>

<script>
    export default {
        data() {
            return {
                
            }
        },
        methods: {
            handleCopy() {
                this.$emit('copyClicked')
            }
        }
    }
</script>