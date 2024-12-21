google apps script GAS 

### WebStorm
* подготорвка
  + установить Node.js и npm
  + в WebStorm создайте новый проект или откройте существующийн
  + `npm install -g @google/clasp` Google Apps Script CLI 
  + `clasp login` авторизуйтесь в clasp 
  + `clasp clone 1cvve2R0SWLlSWHePRXn0nDCGD6f-dpU83J9pj65JA09lIF5qyh3x3A_-`
  + включите опцию Apps Script API в аккаунте https://script.google.com/home/usersettings
* `clasp push` после внесения изменений
  + иногда изменения не видны из-за кэширования, очистите кэш clasp `clasp logout`, `clasp login`
* `clasp open` открыть в браузере связанный проект
* лог ошибок
* перезапустить процесс
  + `clasp pull` выгрузит проект с GAS на локальный диск, это синхронизирует локальный проект с удалённым
  + `clasp push` загрузит изменения снова
* `clasp pull` проверить, есть ли расхождения между удалённым проектом и вашим локальным проектом
* встроенный ИИ WebStorm: автозаполнение, анализа код, рефакторинг
  + не "понимают" задания на естественном языке
* интеграция OpenAI (например ChatGPT) - обработка естественного языка
  + интегрировать OpenAI API или использовать сторонние плагины
  + Установка и настройка плагина ChatGPT:
    - WebStorm > File > Settings > Plugins > плагин ChatGPT или CodeGPT, установите плагин, перезапустите WebStorm
  + выделить участок кода, кликнуть правой кнопкой и выбрать "Generate Documentation", "Explain Code", "Complete Code"
  + открыть окно ИИ и писать задания словами ("Напиши функцию на JavaScript для сортировки массива", "Объясни, как работает этот кусок кода")
  + если требуется API-ключ, получите его на OpenAI и введите в настройках плагина
* перепривяжите проект
  + сохраните копию ваших локальных файлов
  + удалите .clasp.json в рабочей директории
  + `clasp create --type standalone` или `clasp clone 1cvve2R0SWLlSWHePRXn0nDCGD6f-dpU83J9pj65JA09lIF5qyh3x3A_-`
  + переместите файлы обратно в рабочую директорию
* https://script.google.com/macros/s/1cvve2R0SWLlSWHePRXn0nDCGD6f-dpU83J9pj65JA09lIF5qyh3x3A_-/exec настройте Telegram-бот с использованием setWebhook
  + Telegram-боты требуют внешнего вебхука
  + у GAS есть встроенная поддержка вебхуков через doPost(e)
