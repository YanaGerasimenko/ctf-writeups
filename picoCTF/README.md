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
asm1:
		<+0>:	push   ebp				0x8be
    
		<+1>:	mov    ebp,esp				ebp = 0x8be	esp = 0x8be
    
		<+3>:	cmp    DWORD PTR [ebp+0x8],0x71c	if asm1(0x8be) > 0x71c == true (true) 		jump to 37 line
    
	<+10>:	jg     0x512 <asm1+37>		
  
	<+12>:	cmp    DWORD PTR [ebp+0x8],0x6cf
  
	<+19>:	jne    0x50a <asm1+29>
  
	<+21>:	mov    eax,DWORD PTR [ebp+0x8]
  
	<+24>:	add    eax,0x3
  
	<+27>:	jmp    0x529 <asm1+60>
  
	<+29>:	mov    eax,DWORD PTR [ebp+0x8]
  
	<+32>:	sub    eax,0x3
  
	<+35>:	jmp    0x529 <asm1+60>
  
		<+37>:	cmp    DWORD PTR [ebp+0x8],0x8be	if 0x8be != 0x8be == true (false)		jump to 46 line	
    
		<+44>:	jne    0x523 <asm1+54>			
    
		<+46>:	mov    eax,DWORD PTR [ebp+0x8]		eax = 0x8be
    
		<+49>:	sub    eax,0x3				eax = 0x8be - 0x3 = 0x8bb
    
	<+52>:	jmp    0x529 <asm1+60>		
  
	<+54>:	mov    eax,DWORD PTR [ebp+0x8]		
  
	<+57>:	add    eax,0x3			
  
		<+60>:	pop    ebp			
    
		<+61>:	ret    



	Answer: 0x8bb
