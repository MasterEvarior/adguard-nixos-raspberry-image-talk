<script setup lang="ts">
defineProps<{
    links: {
        type: Array<{
            title: string
            url: string
        }>,
        required: true
    }
}>()
</script>

<template>
    <div class="flex justify-center gap-16 mt-16">
        <div
            v-for="(link, index) in links"
            :key="index"
            class="flex flex-col items-center gap-4"
        >
            <h2>{{ link.title }}</h2>

            <img
                :src="`https://api.qrserver.com/v1/create-qr-code/?size=150x150&data=${link.url}`"
                alt="QR Code"
            />

            {{ link.url.replace('https://', '') }}
        </div>
    </div>
</template>
