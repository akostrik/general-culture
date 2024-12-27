### GAS + bot
* данные о боте
  + https://api.telegram.org/bot7088040447:AAEXX5w49fwRe1GSLRYSHWNLXGmTsZEtrH0/getMe 
* открыть проект
  + https://script.google.com
  + https://script.google.com/home/projects/1cvve2R0SWLlSWHePRXn0nDCGD6f-dpU83J9pj65JA09lIF5qyh3x3A_-/edit 
  + если не открывается
    - удалить `script.google.com` chrome://settings/content/all?searchSubpage=script.google.com
    - при этом браузер выйдет из всех аккаунтов
    - зайти в аккаунт gmail скрипта в первую очередь, потом в другие аккаунты
* установить вебхук 3 способа:
  + запустить setWebhook()
  + https://api.telegram.org/bot7088040447:AAEXX5w49fwRe1GSLRYSHWNLXGmTsZEtrH0/setWebhook?url=https://script.google.com/macros/s/AKfycbx4Ss3cRjh-BMwk7j9DUwC4aFWjQRxrwBwTlQCPgEfEhqM7MSUGplA9uwwB3Bcjz0b1HQ/exec
  + curl -X POST "https://api.telegram.org/bot7088040447:AAEXX5w49fwRe1GSLRYSHWNLXGmTsZEtrH0/setWebhook?url=<YOUR_WEB_APP_URL>"
  + вебхуки для Telegram-бота хранятся на серверах Telegram
* проверить вебхук 2 способа
  + https://api.telegram.org/bot7088040447:AAEXX5w49fwRe1GSLRYSHWNLXGmTsZEtrH0/getWebhookInfo 
  + curl -X GET "https://api.telegram.org/bot7088040447:AAEXX5w49fwRe1GSLRYSHWNLXGmTsZEtrH0/getWebhookInfo"
* проверить веб-приложение
  + `Logger.log` + Журналы выполнения в GAS
  + https://script.google.com/macros/s/AKfycbxzOMMj9U7v1S39gSAWheUAXza0Uur9Iq7frxyv72bu4bRbPaTJrfHHtjSKCPeKqLE4EQ/exec
    - подставить свой номер deployment
    - для этого способа нужна doGet
  + `curl -X POST -H "Content-Type: application/json" -d '{ "message": { "chat": { "id": "123456789" }, "text": "/start" }}' "https://script.google.com/macros/s/AKfycbzTXYBip6t-KTkbXS7pEIuzD5AeO-yfOxGeR0ZGY2GAjSrFxtVK6V9Ey93R6lMRU_tLbA/exec"` POST-запрос к приложению, проверить, что сервер обрабатывает запросы
  + 1. GAS получает запрос на опубликованный URL `https://script.google.com/macros/s/.../exec`
  + 2. GAS перенаправляет вас на более защищённый автоматически сгенерированный URL `https://script.googleusercontent.com/macros/...`, который нужно использовать для дальнейших взаимодействий
    - `curl` не следует за перенаправлением по умолчанию
    - `-L` = следовать за HTTP-перенаправлениями
`curl -L -X POST -H "Content-Type: application/json" -d '{ "message": { "chat": { "id": "123456789" }, "text": "/start" }}' "https://script.google.com/macros/s/AKfycbxyo2QY35hBoqVcO2covVwL2hn0fwEJDjC4lhIisy4o2AMoIKKBVdl8JD1M3wj1YyElaA/exec"`
    - 2 способ: вручную скопировать URL, на который перенаправлялся запрос (указан в строке `The document has moved <A HREF="...">here</A>`). Это будет ссылка вида `https://script.googleusercontent.com/macros/echo?user_content_key=...&lib=...` 
curl -X POST -H "Content-Type: application/json" -d '{ "message": { "chat": { "id": "123456789" }, "text": "/start" }}' "https://script.googleusercontent.com/macros/echo?user_content_key=nM9VSALqZRaab9lvktmthZD2SL0lrKRHWXEK8eecVuQrKAZr53rbDbx2U2s0zRZh_dXUGNKr1iy-Cu2lh53jGjy_2lF9HcdKm5_BxDlH2jW0nuo2oDemN9CCS2h10ox_1xSncGQajx_ryfhECjZEnAeNKeVMYg7RmTzN6269Br1F1__-25ENFyRwvHxg54i06eghjkFevHQo-Xj3QBTODYgvqABq-NwWaxa96bt2h6zguZJn6SkkOdz9Jw9Md8uu&amp;lib=MFNBI_7VgapKedRPJnmNJb7m4tjEUa1_m"

### WebStorm + GAS
* подготорвка
  + установить Node.js и npm
  + в WebStorm создайте новый проект или откройте существующийн
  + `npm install -g @google/clasp` Google Apps Script CLI 
  + `clasp login` авторизуйтесь в clasp 
  + `clasp clone 7088040447:AAEXX5w49fwRe1GSLRYSHWNLXGmTsZEtrH0`
  + включите Apps Script API в аккаунте https://script.google.com/home/usersettings
* после внесения изменений
  + `clasp push`
    - иногда изменения не видны из-за кэширования, очистите кэш clasp `clasp logout`, `clasp login`
  + https://script.google.com/macros/s/1cvve2R0SWLlSWHePRXn0nDCGD6f-dpU83J9pj65JA09lIF5qyh3x3A_-/exec настройте Telegram-бот с использованием setWebhook
  + Telegram-боты требуют внешнего вебхука
  + у GAS есть встроенная поддержка вебхуков через doPost(e)
* встроенный ИИ WebStorm: автозаполнение, анализа код, рефакторинг
  + не "понимают" задания на естественном языке
* обработка естественного языка = интегрировать OpenAI API или использовать сторонние плагины
  + плагин: WebStorm > File > Settings > Plugins > плагин ChatGPT или CodeGPT, установите плагин, перезапустите WebStorm
  + выделить участок кода, кликнуть правой кнопкой и выбрать "Generate Documentation", "Explain Code", "Complete Code"
  + открыть окно ИИ и писать задания словами ("Напиши функцию на JavaScript для сортировки массива", ...)
  + если требуется API-ключ, получите его на OpenAI и введите в настройках плагина
  + возможность автоматического изменения кода напрямую в файлах зависит от функционала плагина и его настроек
  + Вы можете задать вопрос или попросить сгенерировать/оптимизировать код. ChatGPT выдаст готовый блок, который вы можете вручную вставить в нужное место
  + Некоторые плагины позволяют автоматически изменять код в вашем файле. Для этого обычно нужно:
    - выделить участок кода
    - выбрать команду, например, "Refactor this code" или "Optimize this code with AI"
    - AI предложит конкретные изменения в виде патча или сразу применит их в файл, если плагин поддерживает эту функцию
    - включите разрешение на внесение изменений в файлы через настройки плагина
    - убедитесь, что в проекте есть резервное копирование (AI может вносить ошибки)
* отладка
  + `clasp open` открыть в браузере связанный проект
  + лог ошибок
  + перезапустить процесс
    - `clasp pull` выгрузит проект с GAS на локальный диск, это синхронизирует локальный проект с удалённым
    - `clasp push` загрузит изменения снова
  + `clasp pull` проверить, есть ли расхождения между удалённым проектом и вашим локальным проектом
  + перепривяжите проект
    - сохраните копию ваших локальных файлов
    - удалите .clasp.json в рабочей директории
    - `clasp create --type standalone` или `clasp clone 1cvve2R0SWLlSWHePRXn0nDCGD6f-dpU83J9pj65JA09lIF5qyh3x3A_-`
    - переместите файлы обратно в рабочую директорию
