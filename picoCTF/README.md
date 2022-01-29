# :computer: picoCTF :shipit:
Здесь Вы можете увидеть решения заданий с платформы rootme (https://picoctf.org/)
## Категории:
- [Web категория](#WEBкатегория)

## WEB категория
#### ___Описание заданий с категорий___:
В этой категории нужно решать задания на тематику уязвимостей сайтов, поиск инъекций, внедрение кода и т.д.
# 
# 
### `JaWT Scratchpad`
В задании написано, что нужно посмотреть блокнот админа. И давайте дня начала зайдем на сайт.

![код страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/picoctf/pics/jwt_1.png)

Теперь понимаем, что нужно авторизоваться. Делаем это и видим наш блокнот, как обычного пользователя.

![код страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/picoctf/pics/jwt_2.png)

Тепрь заходим в куки и что же тут? Правильно - JWT токен.

![код страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/picoctf/pics/jwt_3.png)

Теперь разберем его. Понимаем что Base64 часть, мы сможем подменить. А так же есть подписанная часть, вот ее нам нужно сломать.
![код страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/picoctf/pics/jwt_4.png)
Как я поняла, авторы хотят чтобы мы использовали John The Ripper. Но я буду показывать на примере другого инструмента - jwt_tool.
![код страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/picoctf/pics/jwt_5.png)
И так, путем переборов у нас нашелся пароль. Вы можете видеть его на скрине. Следующий этап - формирование нового токена. используем Питон.
![код страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/picoctf/pics/jwt_6.png)
И так, у нас все готово, осталось загрузить новый токен в куки и перезагрузить страницу. Собственно все, а вот и наш флаг.
![код страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/picoctf/pics/jwt_7.png)
