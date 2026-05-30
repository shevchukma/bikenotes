# BikeNotes.ru — Инструкция по развёртыванию

## Структура сайта

```
site/
├── index.html      — Главная страница
├── reviews.html    — Обзоры велосипедов
├── routes.html     — Маршруты
├── tips.html       — Советы
├── css/
│   └── style.css
└── js/
    └── main.js

nginx/
└── bikenotes.ru.conf   — Конфигурация nginx
```

---

## Быстрое развёртывание

### 1. Установить nginx (если не установлен)

```bash
sudo apt update && sudo apt install nginx -y
```

### 2. Скопировать файлы сайта

```bash
sudo mkdir -p /var/www/bikenotes.ru
sudo cp -r site/* /var/www/bikenotes.ru/
sudo chown -R www-data:www-data /var/www/bikenotes.ru
sudo chmod -R 755 /var/www/bikenotes.ru
```

### 3. Подключить конфигурацию nginx

```bash
sudo cp nginx/bikenotes.ru.conf /etc/nginx/sites-available/bikenotes.ru
sudo ln -s /etc/nginx/sites-available/bikenotes.ru /etc/nginx/sites-enabled/
sudo nginx -t          # проверка конфига
sudo systemctl reload nginx
```

### 4. Проверить что сайт работает

```bash
curl -I http://localhost
# Должен вернуть: HTTP/1.1 200 OK
```

---

## SSL-сертификат (Let's Encrypt) — опционально

```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d bikenotes.ru -d www.bikenotes.ru
```

После этого раскомментировать HTTPS-блок в `nginx/bikenotes.ru.conf`
и закомментировать HTTP-блок.

---

## Проверка

- http://bikenotes.ru         → Главная
- http://bikenotes.ru/reviews.html → Обзоры
- http://bikenotes.ru/routes.html  → Маршруты
- http://bikenotes.ru/tips.html    → Советы

---

## Технические детали

- Чистый HTML5 + CSS3 + ванильный JS
- Нет внешних зависимостей кроме Google Fonts
- Все изображения — встроенные SVG (нет отдельных файлов изображений)
- Полностью адаптивный дизайн (мобильный, планшет, десктоп)
- Тёмная тема, акцент #e8ff00
