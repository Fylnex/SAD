<script setup lang="ts">
definePageMeta({
  layout: false
})

const user = ref('')
const isAuth = ref(false)

// Проверяем авторизацию
onMounted(() => {
  const authStatus = localStorage.getItem('sad_auth')
  const userEmail = localStorage.getItem('sad_user')
  
  if (authStatus === 'true' && userEmail) {
    isAuth.value = true
    user.value = userEmail
  } else {
    navigateTo('/auth')
  }
})

const handleLogout = () => {
  localStorage.removeItem('sad_auth')
  localStorage.removeItem('sad_user')
  navigateTo('/auth')
}

const menuItems = [
  {
    label: 'Управление данными',
    icon: 'i-lucide-database',
    to: '/data-management',
    description: 'Добавляйте и редактируйте персональные данные'
  },
  {
    label: 'Генерация документов',
    icon: 'i-lucide-file-text',
    to: '/document-generator',
    description: 'Создавайте документы из шаблонов'
  },
  {
    label: 'Шаблоны документов',
    icon: 'i-lucide-layout-template',
    to: '/templates',
    description: 'Управление шаблонами документов'
  },
  {
    label: 'История документов',
    icon: 'i-lucide-history',
    to: '/history',
    description: 'Просмотр созданных документов'
  }
]
</script>

<template>
  <div class="min-h-screen bg-gray-50 dark:bg-gray-900">
    <!-- Header -->
    <header class="bg-white dark:bg-gray-800 shadow">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex justify-between items-center py-6">
          <div class="flex items-center">
            <h1 class="text-2xl font-bold text-gray-900 dark:text-white">
              SAD Dashboard
            </h1>
          </div>
          <div class="flex items-center space-x-4">
            <span class="text-sm text-gray-600 dark:text-gray-400">
              {{ user }}
            </span>
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
      </div>
    </header>

    <!-- Main Content -->
    <main class="max-w-7xl mx-auto py-6 sm:px-6 lg:px-8">
      <div class="px-4 py-6 sm:px-0">
        <div class="mb-8">
          <h2 class="text-3xl font-bold text-gray-900 dark:text-white mb-2">
            Добро пожаловать в SAD
          </h2>
          <p class="text-gray-600 dark:text-gray-400">
            Система Автозаполнения Документов - ваш помощник в создании документов
          </p>
        </div>

        <!-- Menu Grid -->
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
          <UCard
            v-for="item in menuItems"
            :key="item.label"
            class="hover:shadow-lg transition-shadow cursor-pointer"
            @click="navigateTo(item.to)"
          >
            <template #header>
              <div class="flex items-center space-x-3">
                <UIcon :name="item.icon" class="w-6 h-6 text-primary" />
                <h3 class="text-lg font-semibold">{{ item.label }}</h3>
              </div>
            </template>

            <p class="text-gray-600 dark:text-gray-400">
              {{ item.description }}
            </p>

            <template #footer>
              <UButton
                color="primary"
                variant="ghost"
                size="sm"
                trailing-icon="i-lucide-arrow-right"
                block
              >
                Перейти
              </UButton>
            </template>
          </UCard>
        </div>

        <!-- Quick Stats -->
        <div class="mt-12">
          <h3 class="text-xl font-semibold text-gray-900 dark:text-white mb-6">
            Быстрая статистика
          </h3>
          <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
            <UCard>
              <div class="text-center">
                <div class="text-3xl font-bold text-primary mb-2">0</div>
                <div class="text-sm text-gray-600 dark:text-gray-400">
                  Сохраненных данных
                </div>
              </div>
            </UCard>
            <UCard>
              <div class="text-center">
                <div class="text-3xl font-bold text-primary mb-2">0</div>
                <div class="text-sm text-gray-600 dark:text-gray-400">
                  Созданных документов
                </div>
              </div>
            </UCard>
            <UCard>
              <div class="text-center">
                <div class="text-3xl font-bold text-primary mb-2">0</div>
                <div class="text-sm text-gray-600 dark:text-gray-400">
                  Шаблонов документов
                </div>
              </div>
            </UCard>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>
