google apps script GAS 

### WebStorm
* установить Node.js и npm
* в WebStorm создайте новый проект или откройте существующийн
* `npm install -g @google/clasp` Google Apps Script CLI 
* `clasp login` авторизуйтесь в clasp 
* `clasp clone 1EQdGt_uGXQHsq-BYPQ90QPv7gx3d6Wt6nyxL_mYIdg-HjZiHnnqln1l9`
* `clasp push` после внесения изменений 
* https://script.google.com/macros/s/1EQdGt_uGXQHsq-BYPQ90QPv7gx3d6Wt6nyxL_mYIdg-HjZiHnnqln1l9/exec настройте Telegram-бот с использованием setWebhook
  + Telegram-боты требуют внешнего вебхука
  + у GAS есть встроенная поддержка вебхуков через doPost(e)
