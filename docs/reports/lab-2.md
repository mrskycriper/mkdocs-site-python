# Практика 2

Автор: Салимов Сергей

---

## Отчёт

### 1. Обновим метаданные проекта

Файл `mkdocs.yml`

```yml
site_name: MkDocs песочница
site_description: Этот сайт используется для изучения веб-разработки на Python с использованием MkDocs.
site_author: Салимов Сергей
copyright: '2024'

repo_url: https://github.com/mrskycriper/mkdocs-site-python
```

### 2. Создаём новую директорию для темы

``` bash
mkdir custom_theme
cd custom_theme
```

### 3. Создаём корневой файл темы

```bash
touch main.html
```

### 4. Подключаем тему в конфиге

Для этого в файле `mkdocs.yml` необходимо добавить тему через ключ `theme`. Если тема является самостоятельной, то поле `name` будет содержать значение `null`. В данном случае унаследуем стандарную тему указав `name: mkdocs`. Указываем директорию для нашей темы через ключ `custom_dir`.

Параметр `color_mode` отвечает за поведение цветовой темы на сайте, установим в `auto` для автоматического переключения в тёмную тему и обратно. Флаг `user_color_mode_toggle` позволяет отобразить ручное управление темой для пользователя.

```yml
theme:
  name: mkdocs
  color_mode: auto
  user_color_mode_toggle: true
  custom_dir: 'custom_theme/'
```

### 5. Заполняем коневой файл темы

Унаследуем содержимое от стандартной темы через вставку при помощи шаблонизатора.

```html
{% extends "base.html" %}
```

При наследовании от темы мы можем выборочно заменить различные блоки, основным способом для этого служит конструкция (замена заголовка вкладки):

```html
{% block htmltitle %}
<title>{{ config.site_name }}</title>
{% endblock %}
```

Шаблон `{{ config.site_name }}` позволяет нам обратиться к соответствующему полю из конфига проекта и вставить его как строку.

Полный список возможных замен можно найти в [исходном коде темы](https://github.com/mkdocs/mkdocs/blob/master/mkdocs/themes/mkdocs/base.html).

### 6. Продолжаем кастомизацию

Уберём стандартные кнопки дял поска и навигации по страницам

```html
{%- block search_button %}

{%- endblock %}

{%- block next_prev %}

{%- endblock %}
```

### 7. Заменим footer на кастомный

```html
{% block footer %}
<hr>
<p>
    {{config.site_author}}, {{config.copyright}}<br>
    {{config.site_description}}
</p>
<hr>
{% endblock %}
```

### 8. Подключаем css

Создаём новый файл в директории темы `css/overrides.css`.

Шаблон `{{ super() }}` позволяет унаследовать содержимое это блока и расширить его.

```html
{% block styles %}
{{ super() }}
<link href="{{ 'css/overrides.css'|url }}" rel="stylesheet">
{% endblock %}
```

Добавляем стили

```css
:root {
    --bs-font-sans-serif: "Inter";
}

:root, [data-bs-theme="light"] {
    /*Material Purple 300*/
    --bs-primary: #BA68C8;
    --bs-primary-rgb: 186, 104, 200;

    /*Material Deep Purple 300*/
    --bs-link-color: #9575CD;
    --bs-link-color-rgb: 149, 117, 205;

    /*Material Deep Purple 400*/
    --bs-link-hover-color: #7E57C2;
    --bs-link-hover-color-rgb: 126, 87, 194;
}

[data-bs-theme="dark"] {
    /*Material Purple 400*/
    --bs-primary: #AB47BC;
    --bs-primary-rgb: 171, 71, 188;

    /*Material Deep Purple 300*/
    --bs-link-color: #9575CD;
    --bs-link-color-rgb: 149, 117, 205;

    /*Material Deep Purple 400*/
    --bs-link-hover-color: #7E57C2;
    --bs-link-hover-color-rgb: 126, 87, 194;
}

body::before {
    background: none;
}

.navbar.bg-primary {
    background-image: none;
}

.content-block {
    border: #7E57C2 2px;
}

footer {
    font-weight: 400;
}
```