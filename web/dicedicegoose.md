# dicedicegoose

***

## Description

```
Follow the leader.

ddg.mc.ax
```
NOTE : managed to solve this *after* the ctf.

***

## Solution

ddg.mc.ax we're taken to a website with a game thingy going on.\
![image](https://github.com/IC3lemon/diceCTF-2024/assets/150153966/a403a6c0-af1d-4d01-8881-15a0469aadb1)

we are the dice, and the black square is the goose.\
we reach the goose, we win and get some score.\
![image](https://github.com/IC3lemon/diceCTF-2024/assets/150153966/2cd296b8-008e-4c34-88ed-bbdb4c19276f)

the score is the number of moves you make, I came to realise.\
I then went through the source code, where I saw this :\

![image](https://github.com/IC3lemon/diceCTF-2024/assets/150153966/604745a9-1ff4-42af-8853-512e94d85e5a)

so we gotta get a score of 9 to get the flag.\
that's only possible if we move straight down 8 blocks and blacky comes right at us.\
like this:
![image](https://github.com/IC3lemon/diceCTF-2024/assets/150153966/8c338fe7-87a4-42b2-bd94-229ca051a99b)


I searched the code for how blacky's movements worked and found this:\
![image](https://github.com/IC3lemon/diceCTF-2024/assets/150153966/5e27c198-55ee-41c4-9202-9231f6342187)\
here, it seems like bro's movements are being randomised by a switch.\
but we want bro to come right to us as we go down.\
So I overrided the html to remove the switch and make it go to the left always.\
![image](https://github.com/IC3lemon/diceCTF-2024/assets/150153966/f452710e-f715-4132-a030-61ccd39cae99)

and sure enough 

![image](https://github.com/IC3lemon/diceCTF-2024/assets/150153966/9f708ef7-6059-4d23-9a31-473efc0d76a8)

![image](https://github.com/IC3lemon/diceCTF-2024/assets/150153966/2f0a752a-dd02-4815-908e-760d6a286ce1)


ez

***

## Flag > `dice{pr0_duck_gam3r_AAEJCQEBCQgCAQkHAwEJBgQBCQUFAQkEBgEJAwcBCQIIAQkB}`

***
