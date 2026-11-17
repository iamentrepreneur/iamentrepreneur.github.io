# ARCHITECTURE — техническое устройство

## Стек
- Astro (актуальная стабильная версия на момент инициализации).
- Tailwind CSS (актуальная стабильная версия, официальный способ интеграции с Astro).
- TypeScript только там, где он действительно полезен; минимальный клиентский JavaScript для темы и мобильной навигации.
- Контент: Markdown или Astro Content Collections (предпочтительно коллекция для проектов, чтобы типизировать метаданные и избежать дублирования).
- GitHub Actions + GitHub Pages, статическая генерация, без сервера.

## URL
- `/` — русская главная (без редиректа и без дублирующего `/ru/`).
- `/projects/dogspace/`, `/projects/coffee-platform/`, `/projects/relayo/`, `/projects/hh-radar/`, `/projects/bitrix-partner-crm/` — русские кейсы.
- `/about/` — о разработчике.
- `/en/`, `/en/projects/<slug>/`, `/en/about/` — английские аналоги.
- Для GitHub Pages user-site `base: '/'`, `site: 'https://iamentrepreneur.github.io'`.
- Канонические URL и hreflang для RU/EN, sitemap по необходимости.

## Рекомендуемая структура
```text
src/
  components/
    Header.astro
    Footer.astro
    ThemeToggle.astro
    LanguageSwitch.astro
    ProjectCard.astro
  content/
    projects/
      ru/
        dogspace.md
        coffee-platform.md
        relayo.md
        hh-radar.md
        bitrix-partner-crm.md
      en/
        ...
  layouts/
    BaseLayout.astro
    CaseStudyLayout.astro
  pages/
    index.astro
    about.astro
    projects/[slug].astro
    en/
      index.astro
      about.astro
      projects/[slug].astro
  styles/
    global.css
public/
  favicon.svg
.github/workflows/
  deploy.yml
```
Структура ориентировочная: адаптировать под версию Astro Content Collections, избегая лишних слоев и дублирования. Данные о проектах должны иметь один источник истины для каждого языка.

## Модель контента
Для каждого проекта: `slug`, `locale`, `title`, `subtitle`, `summary`, `role`, `status`, `featured`, `order`, `stack`, `challenge`, `architecture`, `decisions`, `quality`, `currentState`, `nextSteps`. Технические разделы можно хранить в Markdown body, не пытаться жестко типизировать каждый абзац.

## Сборка и публикация
- `npm run dev`, `npm run build`, `npm run preview`.
- GitHub Actions: checkout, setup-node, npm ci, Astro build, upload-pages-artifact, deploy-pages; permissions `pages: write`, `id-token: write` и concurrency.
- Не публиковать автоматически, пока репозиторий и GitHub Pages не настроены владельцем.
- Убедиться, что build генерирует корректные маршруты и относительные пути к ассетам.

## SEO и безопасность
- Уникальные title/description, Open Graph без вымышленных изображений, canonical, hreflang, robots.txt, sitemap при необходимости.
- Не добавлять стороннюю аналитику, cookies, формы, CMS, авторизацию, API.
- Внешние ссылки с `rel="noopener noreferrer"` при `target="_blank"`.
- Не хранить секреты, персональные данные третьих лиц, внутренние ссылки на приватные репозитории.

## Проверки
- Успешная статическая сборка.
- Все RU/EN маршруты существуют и внутренние ссылки не битые.
- Theme switch работает с клавиатуры, сохраняется и не вызывает flash.
- Проверка ширин 320, 375, 768, 1024, 1440px.
- Lighthouse: ориентир >= 90 по Performance, Accessibility, Best Practices, SEO на типовых страницах (не заявлять без реального прогона).
- Проверка `prefers-reduced-motion`, заголовков, контраста и отсутствия горизонтального скролла.
