<template>
  <app-loader v-if="loading"></app-loader>
  <app-page back title="Заявка" v-else-if="request">
    <p><strong>Имя владельца</strong>: {{ request.fio }}</p>
    <p><strong>Телефон</strong>: {{ request.phone }}</p>
    <p><strong>Сумма</strong>: {{ currency(request.amount) }}</p>
    <p><strong>Статус</strong>: <app-status :type="request.status" /></p>

    <div class="form-control">
      <label for="status">Статус</label>
      <select name="" id="status" v-model="status">
        <option value="done">Завершен</option>
        <option value="canceled">Отменен</option>
        <option value="active">Активен</option>
        <option value="pending">Выполняется</option>
      </select>
    </div>

    <button class="btn danger" @click="remove">Удалить</button>
    <button class="btn" @click="update" v-if="hasChanged">Обновить</button>
  </app-page>
  <h3 v-else class="text-center text-white">Заявки с ID = {{ $route.params.id }} нет.</h3>
</template>

<script>
import AppPage from '@/components/ui/AppPage'
import { useRoute, useRouter } from 'vue-router'
import { ref, onMounted, computed } from 'vue'
import { useStore } from 'vuex'
import AppLoader from '@/components/ui/AppLoader'
import AppStatus from '@/components/ui/AppStatus'
import { currency } from '@/utils/currency-formatter'

export default {
  setup() {
    const loading = ref(true)
    const route = useRoute()
    const router = useRouter()
    const store = useStore()
    const request = ref({})
    const status = ref()

    onMounted(async () => {
      loading.value = true
      request.value = await store.dispatch('request/loadOne', route.params.id)
      status.value = request.value?.status
      loading.value = false
    })

    const hasChanged = computed(() => status.value !== request.value.status)

    const remove = async () => {
        if (confirm('Вы действительно хотите удалить заявку?')) {
          await store.dispatch('request/remove', route.params.id)
          router.push('/')
        }2
    }

    const update = async () => {
      const data = {...request.value, status: status.value, id: route.params.id}
      await store.dispatch('request/update', data)
      request.value.status = status.value
      // router.push('/')
    }

    return {
      loading,
      request,
      currency,
      remove,
      update,
      status,
      hasChanged
    }
  },
  components: {
    AppPage,
    AppLoader,
    AppStatus
  }
}
</script>
