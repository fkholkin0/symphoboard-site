# symphoboard-site

Сайт проекта [[Symphoboard]] — афиши классических концертов Москвы.
Публикуется на GitHub Pages, домен **[shymphoboard.ru](https://shymphoboard.ru)**.

Продуктовый код (Chrome-расширение, n8n-workflow, GAS) живёт отдельно и приватно:
`github.com/fkholkin0/Symphoboard` → `~/Projects/Symphoboard`.
Здесь — **только сайт**. Секретов и бэкенд-конфигов тут быть не должно.

## Что сейчас

Заглушка (`index.html`): название, одно предложение о проекте, ссылка на канал,
блок «скоро здесь» про будущую инструкцию.

## Что дальше

Главный контент — HTML-инструкция **«как слушать музыку»**: с чего начать,
куда идти, что читать перед концертом, как читать программки.
Готовится отдельно; ляжет либо в `index.html`, либо в `/guide/index.html`
со ссылкой с главной.

## Как устроено

Статика без сборки. Никаких зависимостей, шрифты системные, стили инлайном
в `<head>`. Тёмная тема через `prefers-color-scheme`.

```
index.html    вся страница целиком
CNAME         кастомный домен для Pages — не удалять
.nojekyll     отключает Jekyll, чтобы Pages отдавал файлы как есть
```

## Работа с репой

```bash
cd ~/Projects/symphoboard-site

# посмотреть локально
python3 -m http.server 8000     # → http://localhost:8000

# выкатить
git add -A && git commit -m "..." && git push
```

**Пуш в `main` = деплой.** GitHub Pages пересобирает сайт сам,
обычно 30–60 секунд. Отдельного CI нет и не нужно.

Статус выкатки:

```bash
gh api repos/fkholkin0/symphoboard-site/pages/builds/latest --jq '.status, .error.message'
```

## Домен

`shymphoboard.ru` куплен через **Domenus** (NS: `ns1/ns2.domenus.ru`) —
DNS-записи правятся в панели регистратора, не здесь.

Записи, которые должны стоять на апексе:

| Тип | Имя | Значение |
|-----|-----|----------|
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |
| CNAME | www | fkholkin0.github.io. |

Проверка:

```bash
dig +short A shymphoboard.ru
gh api repos/fkholkin0/symphoboard-site/pages --jq '.html_url, .https_enforced, .protected_domain_state'
```

Домен и файл `CNAME` должны совпадать. Если поменять домен в настройках Pages
через веб-интерфейс, GitHub сам перепишет `CNAME` коммитом — тогда `git pull`
перед следующей правкой.
