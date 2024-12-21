### google apps script GAS 
* https://api.telegram.org/bot7088040447:AAEXX5w49fwRe1GSLRYSHWNLXGmTsZEtrH0/setWebhook?url=https://script.google.com/macros/s/AKfycbx4Ss3cRjh-BMwk7j9DUwC4aFWjQRxrwBwTlQCPgEfEhqM7MSUGplA9uwwB3Bcjz0b1HQ/exec

### WebStorm + GAS
* подготорвка
  + установить Node.js и npm
  + в WebStorm создайте новый проект или откройте существующийн
  + `npm install -g @google/clasp` Google Apps Script CLI 
  + `clasp login` авторизуйтесь в clasp 
  + `clasp clone 1cvve2R0SWLlSWHePRXn0nDCGD6f-dpU83J9pj65JA09lIF5qyh3x3A_-`
  + включите опцию Apps Script API в аккаунте https://script.google.com/home/usersettings
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
