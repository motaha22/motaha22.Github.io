---
title: "Rverse Crackme_1_by_taliesin"
classes: wide
header:
  teaser: /assets/images/crackmes/logo.jpg
ribbon: DodgerBlue
description: "Deep Static Analysis Reverse for crackme_1_by_taliesin"
categories:
  - bugbounty
toc: true
---

### About Challenge : 

| Language  | Platform             | Difficulty | Quality | Arch |
| --------- | -------------------- | ---------- | ------- | ---- |
| Assembler | Windows 2000/XP only | 2.0        | 4.0     | x86  |

[To Download](https://crackmes.one/crackme/5ab77f5333c5d40ad448c0d0)

 

### Analysis :

when we run the Challenge  and write any thing it will pop this 

![](/assets/images/crackmes1/1.PNG)

we will see if it packed or not we will see sections and entropy and PE by DIE 

![](/assets/images/crackmes1/2.PNG)

![](/assets/images/crackmes1/3,1.PNG)

we can see it's not packed so we going to static analysis we will use ida pro

![](/assets/images/crackmes1/3.PNG)

On the first screen, we will see 3 functions and we will go through every one   

#### first one :

![](/assets/images/crackmes1/4.PNG)

we will see it try to detect debugger it uses "repne scasb" instruction to search for 0xcc which is used by the debugger to make a breakpoint after that it jump to 4014F5h so we will look for it 

![](/assets/images/crackmes1/5.PNG)

it is bad boy 🙂

so will go back to our function and pach this jump to jnz and go back and go to anthor function

#### second one :

##### like first one 

![](/assets/images/crackmes1/6.PNG)

#### third one :

we will see that it calling 3 functions and there is like encoding string but we will go through every one and it take strings from text box

​	![](/assets/images/crackmes1/7.PNG)

###### 	first one :

​		we will see it calculate sum of string and divide by 18h and put the Remainder in the offset 40304fh

​		then back to function

​		![](/assets/images/crackmes1/8.PNG)

​		it also detect the debugger 

​			![](/assets/images/crackmes1/11.PNG)

###### 	second one :

​		in this we will see 4 sections 

​				1: it checks is serial is 10 char or not And if not it will go to bad boy 

​				2: and then it will calculate sum of string and divide by 9h and put the result at offset 40304Ah 

​				![](/assets/images/crackmes1/9.PNG)

​				3:we will see it checks first char in serial by this serial [0] = key[f]

​					f = 40304fh 

​					key = ZWATRQLCGHPSXYENVBJDFKMU

​					we will see it checks third char in serial by this serial [2] = key[f+f]

​				4: then it will check from 4:9 char According to the equation 

​					serial [i=4..9] = key[f+c-41h]

​						c = prev serial char 

​				![](/assets/images/crackmes1/10.PNG)

###### 	third one :

​					it checks the last char

​					![](/assets/images/crackmes1/12.PNG)

so we found the algo for serial 

f = 40304fh (user divide by 18h and put the Remainder in the offset 40304fh)

key = ZWATRQLCGHPSXYENVBJDFKMU

serial [1] = key[f mod 18h] 

serial [2]  = 'E'

serial [3]  = key[f+f mod 18h] 

serial [i=4..9] = key[f+c-41h]

c = prev serial char

serial [10] = 40304Ah (sum of 1-9 char in serial and divide by 9h and put the result at offset 40304Ah)

i have wrote the keygen by python 

```python
sum_user = input("enter name :")
key = "ZWATRQLCGHPSXYENVBJDFKMU"
sum_indec = 0
for i in range(len (sum_user)):
     sum_indec=sum_indec+ord(sum_user[i])
serial =[]
mod_indec = sum_indec % 24
mod_indec2 = mod_indec
sum_indecSERIAL=0
for i in range (9):
    if (i==1):
         serial.append("E")
         print(serial)
    elif (i==0):
        if (mod_indec2 >=24):
            mod_indec2 = int(mod_indec2 /24)
        serial.append(key[mod_indec2])
        print(serial)
    elif (i==2):
        mod_indec2 = mod_indec +mod_indec
        if (mod_indec2 >24):
            mod_indec2 = int(mod_indec2 /24)
        serial.append(key[mod_indec2])
        print(serial)
 
    else :
        
        print(serial)
        prev = ord(serial[i-1])
        coll = mod_indec2+prev-65
        if (coll >24):
            coll = int (coll /24)
        serial.append(key[coll])
        mod_indec2 = coll
      

for i in range(0,len (serial)):
    sum_indecSERIAL=sum_indecSERIAL+ord(serial[i])
sum_indecSERIAL = int(sum_indecSERIAL / 9)
serial.append(chr(sum_indecSERIAL))
print ("serial for user [sum_user] is : "+"".join(serial))

```

for test 

user : hi 

serial :BEPWUDMPWM



![](/assets/images/crackmes1/13.PNG)

