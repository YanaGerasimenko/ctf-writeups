# :computer: picoCTF :shipit:
Здесь Вы можете увидеть решения заданий с платформы rootme (https://picoctf.org/)
## Категории:
- [Web категория](#WEBкатегория)

## WEB категория
#### ___Описание заданий с категорий___:
В этой категории нужно решать задания на тематику уязвимостей сайтов, поиск инъекций, внедрение кода и т.д
# 
# 
### `JaWT Scratchpad`
В задании написано, что нужно посмотреть блокнот админа. И давайте дня начала зайдем на сайт.

![код страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/picoCTF/pics/jwt_1.png)

Теперь понимаем, что нужно авторизоваться. Делаем это и видим наш блокнот, как обычного пользователя.

![код страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/picoCTF/pics/jwt_2.png)

Тепрь заходим в куки и что же тут? Правильно - JWT токен.

![код страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/picoCTF/pics/jwt_3.png)

Теперь разберем его. Понимаем что Base64 часть, мы сможем подменить. А так же есть подписанная часть, вот ее нам нужно сломать.

![код страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/picoCTF/pics/jwt_4.png)

Как я поняла, авторы хотят чтобы мы использовали John The Ripper. Но я буду показывать на примере другого инструмента - jwt_tool.

![код страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/picoCTF/pics/jwt_5.png)

И так, путем переборов у нас нашелся пароль. Вы можете видеть его на скрине. Следующий этап - формирование нового токена. используем Питон.

![код страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/picoCTF/pics/jwt_6.png)

И так, у нас все готово, осталось загрузить новый токен в куки и перезагрузить страницу. Собственно все, а вот и наш флаг.

![код страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/picoCTF/pics/jwt_7.png)




## Cryptography категория
#### ___Описание заданий с категорий___:
В этой категории нужно решать задания на тематику шифрования, кодирования и т.д
# 
# 
### `Mod 26`
В описании нам дают понять, что у нас простой шифр сдвига цезаря (это можно понять по строке, которая дана). Так же нам говорят про "ROT13". Так что это громадная подсказка, про сдвиг, так что можем воспользоваться любым онлайн редактором. К примеру - https://www.dcode.fr/caesar-cipher (а можно и вовсе вручную). Теперь уже к самому решению, просто выставляем цифру 13 и ждем решения :)

![код страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/picoCTF/pics/ceaser_1.png)
# 
# 
# 
# 
## Reverse категория
#### ___Описание заданий с категорий___:
Category about cracking apps, binary files and etc.
# 
# 
### `asm1`
<+0>:	push   ebp

<+1>:	mov    ebp,esp

<+3>:	cmp    DWORD PTR [ebp+0x8],0x71c	
