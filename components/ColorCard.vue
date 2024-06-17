<template>
    <section class="bg-white p-2 lg:mb-10 sm:mb-6 rounded-lg">
        <div class="colour lg:h-60 sm:h-36 lg:w-40 sm:w-64 rounded hover:cursor-pointer" @click="handleCopy()"></div>
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