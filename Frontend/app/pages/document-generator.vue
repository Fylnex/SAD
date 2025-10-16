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

// Состояние
const selectedData = ref(null)
const selectedTemplate = ref(null)
const generatedDocument = ref('')
const isLoading = ref(false)

// Получаем сохраненные данные
const savedData = ref([])
const templates = ref([
  {
    id: 1,
    name: 'Заявление на отпуск',
    description: 'Стандартное заявление на предоставление отпуска',
    template: `
ЗАЯВЛЕНИЕ

Директору ООО "{{work.company}}"
{{work.position}} {{lastName}} {{firstName}} {{middleName}}

ЗАЯВЛЕНИЕ

Прошу предоставить мне ежегодный оплачиваемый отпуск продолжительностью 28 календарных дней с {{startDate}} по {{endDate}}.

Дата: {{currentDate}}
Подпись: _________________ {{lastName}} {{firstName}} {{middleName}}
    `
  },
  {
    id: 2,
    name: 'Справка с места работы',
    description: 'Справка о трудовой деятельности',
    template: `
СПРАВКА

Настоящая справка выдана {{lastName}} {{firstName}} {{middleName}} в том, что он(а) действительно работает в {{work.company}} в должности {{work.position}} с {{workStartDate}} по настоящее время.

Справка выдана для предъявления по месту требования.

Дата выдачи: {{currentDate}}
Директор: _________________
М.П.
    `
  },
  {
    id: 3,
    name: 'Договор аренды',
    description: 'Типовой договор аренды жилого помещения',
    template: `
ДОГОВОР АРЕНДЫ ЖИЛОГО ПОМЕЩЕНИЯ

г. {{address.city}}                                    {{currentDate}}

Арендодатель: {{landlordName}}, паспорт {{landlordPassport}}, проживающий по адресу: {{landlordAddress}}

Арендатор: {{lastName}} {{firstName}} {{middleName}}, паспорт {{passport.series}} {{passport.number}}, выдан {{passport.issuedBy}} {{passport.issueDate}}, проживающий по адресу: {{address.country}}, {{address.city}}, {{address.street}}, д. {{address.house}}, кв. {{address.apartment}}

Арендодатель сдает, а Арендатор принимает в аренду жилое помещение, расположенное по адресу: {{rentalAddress}}.

Срок аренды: с {{rentalStartDate}} по {{rentalEndDate}}
Размер арендной платы: {{rentalAmount}} рублей в месяц

Арендодатель: _________________
Арендатор: _________________
    `
  }
])

onMounted(() => {
  loadSavedData()
})

const loadSavedData = () => {
  const data = localStorage.getItem('sad_personal_data')
  if (data) {
    savedData.value = JSON.parse(data)
  }
}

const generateDocument = () => {
  if (!selectedData.value || !selectedTemplate.value) {
    useToast().add({
      title: 'Ошибка',
      description: 'Выберите данные и шаблон',
      color: 'red'
    })
    return
  }

  isLoading.value = true

  try {
    let document = selectedTemplate.value.template
    
    // Заменяем переменные в шаблоне
    const data = selectedData.value
    
    // Основные данные
    document = document.replace(/\{\{firstName\}\}/g, data.firstName || '')
    document = document.replace(/\{\{lastName\}\}/g, data.lastName || '')
    document = document.replace(/\{\{middleName\}\}/g, data.middleName || '')
    document = document.replace(/\{\{phone\}\}/g, data.phone || '')
    document = document.replace(/\{\{email\}\}/g, data.email || '')
    
    // Адрес
    document = document.replace(/\{\{address\.country\}\}/g, data.address?.country || '')
    document = document.replace(/\{\{address\.city\}\}/g, data.address?.city || '')
    document = document.replace(/\{\{address\.street\}\}/g, data.address?.street || '')
    document = document.replace(/\{\{address\.house\}\}/g, data.address?.house || '')
    document = document.replace(/\{\{address\.apartment\}\}/g, data.address?.apartment || '')
    
    // Паспорт
    document = document.replace(/\{\{passport\.series\}\}/g, data.passport?.series || '')
    document = document.replace(/\{\{passport\.number\}\}/g, data.passport?.number || '')
    document = document.replace(/\{\{passport\.issuedBy\}\}/g, data.passport?.issuedBy || '')
    document = document.replace(/\{\{passport\.issueDate\}\}/g, data.passport?.issueDate || '')
    
    // Работа
    document = document.replace(/\{\{work\.company\}\}/g, data.work?.company || '')
    document = document.replace(/\{\{work\.position\}\}/g, data.work?.position || '')
    document = document.replace(/\{\{work\.workPhone\}\}/g, data.work?.workPhone || '')
    document = document.replace(/\{\{work\.workEmail\}\}/g, data.work?.workEmail || '')
    
    // Системные переменные
    document = document.replace(/\{\{currentDate\}\}/g, new Date().toLocaleDateString('ru-RU'))
    
    // Дополнительные поля для конкретных шаблонов
    if (selectedTemplate.value.id === 1) {
      // Заявление на отпуск
      const startDate = prompt('Введите дату начала отпуска (ДД.ММ.ГГГГ):')
      const endDate = prompt('Введите дату окончания отпуска (ДД.ММ.ГГГГ):')
      document = document.replace(/\{\{startDate\}\}/g, startDate || '')
      document = document.replace(/\{\{endDate\}\}/g, endDate || '')
    }
    
    if (selectedTemplate.value.id === 2) {
      // Справка с места работы
      const workStartDate = prompt('Введите дату начала работы (ДД.ММ.ГГГГ):')
      document = document.replace(/\{\{workStartDate\}\}/g, workStartDate || '')
    }
    
    if (selectedTemplate.value.id === 3) {
      // Договор аренды
      const landlordName = prompt('Введите ФИО арендодателя:')
      const landlordPassport = prompt('Введите паспортные данные арендодателя:')
      const landlordAddress = prompt('Введите адрес арендодателя:')
      const rentalAddress = prompt('Введите адрес сдаваемого помещения:')
      const rentalStartDate = prompt('Введите дату начала аренды (ДД.ММ.ГГГГ):')
      const rentalEndDate = prompt('Введите дату окончания аренды (ДД.ММ.ГГГГ):')
      const rentalAmount = prompt('Введите размер арендной платы:')
      
      document = document.replace(/\{\{landlordName\}\}/g, landlordName || '')
      document = document.replace(/\{\{landlordPassport\}\}/g, landlordPassport || '')
      document = document.replace(/\{\{landlordAddress\}\}/g, landlordAddress || '')
      document = document.replace(/\{\{rentalAddress\}\}/g, rentalAddress || '')
      document = document.replace(/\{\{rentalStartDate\}\}/g, rentalStartDate || '')
      document = document.replace(/\{\{rentalEndDate\}\}/g, rentalEndDate || '')
      document = document.replace(/\{\{rentalAmount\}\}/g, rentalAmount || '')
    }
    
    generatedDocument.value = document
    
    useToast().add({
      title: 'Документ сгенерирован',
      color: 'green'
    })
  } catch (error) {
    useToast().add({
      title: 'Ошибка генерации',
      description: 'Попробуйте еще раз',
      color: 'red'
    })
  } finally {
    isLoading.value = false
  }
}

const downloadDocument = () => {
  if (!generatedDocument.value) return
  
  const blob = new Blob([generatedDocument.value], { type: 'text/plain;charset=utf-8' })
  const url = URL.createObjectURL(blob)
  const link = document.createElement('a')
  link.href = url
  link.download = `${selectedTemplate.value?.name || 'document'}.txt`
  document.body.appendChild(link)
  link.click()
  document.body.removeChild(link)
  URL.revokeObjectURL(url)
}

const printDocument = () => {
  if (!generatedDocument.value) return
  
  const printWindow = window.open('', '_blank')
  printWindow.document.write(`
    <html>
      <head>
        <title>Документ</title>
        <style>
          body { font-family: Arial, sans-serif; line-height: 1.6; margin: 20px; }
          pre { white-space: pre-wrap; }
        </style>
      </head>
      <body>
        <pre>${generatedDocument.value}</pre>
      </body>
    </html>
  `)
  printWindow.document.close()
  printWindow.print()
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
              Генерация документов
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
        <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
          <!-- Настройки генерации -->
          <div>
            <UCard>
              <template #header>
                <h2 class="text-xl font-semibold">Настройки генерации</h2>
              </template>

              <div class="space-y-6">
                <!-- Выбор данных -->
                <div>
                  <UFormGroup label="Выберите данные" required>
                    <USelect
                      v-model="selectedData"
                      :options="savedData.map(data => ({
                        label: `${data.lastName} ${data.firstName} ${data.middleName}`,
                        value: data
                      }))"
                      placeholder="Выберите сохраненные данные"
                    />
                  </UFormGroup>
                </div>

                <!-- Выбор шаблона -->
                <div>
                  <UFormGroup label="Выберите шаблон" required>
                    <USelect
                      v-model="selectedTemplate"
                      :options="templates.map(template => ({
                        label: template.name,
                        value: template
                      }))"
                      placeholder="Выберите шаблон документа"
                    />
                  </UFormGroup>
                  
                  <div v-if="selectedTemplate" class="mt-2">
                    <p class="text-sm text-gray-600 dark:text-gray-400">
                      {{ selectedTemplate.description }}
                    </p>
                  </div>
                </div>

                <!-- Кнопка генерации -->
                <UButton
                  @click="generateDocument"
                  :loading="isLoading"
                  :disabled="!selectedData || !selectedTemplate"
                  block
                  size="lg"
                >
                  Сгенерировать документ
                </UButton>
              </div>
            </UCard>

            <!-- Список шаблонов -->
            <UCard class="mt-6">
              <template #header>
                <h3 class="text-lg font-semibold">Доступные шаблоны</h3>
              </template>

              <div class="space-y-4">
                <div
                  v-for="template in templates"
                  :key="template.id"
                  class="p-4 border rounded-lg hover:bg-gray-50 dark:hover:bg-gray-800 cursor-pointer"
                  :class="{ 'border-primary bg-primary-50 dark:bg-primary-900/20': selectedTemplate?.id === template.id }"
                  @click="selectedTemplate = template"
                >
                  <h4 class="font-medium">{{ template.name }}</h4>
                  <p class="text-sm text-gray-600 dark:text-gray-400 mt-1">
                    {{ template.description }}
                  </p>
                </div>
              </div>
            </UCard>
          </div>

          <!-- Результат генерации -->
          <div>
            <UCard>
              <template #header>
                <div class="flex justify-between items-center">
                  <h2 class="text-xl font-semibold">Результат</h2>
                  <div v-if="generatedDocument" class="flex space-x-2">
                    <UButton
                      size="sm"
                      color="blue"
                      variant="outline"
                      icon="i-lucide-download"
                      @click="downloadDocument"
                    >
                      Скачать
                    </UButton>
                    <UButton
                      size="sm"
                      color="green"
                      variant="outline"
                      icon="i-lucide-printer"
                      @click="printDocument"
                    >
                      Печать
                    </UButton>
                  </div>
                </div>
              </template>

              <div v-if="!generatedDocument" class="text-center py-12">
                <UIcon name="i-lucide-file-text" class="w-16 h-16 text-gray-400 mx-auto mb-4" />
                <p class="text-gray-500">Документ не сгенерирован</p>
                <p class="text-sm text-gray-400 mt-2">
                  Выберите данные и шаблон, затем нажмите "Сгенерировать документ"
                </p>
              </div>

              <div v-else class="space-y-4">
                <div class="bg-gray-50 dark:bg-gray-800 p-4 rounded-lg">
                  <pre class="whitespace-pre-wrap text-sm font-mono">{{ generatedDocument }}</pre>
                </div>
              </div>
            </UCard>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>
