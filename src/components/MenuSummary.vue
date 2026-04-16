<script setup>
    defineProps({
        totalItems: {
            type: Number,
            required: true,
        },
        availableItems: {
            type: Number,
            required: true,
        },
        averagePrice: {
            type: String,
            required: true,
        },
    })
</script>

<template>
    <div class="row g-3 mb-4">
        <div class="col-sm-4">
            <div class="card border-0 shadow-sm text-center">
                <div class="card-body py-3">
                    <div class="fs-3 fw-bold text-dark">{{ totalItems }}</div>
                    <small class="text-muted">Total de itens</small>
                </div>
            </div>
        </div>
        <div class="col-sm-4">
            <div class="card border-0 shadow-sm text-center">
                <div class="card-body py-3">
                    <div class="fs-3 fw-bold text-success">{{ availableItems }}</div>
                    <small class="text-muted">Disponíveis</small>
                </div>
            </div>
        </div>
        <div class="col-sm-4">
            <div class="card border-0 shadow-sm text-center">
                <div class="card-body py-3">
                    <div class="fs-3 fw-bold text-primary">R$ {{ averagePrice }}</div>
                    <small class="text-muted">Preço médio</small>
                </div>
            </div>
        </div>
    </div>
</template>
