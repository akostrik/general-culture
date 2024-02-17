# Languages
* Cairo, initiation https://github.com/shramee/starklings-cairo1, https://book.cairo-lang.org/
* [C++](https://github.com/akostrik/CPP_modules_42)
* [Python](https://github.com/akostrik/Python-for-Data-Science)

# Blokchain
* starkware  
* starknet
* onlyDust
* https://www.youtube.com/channel/UCl6oyLa4CblZRurgwZwpgPQ 

## Starknet
* secority scaling technologie
* Starknet France: https://t.me/+LreNgBuiV2VmZjVk
* Explorer Starknet: https://voyager.online/, https://starkscan.co/
* Starknet docs: https://docs.starknet.io/documentation/
* Starknet community forum: https://community.starknet.io/
* CoreStars (le plus gros groupe Starknet dev de la planete): https://t.me/sncorestars
* OnlyDust: https://t.me/+n3C1mzmYORs5MDZk
* Only dust: https://onlydust.com
* Quelques labo proposant des stages: KSS (https://github.com/keep-starknet-strange), Kasar (https://github.com/kasarlabs), LambdaClass (https://github.com/lambdaclass), Nethermind (https://github.com/nethermindeth), Carbonable (https://github.com/carbonable-labs) etc

<details open>
#  <summary>GIT</summary>
 
* `git add` внести изменения в индекс (временное хранилище)
* `git commit -a -m "message"` только для отслеживаемых файлов, Untracked files игнорируются
* откатить (сместить указатель на один коммит назад)
    + git hist
    + git reset --hard HEAD~1 
    + git push -f origin master принудительный коммит
* откатить (оставить нетронутыми индекс и дерево файлов проекта, вернуться к работе с указанным коммитом)
    + git reset --hard HEAD~1 
    + git reset --soft 
    + git push -f origin master принудительный коммит
* откатить (убрать локальные изменения, возвращает к последнему push)
   + `git checkout .`
* отменяйте изменения безопасным способом
   + `git stash save`
* откат
   + `git log`
   + `git revert <commit>` 
   + https://ru.stackoverflow.com/questions/431520/%D0%9A%D0%B0%D0%BA-%D0%B2%D0%B5%D1%80%D0%BD%D1%83%D1%82%D1%8C%D1%81%D1%8F-%D0%BE%D1%82%D0%BA%D0%B0%D1%82%D0%B8%D1%82%D1%8C%D1%81%D1%8F-%D0%BA-%D0%B1%D0%BE%D0%BB%D0%B5%D0%B5-%D1%80%D0%B0%D0%BD%D0%BD%D0%B5%D0%BC%D1%83-%D0%BA%D0%BE%D0%BC%D0%BC%D0%B8%D1%82%D1%83
* `reset` переместит то, на что указывает HEAD
* `checkout` изменяется сам HEAD
* `git log`
* `git log --name-status` список файлов коммита
* `gît commit "filename"` много информации
* `git fsck --lost-found` Каждый найденный объект будет выложен в папку .git/lost-found/other/ в том же виде, в каком и был (т.е. не blob), но с sha1 соответствующего blob'a вместо имени - таким образом, содержимое файлов восстановить можно, имена - нельзя
* alias in .gitconfig
## submodule
* https://github.com/chaconinc/DbConnector
* submodules are not downloaded when you git clone
* git видит его как конкретный коммит этого репозитория, не отслеживает его содержимое пока вы не перейдёте в него
* `git submodule add`
* клинирование с субмодулями 
    + 1 way
       - git clone --recurse-submodules, инициализирует и обновит каждый подмодуль
    + 2 way
       - git clone (вместо подмодуля пустая папка)
       - git submodule init инициализация лок конфиг файла (появяется .gitmodules)
       - git submodule update получение всех данных этого проекта и извлечения соответствующего коммита, указанного в основном проекте
    + 3 way
       - `git clone`
       - `git submodule update --init`
    + 4 way
       - `git submodule update --init --recursive`
* проверить наличие изменений в подмодуле:
   + `cd libft`
   + `git fetch`
   + `git merge` для обновления локальной версии отслеживаемой ветки
* опубликовать изменения в подмодуле
    + `cd libft`
    + `git pull origin master`
* un truc comme la mlx tu utilises un gestionnaire de paquet (vcpkg, conan)

# Tools, applications, technical details
* не запускается chrome или brave
`
rm -rf ~/.config/google-chrome/Singleton*
rm -rf ~/.config/BraveSoftware/Brave-Browser/SingletonLock
`
* не запускается vscode: `rm -rf ~/.cache // vscode` 
* редакторы кода: VScode, subl
* VS Code "editor.insertSpaces": true
* https://account.jetbrains.com/licenses  
* на студ компах Докер и веб сервер устанавливать в виртуалку, а саму виртуалку в Sgoinfre (не goinfre, которая привязана к конкретному компу, а Sgoinfre, которая работает на всех станциях и хранит файлы несколько месяцев)
* Practice the exam just like you would in the real exam https://github.com/JCluzet/42_EXAM
</details>

<details open>
<summary>Regular expression</summary>

## basic regular expressions (BRE)
* Традиционные регулярные выражения UNIX
* определён POSIX'ом как устаревший, но он до сих пор широко распространён из соображений обратной совместимости
* `.`
* `|[ ]`
* `[^ ]`
* `^` (действует только в начале выражения)
* `$` (действует только в конце выражения)
* `*`
* `\{ \}` = { }
* `\( \)` = ( )
* `\n`
* #include <regex.h> + the default regular expression type
 
## Extended regular expressions (ERE)
* Отменено `\{ \}`* `\( \)`
* Обратная косая черта перед метасимволом отменяет его специальное значение
* Отвергнута теоретически нерегулярная конструкция \n
* `+`
* `?`
* `|`
* #include <regex.h> + regcomp(..., REG_EXTENDED)

## Perl Compatible Regular Expressions (PCRE) 
...
##  Unicode regular expressions
...
</details>


# AI for code
* copilot 10€ в месяц, 1 месяц бесплатно, связан с github  
* tabnine 3 месяца бесплатно, не связан с github

# Verify a project
   + условие
   + evaluation
   + valgrind
   + warnings 
   + каждая операция - а что, если не пройдёт
   + аргументы, вводимые данные
      - INT_MAX
      - 0
      - "\0"
      - ""
      - NULL
      - EOF
      - BUFSIZE > 0
      - "-3"
      - "03" "0"
      - --3
      - "21474833649"
      - 2 5 6 + 3
      - 2\ a\
      - "2 5 6+3"
      - "" ""
      - "+" "++"
      - буквы вместо чисел
      - echo "*22" | ./a.out
   + valgrind, особенно для ошибок
   + norminette в тч libft
   + header vscode ctrl alt H, vim fn+F1, Emacs ctrl + c -> ctrl + h
   + libft не подмодуль
   + убрать запрещённые функции (в т.ч. printf) в т.ч. из libft
   + удалить gitignore, mlx, чекеры, libft/readme, скрытые файлы
   + -Wall -Wextra -Werror
   + клонировать и проверить 

# 42 useful liens
* **peer finder** https://find-peers.codam.nl/Paris
* https://rphlr.github.io/42-Evals/ больше не работает
* https://meta.intra.42.fr/articles/title-level-7

# Dictionnary
* snapshot : the state of a system at a particular point in time
