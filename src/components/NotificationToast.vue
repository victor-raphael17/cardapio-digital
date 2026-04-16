<script setup>
    import { ref, computed, onMounted, onUnmounted } from 'vue'

    const props = defineProps({
        message: {
            type: String,
            required: true,
        },
        type: {
            type: String,
            default: 'success',
            validator: (val) => ['success', 'info', 'error'].includes(val),
        },
        duration: {
            type: Number,
            default: 3000,
        },
    })

    const emit = defineEmits(['dismiss'])

    const timerId = ref(null)

    const alertClass = computed(() => {
        const map = { success: 'alert-success', info: 'alert-info', error: 'alert-danger' }
        return map[props.type] || 'alert-info'
    })

    onMounted(() => {
        timerId.value = setTimeout(() => {
            emit('dismiss')
        }, props.duration)
    })

    onUnmounted(() => {
        clearTimeout(timerId.value)
    })
</script>

<template>
    <div :class="['alert d-flex align-items-center shadow-sm mb-2', alertClass]" role="alert">
        <span class="me-auto">{{ message }}</span>
        <button type="button" class="btn-close ms-2" @click="emit('dismiss')"></button>
    </div>
</template>

<style scoped>
    .alert {
        min-width: 300px;
        animation: slideIn 0.3s ease;
    }

    @keyframes slideIn {
        from {
            opacity: 0;
            transform: translateX(30px);
        }

        to {
            opacity: 1;
            transform: translateX(0);
        }
    }
</style>
