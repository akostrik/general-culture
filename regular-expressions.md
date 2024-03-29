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
