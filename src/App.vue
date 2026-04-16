<script setup>
    import { ref, computed, watch, watchEffect, onMounted } from 'vue'
    import MenuForm from './components/MenuForm.vue'
    import CategoryFilter from './components/CategoryFilter.vue'
    import MenuItem from './components/MenuItem.vue'
    import MenuSummary from './components/MenuSummary.vue'
    import NotificationToast from './components/NotificationToast.vue'

    const items = ref([])
    const activeCategory = ref('Todas')
    const notifications = ref([])

    const filteredItems = computed(() => {
        if (activeCategory.value === 'Todas') return items.value
        return items.value.filter((item) => item.category === activeCategory.value)
    })

    const availableCount = computed(() => {
        return items.value.filter((item) => item.available).length
    })

    const averagePrice = computed(() => {
        if (filteredItems.value.length === 0) return '0.00'
        const sum = filteredItems.value.reduce((acc, item) => acc + item.price, 0)
        return (sum / filteredItems.value.length).toFixed(2)
    })

    onMounted(() => {
        const saved = localStorage.getItem('cardapio-items')
        if (saved) {
            try {
                items.value = JSON.parse(saved)
            } catch {
                localStorage.removeItem('cardapio-items')
            }
        }
    })

    watchEffect(() => {
        document.title = `Cardápio Digital (${items.value.length} itens)`
    })

    watch(items, (newItems) => {
            localStorage.setItem('cardapio-items', JSON.stringify(newItems))
        },
        { deep: true },
    )

    function addItem(item) {
        items.value.push(item)
        notify(`"${item.name}" adicionado ao cardápio`, 'success')
    }

    function removeItem(id) {
        const removed = items.value.find((item) => item.id === id)
        items.value = items.value.filter((item) => item.id !== id)
        if (removed) {
            notify(`"${removed.name}" removido do cardápio`, 'error')
        }
    }

    function notify(message, type = 'info') {
        notifications.value.push({ id: crypto.randomUUID(), message, type })
    }

    function removeNotification(id) {
        notifications.value = notifications.value.filter((n) => n.id !== id)
    }
</script>

<template>
    <div id="app" class="min-vh-100 bg-light">
        <header class="bg-dark text-white py-4 mb-4 shadow">
            <div class="container text-center">
                <h1 class="display-5 fw-bold mb-1">🍔 Cardápio Digital</h1>
                <p class="text-white-50 mb-0">Lanchonete do Vue</p>
            </div>
        </header>

        <main class="container pb-5">
            <div class="row g-4">
                <aside class="col-lg-4">
                    <MenuForm @add-item="addItem" />
                </aside>

                <section class="col-lg-8">
                    <MenuSummary
                        :total-items="items.length"
                        :available-items="availableCount"
                        :average-price="averagePrice"
                    />

                    <CategoryFilter
                        :active-category="activeCategory"
                        @filter-change="activeCategory = $event"
                    />

                    <div
                        v-if="filteredItems.length"
                        class="row row-cols-1 row-cols-md-2 row-cols-xl-3 g-3"
                    >
                        <div v-for="item in filteredItems" :key="item.id" class="col">
                            <MenuItem :item="item" @remove-item="removeItem" />
                        </div>
                    </div>

                    <div v-else class="text-center py-5">
                        <p class="text-muted fs-5">
                            Nenhum item encontrado. Cadastre um novo item no formulário ao lado.
                        </p>
                    </div>
                </section>
            </div>
        </main>

        <div class="toast-wrapper position-fixed bottom-0 end-0 p-3" style="z-index: 1080">
            <NotificationToast
                v-for="toast in notifications"
                :key="toast.id"
                :message="toast.message"
                :type="toast.type"
                @dismiss="removeNotification(toast.id)"
            />
        </div>
    </div>
</template>
