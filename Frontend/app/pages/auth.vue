<script setup lang="ts">
definePageMeta({
  layout: false
})

const email = ref('')
const password = ref('')
const isLoading = ref(false)
const error = ref('')

const handleLogin = async () => {
  if (!email.value || !password.value) {
    error.value = 'Пожалуйста, заполните все поля'
    return
  }

  isLoading.value = true
  error.value = ''

  try {
    // Имитация авторизации (без API)
    await new Promise(resolve => setTimeout(resolve, 1000))
    
    // Простая проверка для демонстрации
    if (email.value === 'admin@sad.ru' && password.value === 'admin') {
      // Сохраняем состояние авторизации в localStorage
      localStorage.setItem('sad_auth', 'true')
      localStorage.setItem('sad_user', email.value)
      
      // Перенаправляем на главную страницу
      await navigateTo('/dashboard')
    } else {
      error.value = 'Неверный email или пароль'
    }
  } catch (err) {
    error.value = 'Ошибка авторизации'
  } finally {
    isLoading.value = false
  }
}

// Проверяем, авторизован ли пользователь
onMounted(() => {
  const isAuth = localStorage.getItem('sad_auth')
  if (isAuth) {
    navigateTo('/dashboard')
  }
})
</script>

<template>
  <div class="min-h-screen bg-gray-50 dark:bg-gray-900 flex flex-col justify-center py-12 sm:px-6 lg:px-8">
    <div class="sm:mx-auto sm:w-full sm:max-w-md">
      <div class="text-center">
        <h1 class="text-3xl font-bold text-gray-900 dark:text-white">
          SAD
        </h1>
        <p class="mt-2 text-sm text-gray-600 dark:text-gray-400">
          Система Автозаполнения Документов
        </p>
      </div>
    </div>

    <div class="mt-8 sm:mx-auto sm:w-full sm:max-w-md">
      <div class="bg-white dark:bg-gray-800 py-8 px-4 shadow sm:rounded-lg sm:px-10">
        <form class="space-y-6" @submit.prevent="handleLogin">
          <div>
            <UFormGroup label="Email" required>
              <UInput
                v-model="email"
                type="email"
                placeholder="admin@sad.ru"
                :disabled="isLoading"
              />
            </UFormGroup>
          </div>

          <div>
            <UFormGroup label="Пароль" required>
              <UInput
                v-model="password"
                type="password"
                placeholder="admin"
                :disabled="isLoading"
              />
            </UFormGroup>
          </div>

          <UAlert
            v-if="error"
            color="red"
            variant="soft"
            :title="error"
            class="mb-4"
          />

          <div>
            <UButton
              type="submit"
              block
              size="lg"
              :loading="isLoading"
              :disabled="!email || !password"
            >
              Войти в систему
            </UButton>
          </div>

          <div class="text-center">
            <p class="text-sm text-gray-600 dark:text-gray-400">
              Демо-доступ: admin@sad.ru / admin
            </p>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>
