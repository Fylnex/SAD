<script setup lang="ts">
definePageMeta({
  layout: false
})

// Проверяем авторизацию
onMounted(() => {
  const authStatus = localStorage.getItem('sad_auth')
  if (authStatus !== 'true') {
    navigateTo('/auth')
  }
})

// Состояние формы
const form = ref({
  // Личные данные
  firstName: '',
  lastName: '',
  middleName: '',
  birthDate: '',
  gender: '',
  phone: '',
  email: '',
  
  // Адрес
  address: {
    country: '',
    city: '',
    street: '',
    house: '',
    apartment: '',
    postalCode: ''
  },
  
  // Дополнительная информация
  passport: {
    series: '',
    number: '',
    issuedBy: '',
    issueDate: '',
    departmentCode: ''
  },
  
  // Работа
  work: {
    company: '',
    position: '',
    workPhone: '',
    workEmail: ''
  }
})

const isLoading = ref(false)
const isEditing = ref(false)
const editingId = ref(null)

// Получаем сохраненные данные из localStorage
const savedData = ref([])

onMounted(() => {
  loadSavedData()
})

const loadSavedData = () => {
  const data = localStorage.getItem('sad_personal_data')
  if (data) {
    savedData.value = JSON.parse(data)
  }
}

const saveData = async () => {
  isLoading.value = true
  
  try {
    const dataToSave = {
      id: isEditing.value ? editingId.value : Date.now(),
      ...form.value,
      createdAt: isEditing.value ? savedData.value.find(item => item.id === editingId.value)?.createdAt : new Date().toISOString(),
      updatedAt: new Date().toISOString()
    }

    if (isEditing.value) {
      const index = savedData.value.findIndex(item => item.id === editingId.value)
      savedData.value[index] = dataToSave
    } else {
      savedData.value.push(dataToSave)
    }

    localStorage.setItem('sad_personal_data', JSON.stringify(savedData.value))
    
    // Сбрасываем форму
    resetForm()
    
    useToast().add({
      title: isEditing.value ? 'Данные обновлены' : 'Данные сохранены',
      color: 'green'
    })
  } catch (error) {
    useToast().add({
      title: 'Ошибка при сохранении',
      description: 'Попробуйте еще раз',
      color: 'red'
    })
  } finally {
    isLoading.value = false
  }
}

const editData = (data) => {
  form.value = { ...data }
  isEditing.value = true
  editingId.value = data.id
}

const deleteData = (id) => {
  savedData.value = savedData.value.filter(item => item.id !== id)
  localStorage.setItem('sad_personal_data', JSON.stringify(savedData.value))
  useToast().add({
    title: 'Данные удалены',
    color: 'green'
  })
}

const resetForm = () => {
  form.value = {
    firstName: '',
    lastName: '',
    middleName: '',
    birthDate: '',
    gender: '',
    phone: '',
    email: '',
    address: {
      country: '',
      city: '',
      street: '',
      house: '',
      apartment: '',
      postalCode: ''
    },
    passport: {
      series: '',
      number: '',
      issuedBy: '',
      issueDate: '',
      departmentCode: ''
    },
    work: {
      company: '',
      position: '',
      workPhone: '',
      workEmail: ''
    }
  }
  isEditing.value = false
  editingId.value = null
}

const handleLogout = () => {
  localStorage.removeItem('sad_auth')
  localStorage.removeItem('sad_user')
  navigateTo('/auth')
}
</script>

<template>
  <div class="min-h-screen bg-gray-50 dark:bg-gray-900">
    <!-- Header -->
    <header class="bg-white dark:bg-gray-800 shadow">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex justify-between items-center py-6">
          <div class="flex items-center space-x-4">
            <UButton
              color="gray"
              variant="ghost"
              icon="i-lucide-arrow-left"
              @click="navigateTo('/dashboard')"
            >
              Назад
            </UButton>
            <h1 class="text-2xl font-bold text-gray-900 dark:text-white">
              Управление данными
            </h1>
          </div>
          <UButton
            color="red"
            variant="outline"
            size="sm"
            @click="handleLogout"
          >
            Выйти
          </UButton>
        </div>
      </div>
    </header>

    <!-- Main Content -->
    <main class="max-w-7xl mx-auto py-6 sm:px-6 lg:px-8">
      <div class="px-4 py-6 sm:px-0">
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
          <!-- Форма -->
          <div class="lg:col-span-2">
            <UCard>
              <template #header>
                <h2 class="text-xl font-semibold">
                  {{ isEditing ? 'Редактировать данные' : 'Добавить новые данные' }}
                </h2>
              </template>

              <UForm @submit="saveData" class="space-y-6">
                <!-- Личные данные -->
                <div>
                  <h3 class="text-lg font-medium mb-4">Личные данные</h3>
                  <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                    <UFormGroup label="Фамилия" required>
                      <UInput v-model="form.lastName" />
                    </UFormGroup>
                    <UFormGroup label="Имя" required>
                      <UInput v-model="form.firstName" />
                    </UFormGroup>
                    <UFormGroup label="Отчество">
                      <UInput v-model="form.middleName" />
                    </UFormGroup>
                  </div>
                  
                  <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mt-4">
                    <UFormGroup label="Дата рождения">
                      <UInput v-model="form.birthDate" type="date" />
                    </UFormGroup>
                    <UFormGroup label="Пол">
                      <USelect
                        v-model="form.gender"
                        :options="[
                          { label: 'Мужской', value: 'male' },
                          { label: 'Женский', value: 'female' }
                        ]"
                      />
                    </UFormGroup>
                  </div>
                  
                  <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mt-4">
                    <UFormGroup label="Телефон">
                      <UInput v-model="form.phone" placeholder="+7 (999) 123-45-67" />
                    </UFormGroup>
                    <UFormGroup label="Email">
                      <UInput v-model="form.email" type="email" />
                    </UFormGroup>
                  </div>
                </div>

                <!-- Адрес -->
                <div>
                  <h3 class="text-lg font-medium mb-4">Адрес</h3>
                  <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <UFormGroup label="Страна">
                      <UInput v-model="form.address.country" />
                    </UFormGroup>
                    <UFormGroup label="Город">
                      <UInput v-model="form.address.city" />
                    </UFormGroup>
                  </div>
                  
                  <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mt-4">
                    <UFormGroup label="Улица">
                      <UInput v-model="form.address.street" />
                    </UFormGroup>
                    <UFormGroup label="Дом">
                      <UInput v-model="form.address.house" />
                    </UFormGroup>
                    <UFormGroup label="Квартира">
                      <UInput v-model="form.address.apartment" />
                    </UFormGroup>
                  </div>
                  
                  <UFormGroup label="Почтовый индекс" class="mt-4">
                    <UInput v-model="form.address.postalCode" />
                  </UFormGroup>
                </div>

                <!-- Паспорт -->
                <div>
                  <h3 class="text-lg font-medium mb-4">Паспортные данные</h3>
                  <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <UFormGroup label="Серия">
                      <UInput v-model="form.passport.series" />
                    </UFormGroup>
                    <UFormGroup label="Номер">
                      <UInput v-model="form.passport.number" />
                    </UFormGroup>
                  </div>
                  
                  <UFormGroup label="Кем выдан" class="mt-4">
                    <UInput v-model="form.passport.issuedBy" />
                  </UFormGroup>
                  
                  <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mt-4">
                    <UFormGroup label="Дата выдачи">
                      <UInput v-model="form.passport.issueDate" type="date" />
                    </UFormGroup>
                    <UFormGroup label="Код подразделения">
                      <UInput v-model="form.passport.departmentCode" />
                    </UFormGroup>
                  </div>
                </div>

                <!-- Работа -->
                <div>
                  <h3 class="text-lg font-medium mb-4">Рабочая информация</h3>
                  <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <UFormGroup label="Компания">
                      <UInput v-model="form.work.company" />
                    </UFormGroup>
                    <UFormGroup label="Должность">
                      <UInput v-model="form.work.position" />
                    </UFormGroup>
                  </div>
                  
                  <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mt-4">
                    <UFormGroup label="Рабочий телефон">
                      <UInput v-model="form.work.workPhone" />
                    </UFormGroup>
                    <UFormGroup label="Рабочий email">
                      <UInput v-model="form.work.workEmail" type="email" />
                    </UFormGroup>
                  </div>
                </div>

                <div class="flex justify-end space-x-4">
                  <UButton
                    v-if="isEditing"
                    color="gray"
                    variant="outline"
                    @click="resetForm"
                  >
                    Отмена
                  </UButton>
                  <UButton
                    type="submit"
                    :loading="isLoading"
                    :disabled="!form.firstName || !form.lastName"
                  >
                    {{ isEditing ? 'Обновить' : 'Сохранить' }}
                  </UButton>
                </div>
              </UForm>
            </UCard>
          </div>

          <!-- Список сохраненных данных -->
          <div class="lg:col-span-1">
            <UCard>
              <template #header>
                <h3 class="text-lg font-semibold">Сохраненные данные</h3>
              </template>

              <div v-if="savedData.length === 0" class="text-center py-8">
                <UIcon name="i-lucide-database" class="w-12 h-12 text-gray-400 mx-auto mb-4" />
                <p class="text-gray-500">Нет сохраненных данных</p>
              </div>

              <div v-else class="space-y-4">
                <UCard
                  v-for="data in savedData"
                  :key="data.id"
                  class="hover:shadow-md transition-shadow"
                >
                  <div class="flex justify-between items-start">
                    <div>
                      <h4 class="font-medium">
                        {{ data.lastName }} {{ data.firstName }}
                      </h4>
                      <p class="text-sm text-gray-500">
                        {{ data.email || data.phone || 'Без контактов' }}
                      </p>
                      <p class="text-xs text-gray-400">
                        {{ new Date(data.createdAt).toLocaleDateString() }}
                      </p>
                    </div>
                    <div class="flex space-x-2">
                      <UButton
                        size="xs"
                        color="blue"
                        variant="ghost"
                        icon="i-lucide-edit"
                        @click="editData(data)"
                      />
                      <UButton
                        size="xs"
                        color="red"
                        variant="ghost"
                        icon="i-lucide-trash"
                        @click="deleteData(data.id)"
                      />
                    </div>
                  </div>
                </UCard>
              </div>
            </UCard>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>
