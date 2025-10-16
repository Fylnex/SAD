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
              Шаблоны документов
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
        <div class="text-center py-12">
          <UIcon name="i-lucide-layout-template" class="w-16 h-16 text-gray-400 mx-auto mb-4" />
          <h2 class="text-2xl font-semibold text-gray-900 dark:text-white mb-2">
            Управление шаблонами
          </h2>
          <p class="text-gray-600 dark:text-gray-400 mb-8">
            Эта функция будет доступна в следующих версиях
          </p>
          <UButton
            color="primary"
            @click="navigateTo('/dashboard')"
          >
            Вернуться в дашборд
          </UButton>
        </div>
      </div>
    </main>
  </div>
</template>
