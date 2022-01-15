# :computer: rootme :octocat:
Здесь Вы можете увидеть решения заданий с платформы rootme.
## Категории:
- [Web категория](#WEBкатегория)

## WEB категория
Я совместила две отдельных категории Веба в одну, т.к. - обычно они представляются одной категорией.
Совмещены будут 'Веб - сервер' и 'Веб - Клиент'.
#### ___Описание заданий с категорий___:
Эти задачи сталкивают вас с использованием языков сценариев и программирования на стороне клиента. В основном это скрипты, которые нужно анализировать и понимать. Это позволит вам изучить языки, которые широко используются в Интернете.
Эти задачи предназначены для обучения пользователей работе с HTML, HTTP и другими серверными механизмами. Следующая серия задач будет способствовать лучшему пониманию таких методов, как : Базовая работа нескольких механизмов аутентификации, обработка данных форм, внутренняя работа веб-приложений и т.д.
# 
# 
### `HTML - Source code`

Подсказка дается уже в самом название. Нам нужно зайти в код страницы. Попробуем поискать спрятанный флаг (пароль) и наткнемся на ЭТО:
![код страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/first_task.png)
# 
# 
### `HTTP - User-agent`

Название снова подсказыает нам на то место, куда смотреть. Поэтому мы должны воспользоваться Burp Suite и перехватить запрос с сайта. Теперь мы видим другой User-Agent (то, с какого браузера мы зашли). Меняем его на "admin":
![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/2task.png)
Форвардим запрос и перезагружаем страницу. Или кидаем его в Repeater и наживаем "Send". Покажу пример с Repeater:
![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/2_1task.png)
#
#
### `File upload - Double extensions`
В задании нужно прочесть ".passwd". А сделать это можно с помощью загрузки файла. Чем мы собственно и займемся.

![фото страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/file_upload_1.png)
# 
# 
### `Java - Server-side Template Injection`
Название задания говорит нам о том, что это шаблонная инъекция или же 'template injection'. Мы видим форму и сразу понимаем что нужно сделать что-то с ней. Ищем разные пэйлоады. И по методологии, пытаемся подобрать нужный нам шаблон. Я использовала этот репозиторий:
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/README.md
![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/ssti_1.png)

--тут позже допишу--
#
#
### `PHP - Filters`
Здесь нам нужно получить пароль админа. Тут у нас LFI, но не обычный, а с фильтром (об этом нам говорит название задания). Нам нужен php filter, для того, чтобы вывести содержимое файла.

https://kmb.cybber.ru/web/lfi/main.html - я воспользовалась этим сайтом и пэйлоадом оттуда.

![код страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_lfi_filter_1.png)

Теперь у нас есть содержимое файла, но в формате base64. В этот раз покажу как раскодировать строку через консоль.

![строка консоли](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_lfi_filter_2.png)

Здесь присутствует важная строка - "include(config.php)"; Именно она подсказывает нам, что нужно вместо файла "login.php", вставить "config.php" (делаем так же, как в прошлый раз). Мы все-таки же получаем строку base64. Все-так же идем в консоль.

![строка консоли](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_lfi_filter_3.png)

Теперь мы видим внутренность ".passwd". Остался последний шаг этого задания. Снова декодировать в base64.

![строка консоли](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_lfi_filter_4.png)

Теперь мы получили наш ответ и можем заливать его на сайт.
#
#
### `Local File Inclusion`

В задании говорится, что нам нужно зайти в секцию админа. А так же то, что уязвимость формата LFI. Перейдя по ссылке на задание, мы видим то, что страница имеет секции, попробуем перейти по ним. И видим что переход осуществляется через параметр. Так же видим файл index.html, который находится в открытом доступе как и остальные.

![сайт](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_lfi_1.png)

Так как это LFI, можно попытаться постаивить в параметр слово "admin". Но у нас ничего не выйдет, поэтому попытаемся сделать самый легкий вариант LFI'я, это вставить "../", можно просто оставить "../", но я сразу допишу "admin"

![сайт](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_lfi_2.png)

У нас появился файл "index.html", попробуем перейти в этот файл и поищим что-то похожее на флаг. А вот и он.

![сайт](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_lfi_3.png)
#
#
### `Local File Inclusion - Double encoding`
Нам нужно найти пароль в исходных файлах сайта. Получается попробуем "php://filter/convert.base64-encode/resource=cv", чтобы прочесть файл. И кажется тут фильтр, атаку распознали.

![скрин сайта](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_lfi_double_1.png)

И так, теперь мы понимаем что оно реагирует на спец.знаки. Значит используем URL decode, но дублируем кодировку два раза, так как задание дает нам это понять (double).

![скрин сайта](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_lfi_double_2.png)

И так, теперь у нас есть исходник страницы. Но с ходу можно сказать, что это все base64. Значит декодируем.

![скрин сайта](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_lfi_double_3.png)

Декодирую я снова все через консоль. `<?php include("conf.inc.php"); ?>` - дает нам понять, что прочесть нужно именно файл "conf". 

![скрин сайта](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_lfi_double_4.png)

Теперь меняем прошлый пэйлоад со слова "cv" на "conf". И получаем все такой же base64 код. Осталось снова декодировать.

![скрин сайта](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_lfi_double_5.png)

Снова показываю пример декодирования через консоль. А вот и наш обещанный флаг. 
# 
# 
### `PHP - Serialization`
Естественно в этом задании нужно обращать внимание на код, который нам дан, так что при выполнении задания я обращалась именно к нему (white box).

![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/php_ser.png)

В задании нужно получить доступ админа. Поэтому запомним это и перейдем на страницу задания. Тут по условию вводим данные гостя (guest) и не забываем нажать галочку о том, чтобы нас запомнили, это понадобится для создания нашей сессии. Собственно, сама сессия нужна для ее подмены под данные суперадмина.

![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/php_ser_2.png)

Теперь видим, что мы действительно смогли зайти под данными гостя (guest). Следующим шагом предлагаю перейти в куки, то место, где создалась наша сессия. Если что, утилита называется "EditThisCookie" (та, что на скрине).

![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/php_ser_3.png)

Тут сразу видно, что это URL кодировка. Время использовать мой любимый "CyberShef". 
![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/php_ser_4.png)

Но давайте разберем структуру этого куки подробнее. __Все будет описано в примере ниже.__ Во первых нужно отметить то, что "s" означает "size" (размер). Т.е. - нам нужно изменить это при смене "quest" на "superadmin". Так же последнее значение тоже поменялось, с сигнала "s" на "b", что означает boolean (лог. 1 или 0). Вставляем единицу, т.к. нам нужно true.

![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/php_ser_5.png)

Теперь дисконектимся (отключаемся) и видим вот это. Но не забывайте сохранить измененные куки (иначе будете как я сидеть 30 минут и не понимать, что не так).

![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/php_ser_6.png)
# 
# 
### `JSON Web Token (JWT) - Introduction`
Нам дается регистрационная страница и мы видим опцию "Login as Guest!". Делаем мы это для того, чтобы нам присвоился jwt токен.

![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_jwt_1.png)

Пользуемся моим любимым расширением "EditThisCookie" (то, что на скрине). И теперь пробуем раскопать то, что нам дано.

![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_jwt_2.png)

Поговорим о структуре jwt токенов. То, что обозначено фиолетовым - base64. Остальная же часть - нам не нужна, отбрасываем. Теперь понимаме, что по заданию нам нужно подменить значения, из-за доступа админа. Так что из первой фиолетовой части убираем шифрование и ставим значение "none", а во второй фиолетовой части меняем значение на "admin", но не забываем перевести все в base64.

![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_jwt_3.png)


{"typ":"JWT","alg":"none"} = "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0="

{"username":"admin"} = "eyJ1c2VybmFtZSI6ImFkbWluIn0="

![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_jwt_4.png)

Убираем знак "=" на конце, так как это лишний хвост. Вот что получилось в итоге.

![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_jwt_5.png)

Сохраняем, перезагружаем страницу и флаг наш :)
# 
# 
##
## 
##
##
##
##
##
##
##
##
## Networks категория
#### ___Описание заданий с категорий___:

Следующий набор проблем касается сетевого трафика, включая различные протоколы. Вам необходимо проанализировать захваты пакетов для решения этих проблем.
Предпосылки:
Знание инструмента анализа сетевых захватов.
Знание наиболее распространенных сетевых протоколов.
# 
# 
### `FTP - authentication`
Мы работаем с файлов .pcap, а это означате что мы можем открыть его в Wireshark. По условию нам нужно работать с FTP и найти пароль.
![скрин С ftp пакетом](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/wire1.png)
Так что находим самый первый пакет FTP и наживаем TCP-stream. А теперь ищем место с паролем.
![скрин с ответом](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/wire1-2.png)
# 
#
### `TELNET - authentication`
Здесь делаем практически такой же алгоритм, что и в предыдущем задании (FTP - authentication). 
![скрин с ответом](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/wire1-2.png)
# 
# 
### `ETHERNET - frame`
Нам дается .txt файл и открыв его, мы видим то, что это hex'ы. Поэтому открываем любой декодер, в моем случае это CyberShef. И переводим hex'ы в текст.
![скрин с ответом](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/network_cyber_1.png)
Теперь мы видим кодировку base64, и опять же, просто переводим в обычный текст. Заливаем ответ.
#
#
### `Twitter authentication`
Здесь нам дается файл расширения .pcap, а значит интуитивно заходим в Wireshark. И видим всго один пакет.
![скрин с ответом](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/network_wire_2.png)

Выбираем "follow TCP" и нам открывается такое окно. Как мы видим, у запроса есть поле авторизации. Копируем текст оттуда и переводим base64 в текст.
#
#
### `Bluetooth - Unknown file`
В описании дается понять, что мы имеем дело с файлом Bluetooth. Воспользовавшись командой "cat", мы можем посмотреть то, что находится внутри файла и увидим расширение BTSnoop. Теперь мы можем воспользоваться Wireshark, так как он поддерживает работу с файлами связанными с Bluetooth. Теперь заходим во вкладку "Wireless" и переходим в "Bluetooth devices".

![что нам выдает файл](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/device.png)

А теперь просто переводим в sha1, но ___НЕ ЗАБЫВАЕМ ПЕРЕВЕСТИ В ВЕРХНИЙ РЕГИСТР БУКВЫ MAC АДРЕССА___.
#
#
### `DNS - zone transfert`
Здесь мы работаем с DNS, а если быть точнее, нам дается: хост, порт и домен. Значит в голову приходит команда dig.

![результат команды dig](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/networks_dns.png)

Теперь осталось создать нормальную команду, чтобы извлечь то, что нам нужно. Поэтому предлагаю использовать axfr, которая даст нам расширенную информацию о запрашиваемом домене. Осталось только забрать флаг.
