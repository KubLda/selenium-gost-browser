# selenium-gost-browser

Реализация браузеров для Selenium с поддержкой шифрования по ГОСТ.

[Selenium](https://github.com/SeleniumHQ/selenium)  WebDriver — это инструмент для автоматизации действий в веб‑браузере. Чаще всего его используют для тестирования веб‑приложений.

В Selenium Docker существует ограниченный набор браузеров, которые уже упакованы в готовые образы и доступны для запуска “из коробки”: Firefox, Chrome, Opera и Edge.

## Использование

1) Склонируйте репозиторий:

```bash
git clone https://github.com/KubLda/selenium-gost-browser
cd selenium-gost-browser
```

2) Перейдите в нужный каталог (пример: **NodeChromium / NodeYandex**):

```bash
cd NodeChromium
```

3) Соберите Docker-образ:

```bash
docker build -t node-chromium:146 ./NodeChromiumGost/
```

4) Запустите контейнер:

```bash
docker compose up -d
```
