google apps script GAS 

### WebStorm
* установить Node.js и npm
* в WebStorm создайте новый проект или откройте существующий
* Установите Google Apps Script CLI (clasp) `npm install -g @google/clasp`
* Авторизуйтесь в clasp `clasp login`
* клонируйте проект `clasp clone 1EQdGt_uGXQHsq-BYPQ90QPv7gx3d6Wt6nyxL_mYIdg-HjZiHnnqln1l9` - код Telegram-бота доступен локально в проекте WebStorm
* После внесения изменений `clasp push`
* Настройте Telegram-бот с использованием setWebhook: https://script.google.com/macros/s/1EQdGt_uGXQHsq-BYPQ90QPv7gx3d6Wt6nyxL_mYIdg-HjZiHnnqln1l9/exec
  + Telegram-боты требуют внешнего вебхука
  + у GAS есть встроенная поддержка вебхуков через doPost(e)
* для тестирования локально можно использовать инструменты ngrok или локальные симуляторы
* если Telegram-бот использует сторонние библиотеки (node-telegram-bot-api, ..), добавьте их в проект WebStorm для локальной разработки
  + такие зависимости нужно будет заменить встроенными функциями GAS (так как GAS не поддерживает npm)
