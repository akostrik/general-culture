google apps script GAS 

### WebStorm
* подготорвка
  + установить Node.js и npm
  + в WebStorm создайте новый проект или откройте существующийн
  + `npm install -g @google/clasp` Google Apps Script CLI 
  + `clasp login` авторизуйтесь в clasp 
  + `clasp clone 1EQdGt_uGXQHsq-BYPQ90QPv7gx3d6Wt6nyxL_mYIdg-HjZiHnnqln1l9`
  + включите опцию Apps Script API в аккаунте https://script.google.com/home/usersettings
* `clasp push` после внесения изменений
* `clasp open` открыть в браузере связанный проект
  + если открылся другой проект, перенастройте clasp: `clasp create --type standalone`
  + Или, если проект уже существует: `clasp settings`
  + Убедитесь, что ваш scriptId совпадает с ID нужного проекта
* Иногда изменения не видны из-за кэширования
  + обновите страницу Google Apps Script (Ctrl + R)
* Убедитесь, что вы работаете в правильной ветке
  + Если вы используете системы контроля версий (например, Git), убедитесь, что текущая ветка содержит последние изменения
* проблемы с синхронизацией
  + если проблема сохраняется, попробуйте перезапустить процесс:
  + `clasp pull` выгрузите текущий проект с Google Apps Script на локальный диск, это синхронизирует ваш локальный проект с удалённым
  + `clasp push` Загрузите изменения снова:
* лог ошибок
  + `clasp push --verbose` более подробный вывод команд для clasp
* https://script.google.com/macros/s/1EQdGt_uGXQHsq-BYPQ90QPv7gx3d6Wt6nyxL_mYIdg-HjZiHnnqln1l9/exec настройте Telegram-бот с использованием setWebhook
  + Telegram-боты требуют внешнего вебхука
  + у GAS есть встроенная поддержка вебхуков через doPost(e)
