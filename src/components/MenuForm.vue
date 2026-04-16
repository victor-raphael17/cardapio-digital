<script setup>
    import { ref, computed, onMounted, nextTick } from 'vue'

    const emit = defineEmits(['add-item'])

    const name = ref('')
    const price = ref(null)
    const category = ref('')
    const available = ref(true)
    const nameInput = ref(null)

    const categories = ['Lanche', 'Bebida', 'Sobremesa']

    const isFormValid = computed(
        () => name.value !== '' && price.value != null && price.value > 0 && category.value !== ''
    )

    onMounted(() => {
        nameInput.value?.focus()
    })

    function submitForm() {
        if (!name.value || price.value == null || price.value <= 0 || !category.value) return

        emit('add-item', {
            id: crypto.randomUUID(),
            name: name.value,
            price: price.value,
            category: category.value,
            available: available.value,
        })

        name.value = ''
        price.value = null
        category.value = ''
        available.value = true

        nextTick(() => {
            nameInput.value?.focus()
        })
    }
</script>

<template>
    <div class="card shadow-sm border-0">
        <div class="card-body">
            <h2 class="card-title h5 fw-bold mb-3">➕ Novo Item</h2>

            <form @submit.prevent="submitForm">
                <div class="mb-3">
                    <label for="name" class="form-label fw-semibold">Nome</label>
                    <input
                        id="name"
                        ref="nameInput"
                        v-model.trim="name"
                        type="text"
                        class="form-control"
                        placeholder="Ex: X-Bacon"
                        required
                    />
                </div>

                <div class="mb-3">
                    <label for="price" class="form-label fw-semibold">Preço (R$)</label>
                    <input
                        id="price"
                        v-model.number="price"
                        type="number"
                        class="form-control"
                        min="0.01"
                        step="0.01"
                        placeholder="0.00"
                        required
                    />
                </div>

                <div class="mb-3">
                    <label for="category" class="form-label fw-semibold">Categoria</label>
                    <select id="category" v-model="category" class="form-select" required>
                        <option value="" disabled>Selecione</option>
                        <option v-for="cat in categories" :key="cat" :value="cat">{{ cat }}</option>
                    </select>
                </div>

                <div class="mb-3 form-check">
                    <input
                        id="available"
                        v-model="available"
                        type="checkbox"
                        class="form-check-input"
                    />
                    <label for="available" class="form-check-label">Disponível</label>
                </div>

                <button
                    type="submit"
                    class="btn btn-dark w-100 fw-semibold"
                    :disabled="!isFormValid"
                >
                    Adicionar
                </button>
            </form>
        </div>
    </div>
</template>
