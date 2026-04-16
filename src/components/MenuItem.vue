<script setup>
    defineProps({
        item: {
            type: Object,
            required: true,
        },
    })

    const emit = defineEmits(['remove-item'])
</script>

<template>
    <div class="card h-100 shadow-sm border-0" :class="{ 'opacity-50': !item.available }">
        <div class="card-body d-flex flex-column">
            <div class="d-flex justify-content-between align-items-start mb-2">
                <span class="badge bg-secondary rounded-pill">{{ item.category }}</span>
                <button
                    class="btn btn-sm btn-outline-danger border-0"
                    title="Remover"
                    @click="emit('remove-item', item.id)"
                >
                    ✕
                </button>
            </div>

            <h5 class="card-title fw-bold mb-2">{{ item.name }}</h5>

            <p class="card-text fs-5 fw-semibold text-success mb-2">
                R$ {{ Number(item.price).toFixed(2) }}
            </p>

            <div class="mt-auto">
                <span :class="['badge rounded-pill', item.available ? 'bg-success' : 'bg-danger']">
                    {{ item.available ? 'Disponível' : 'Indisponível' }}
                </span>
            </div>
        </div>
    </div>
</template>
