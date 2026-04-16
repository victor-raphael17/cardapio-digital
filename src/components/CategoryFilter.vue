<script setup>
    defineProps({
        activeCategory: {
            type: String,
            required: true,
        },
    })

    const emit = defineEmits(['filter-change'])

    const allCategories = ['Todas', 'Lanche', 'Bebida', 'Sobremesa']
</script>

<template>
    <div class="d-flex flex-wrap gap-2 mb-4">
        <button
            v-for="cat in allCategories"
            :key="cat"
            :class="[
                'btn btn-sm rounded-pill px-3',
                cat === activeCategory ? 'btn-dark' : 'btn-outline-dark',
            ]"
            @click="emit('filter-change', cat)"
        >
            {{ cat }}
        </button>
    </div>
</template>
