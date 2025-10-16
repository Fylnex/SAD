---
seo:
  title: SAD - Система Автозаполнения Документов
  description: Создавайте и заполняйте документы автоматически с помощью SAD.
---

::u-page-hero{class="dark:bg-gradient-to-b from-neutral-900 to-neutral-950"}
---
orientation: horizontal
---
#top
:hero-background

#title
Система [Автозаполнения Документов]{.text-primary}.

#description
Создавайте, заполняйте и генерируйте документы автоматически. Удобная система для работы с шаблонами документов и персональными данными.

#links
  :::u-button
  ---
  to: /getting-started
  size: xl
  trailing-icon: i-lucide-arrow-right
  ---
  Начать работу
  :::

  :::u-button
  ---
  icon: i-lucide-login
  color: neutral
  variant: outline
  size: xl
  to: /auth
  ---
  Войти в систему
  :::

#default
  :::prose-pre
  ---
  code: |
    export default defineNuxtConfig({
      modules: [
        '@nuxt/ui',
        '@nuxt/content',
        'nuxt-og-image',
        'nuxt-llms'
      ],

      css: ['~/assets/css/main.css']
    })
  filename: nuxt.config.ts
  ---

  ```ts [nuxt.config.ts]
  export default defineNuxtConfig({
    modules: [
      '@nuxt/ui',
      '@nuxt/content',
      'nuxt-og-image',
      'nuxt-llms'
    ],

    css: ['~/assets/css/main.css']
  })
  ```
  :::
::

::u-page-section{class="dark:bg-neutral-950"}
#title
Возможности системы SAD

#links
  :::u-button
  ---
  color: neutral
  size: lg
  target: _blank
  to: https://ui.nuxt.com/docs/getting-started/installation/nuxt
  trailingIcon: i-lucide-arrow-right
  variant: subtle
  ---
  Explore Nuxt UI
  :::

#features
  :::u-page-feature
  ---
  icon: i-lucide-palette
  ---
  #title
  Управление данными

  #description
  Добавляйте, редактируйте и храните персональные данные в удобном интерфейсе. Все данные защищены и доступны только вам.
  :::

  :::u-page-feature
  ---
  icon: i-lucide-type
  ---
  #title
  Шаблоны документов

  #description
  Создавайте и используйте готовые шаблоны документов. Поддерживаются различные форматы: договоры, заявления, справки.
  :::

  :::u-page-feature
  ---
  icon: i-lucide-layers
  ---
  #title
  Автозаполнение

  #description
  Автоматическое заполнение документов данными из базы. Просто выберите шаблон и получите готовый документ.
  :::

  :::u-page-feature
  ---
  icon: i-lucide-search
  ---
  #title
  Экспорт документов

  #description
  Экспортируйте готовые документы в различных форматах: PDF, Word, печать. Удобная работа с результатами.
  :::

  :::u-page-feature
  ---
  icon: i-lucide-navigation
  ---
  #title
  Безопасность

  #description
  Все ваши данные надежно защищены. Система авторизации и шифрования обеспечивает конфиденциальность информации.
  :::

  :::u-page-feature
  ---
  icon: i-lucide-moon
  ---
  #title
  Удобный интерфейс

  #description
  Интуитивно понятный интерфейс с поддержкой темной и светлой темы. Адаптивный дизайн для всех устройств.
  :::
::

::u-page-section{class="dark:bg-neutral-950"}
#title
Enhanced with Nuxt Content

#links
  :::u-button
  ---
  color: neutral
  size: lg
  target: _blank
  to: https://content.nuxt.com/docs/getting-started/installation
  trailingIcon: i-lucide-arrow-right
  variant: subtle
  ---
  Explore Nuxt Content
  :::

#features
  :::u-page-feature
  ---
  icon: i-simple-icons-markdown
  ---
  #title
  MDC Enhanced Markdown

  #description
  Write in Markdown while embedding Vue components. Seamlessly integrate interactive elements in your content.
  :::

  :::u-page-feature
  ---
  icon: i-lucide-file-text
  ---
  #title
  File-based Routing

  #description
  Organize content in folders and files. Your documentation structure automatically becomes your navigation.
  :::

  :::u-page-feature
  ---
  icon: i-lucide-code
  ---
  #title
  Syntax Highlighting

  #description
  Beautiful code blocks with language detection, line numbers, and copy buttons. Support for 100+ languages.
  :::

  :::u-page-feature
  ---
  icon: i-lucide-database
  ---
  #title
  Content Database

  #description
  Query your content with a MongoDB-like API. Filter, sort, and search through your documentation programmatically.
  :::

  :::u-page-feature
  ---
  icon: i-lucide-file-code
  ---
  #title
  Frontmatter Support

  #description
  Add metadata to your content files. Define SEO tags, navigation properties, and custom fields.
  :::

  :::u-page-feature
  ---
  icon: i-lucide-git-branch
  ---
  #title
  Version Control

  #description
  Content lives in your repository. Branch, review, and deploy documentation alongside your code.
  :::
::

::u-page-section{class="dark:bg-gradient-to-b from-neutral-950 to-neutral-900"}
  :::u-page-c-t-a
  ---
  links:
    - label: Start building
      to: '/getting-started'
      trailingIcon: i-lucide-arrow-right
    - label: View on GitHub
      to: 'https://github.com/nuxt-ui-templates/docs'
      target: _blank
      variant: subtle
      icon: i-simple-icons-github
  title: Ready to build an amazing documentation?
  description: Join thousands of developers building with Nuxt and Nuxt UI. Get this template and start shipping today.
  class: dark:bg-neutral-950
  ---

  :stars-bg
  :::
::
