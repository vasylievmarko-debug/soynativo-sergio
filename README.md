# SOY NATIVO — landing page

Статический лендинг онлайн-школы языков. Один HTML-файл + скомпилированный
Tailwind CSS, без бандлера.

## Структура

```
public/
  index.html      ← страница
  styles.css      ← скомпилированный Tailwind (артефакт билда, лежит в git)
src/
  input.css       ← вход для tailwind: @import + @source
package.json
```

## Команды

```bash
npm install      # один раз
npm run build    # одноразовая сборка styles.css
npm run watch    # пересобирать styles.css при изменении HTML
npm run serve    # локальный http-сервер на http://127.0.0.1:8765
npm run dev      # watch + serve в одном
```

## Что было исправлено при импорте

1. **Сломанный `<script>` в `<head>`.** Был тег `<script src="https://cdn.tailwindcss.com">…inline JS…</script>` —
   по HTML-спеке inline-содержимое игнорируется, когда задан `src`. Функции были
   мёртвым кодом (продублированы внизу страницы). Многие CMS / HTML-санитайзеры
   при виде такой конструкции выбрасывают `<script>` целиком — и тогда Tailwind
   не загружается, страница теряет всё оформление. Это самый вероятный сценарий
   «в браузере открывается, на сайте CSS поломан».
2. **Tailwind Play CDN заменён на скомпилированный `styles.css`.** Сами разработчики
   Tailwind пишут «не использовать в production». Компилируем заранее → 29 КБ
   статического CSS вместо 400 КБ JS-компилятора в рантайме.

## Что ещё стоит держать в голове

- `animation-timeline: view()` (класс `.fade-in-on-scroll`) — фича scroll-driven
  animations. Полная поддержка только Chrome 115+/Edge. В Safari ≤17 и старом
  Firefox таймлайн игнорируется и анимация просто отрабатывает на загрузке —
  визуально иначе, но не блокер.
- Кастомные классы `.feature-card`, `.feature-icon`, `.feature-title`,
  `.feature-text`, `.choose-teacher-btn`, `.modal-choose-btn`, `.teachers-grid`,
  `.handwriting-text` определены в inline `<style>` без префикса. Если будешь
  встраивать страницу в существующий сайт с собственным CSS — могут быть
  конфликты по этим именам. Безопаснее переименовать в `sn-feature-card` и т.п.
- Шрифт `Brush Script MT` (для рукописной надписи «вместе с нами») есть на
  Mac/Windows, но не на Linux/Android. На Linux падает в дефолтный `cursive` —
  выглядит иначе.
