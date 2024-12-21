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
* `clasp open` открыть в браузере связанный проект
  + если открылся другой проект, перенастройте clasp: `clasp create --type standalone`
  + Или, если проект уже существует: `clasp settings`
  + Убедитесь, что ваш scriptId совпадает с ID нужного проекта
* иногда изменения не видны из-за кэширования
  + обновите страницу Google Apps Script (Ctrl + R)
* убедитесь, что вы работаете в правильной ветке
  + Если вы используете системы контроля версий (например, Git), убедитесь, что текущая ветка содержит последние изменения
* проблемы с синхронизацией
  + если проблема сохраняется, попробуйте перезапустить процесс:
  + `clasp pull` выгрузите текущий проект с Google Apps Script на локальный диск, это синхронизирует ваш локальный проект с удалённым
  + `clasp push` Загрузите изменения снова:
* лог ошибок
* `.clasp.json` в вашей рабочей директории должен содержать правильный scriptId
  + Если scriptId неверный, выполните `clasp setting scriptId <ваш_правильный_scriptId>`
* `clasp pull` чтобы проверить, есть ли расхождения между удалённым проектом и вашим локальным проектом
  + Если clasp pull загружает изменения, значит, локальные файлы не совпадают с удалёнными
* убедитесь, что аккаунт Google имеет полный доступ к проекту
  + Проверьте это в Google Apps Script, открыв проект и убедившись, что вы можете редактировать его
* очистите кэш clasp `clasp logout`, `clasp login`
* перепривяжите проект
  + сохраните копию ваших локальных файлов
  + удалите .clasp.json в рабочей директории
  + `clasp create --type standalone` или `clasp clone 1cvve2R0SWLlSWHePRXn0nDCGD6f-dpU83J9pj65JA09lIF5qyh3x3A_-`
  + переместите файлы обратно в рабочую директорию
* https://script.google.com/macros/s/1cvve2R0SWLlSWHePRXn0nDCGD6f-dpU83J9pj65JA09lIF5qyh3x3A_-/exec настройте Telegram-бот с использованием setWebhook
  + Telegram-боты требуют внешнего вебхука
  + у GAS есть встроенная поддержка вебхуков через doPost(e)
