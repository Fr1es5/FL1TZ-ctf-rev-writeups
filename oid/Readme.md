# Oid challenge
## Description:  don't panic ! look for the "input" and start from there ! good luck
## Tools required
- **Cyberchef, Python**
## Steps to solve
-  Using Cyberchef online with the **from oid to hex** operation we can convert the oid to hex then we apply “Magic”. The Magic operation attempts to detect various properties of the input data and suggests which operations could help to make more sense of it. Thanks to it we obtain this particular
gibberish text that I took interest in:
 ```
 ]<>[---------------------------------------------------------------------------------------------<]>-<-[<]->++++<[++++++++>>,]<>[-----<]>-<-[<]->++<[+++++++++++++++++++++++>>,]<>[-----------------------------------------------------------------<]>-<-[<]->++<[+++++++++++++++++++>>,]<>[----------------------<]>-<-[<]->+++++<[++++++>>,]<>[---------------------------------------------------------------------<]>-<-[<]->++++++<[++++++++>>,]<>[----------------------------------------------------------------<]>-<-[<]->+++<[+++++++++++++>>,]<>[--------------------------------------------------------------<]>-<-[<]->++++++<[++++++++>>,]<>[------<]>-<-[<]->++<[+++++++++++++++++++++++>>,]<>[---------------------------------------------------------<]>-<-[<]->+++<[+++++++++++++++++>>,]<>[---------------------------<]>-<-[<]->++++<[+++++++++++++++++>>,]<>[---------------------------------------------------------------------------------<]>-<-[<]->++<[+++++++++++++++++>>,]<>[+++++++++++++++++++<]>-<-[<]->++++<[+++++++++++++++++>>,]<>[----------------------------------------------------------------------<]>-<-[<]->++<[+++++++++++++++++>>,]<>[------------------------<]>-<-[<]->++++++<[++++++++++>>,]<>[-----------------------------------------------------<]>-<-[<]->++++++<[+++++++>>,]<>[------------------------------------------------------------<]>-<-[<]->++<[+++++++++++++++++++++++++++++>>,]<>[-----------------------------------------------------------------<]>-<-[<]->++++<[+++++++++++++>>,]<>[----------------------------------------<]>-<-[<]->++++++<[++++++>>,]<>[-----------------------------------------------------------<]>-<-[<]->++++++<[++++++>>,]<>[---------------<]>-<-[<]->++<[+++++++++++++++++++++++++++++>>,]<>[------------------------------------------------------<]>-<-[<]->+++<[+++++++++++++++++++++++>>,]<>[------------------------------------------------------<]>-<-[<]->++++++<[++++++>>,]<>[------------------------------<]>-<-[<]->++++++<[+++++++++>>,]<>[+++++++++++++++++++<]>-<-[<]->++++<[+++++++++++++++++>>,]<>[--------------------------------------------<]>-<-[<]->++++<[++++++++>>,]<>[----------<]>-<-[<]->++++++<[++++++++++>>,
 ```
- This is what we call the **brainfuck** programming language that uses particular signs and symbols to apply operations on memory cells using pointers. Here are its commands:
 ```
>: increment pointer
<: decrement pointer
+: increment value at pointer
-: decrement value at pointer
[: begin loop (continues while value at pointer is non-zero)
[:end loop
,: read one character from input into value at pointer
.: print value at pointer to output as a character
#: display debugging info (in debug mode)
Any other characters are ignored.
 ```
- After looking through the code and putting it in a brainfuck interpreter online (https://gramthanos.github.io/BrainFLAG/), it crashes. This is because every loop in the code is somewhat inverted (starts with ] and ends with [ ). Here the **description** comes in handy:
**don't panic ! look for the "input" and start from there ! good luck**
In the code we see a comma, which is responsible for the input in brainfuck language, at the end which is weird because you ask for input at the start. So we should start from there, meaning we should invert the code. A simple ch[::-1] gives us:
```
 ,>>++++++++++[<++++++>-]<[-<->]<----------[><],>>++++++++[<++++>-]<[-<->]<--------------------------------------------[><],>>+++++++++++++++++[<++++>-]<[-<->]<+++++++++++++++++++[><],>>+++++++++[<++++++>-]<[-<->]<------------------------------[><],>>++++++[<++++++>-]<[-<->]<------------------------------------------------------[><],>>+++++++++++++++++++++++[<+++>-]<[-<->]<------------------------------------------------------[><],>>+++++++++++++++++++++++++++++[<++>-]<[-<->]<---------------[><],>>++++++[<++++++>-]<[-<->]<-----------------------------------------------------------[><],>>++++++[<++++++>-]<[-<->]<----------------------------------------[><],>>+++++++++++++[<++++>-]<[-<->]<-----------------------------------------------------------------[><],>>+++++++++++++++++++++++++++++[<++>-]<[-<->]<------------------------------------------------------------[><],>>+++++++[<++++++>-]<[-<->]<-----------------------------------------------------[><],>>++++++++++[<++++++>-]<[-<->]<------------------------[><],>>+++++++++++++++++[<++>-]<[-<->]<----------------------------------------------------------------------[><],>>+++++++++++++++++[<++++>-]<[-<->]<+++++++++++++++++++[><],>>+++++++++++++++++[<++>-]<[-<->]<---------------------------------------------------------------------------------[><],>>+++++++++++++++++[<++++>-]<[-<->]<---------------------------[><],>>+++++++++++++++++[<+++>-]<[-<->]<---------------------------------------------------------[><],>>+++++++++++++++++++++++[<++>-]<[-<->]<------[><],>>++++++++[<++++++>-]<[-<->]<--------------------------------------------------------------[><],>>+++++++++++++[<+++>-]<[-<->]<----------------------------------------------------------------[><],>>++++++++[<++++++>-]<[-<->]<---------------------------------------------------------------------[><],>>++++++[<+++++>-]<[-<->]<----------------------[><],>>+++++++++++++++++++[<++>-]<[-<->]<-----------------------------------------------------------------[><],>>+++++++++++++++++++++++[<++>-]<[-<->]<-----[><],>>++++++++[<++++>-]<[-<->]<---------------------------------------------------------------------------------------------[><]
```
-  If we now look over the code again, we see a pattern that repeats itself: a comma following by a lot of instructions This suggests that this code requires us to make various inputs However most of the ones I tries in the interpreter (input in hex) resulted in an infinite loop behavior since the loops only stops when the memory cell is 0. From this I concluded that to prevent the infinite loop we need to **input something specific, like the flag letters!**
And infact when I tried the letter F before the first comma it worked and no infinite loop has occurred. Now we need to reverse the instructions to get our flag.
Manual or using code are both viable options
### manual : 
- Do not fear the manual solving for this one because it is very eazy and follows a certain mathematical equations that simplifies things:
- The instructions after each comma does the same operation on the input. And each command operates initially on memory cells initiated at 0. It is like having an array of 0s that are subject to these instructions. Let’s call this array tab.
- - To facilitate things we will use an example. The letter F which is the start of the flag and it will be the first input so only this part of the brainfuck will be needed:
 ```
,>>++++++++++[<++++++>-]<[-<->]<----------[><]
```
-``` ,>>++++++++++ ```:
- The ascii code of F is 70. It's stored at tab[0] thanks to the comma. so **tab[0]==70**. Then >> shifts the pointer to tab[2] and a certain number of + increments tab[2]. Let’s call the number of +s **x**. In this case **x==10**.

-``` [<++++++>-]```:

- Then we enter a loop that operates over tab[2]. It moves the pointer back to tab[1] , increments it with a certain number of + let’s call it **y** (**y==10**) , goes back to tab[2] and decrements it by only one '-'. This process is repeated until **tab[2]==0**. So basically this loops increments tab[1] by a value equals to **x*y** (**x*y==60**) and so tab[2] equals **zero** and **tab[1]==x*y** (60).

- ```<[-<->]```:

- < moves the pointer to tab[1] and it enters a loop that decrements tab[1] by 1, moves to tab[0], decrements it by 1 and moves back to tab[1]. This process is repeated until tab[1] reaches 0. Overally this loop just **subtracts tab[1] from the input** (tab[0]==70-60==10).
-```<----------[><]```:
- The pointer moves to tab[0] (10) and decrements it with a certain number of - that we will call it **z**. In our case **z==10**. Then it enters a loop that moves back and forth between tab[0] and tab[1] and won’t stop until **tab[0] equals 0**.
  In our case tab[0]==10 and z==10 so tab[0]=tab[0]-z==0! And so the pointer won't go into an infinite loop and will move on to the next input.
  ### Conclusion: We need an input so that after the last decrementation by z tab[0]==0. 
- following all of this logic this follows a simple mathematical equation:
```
Input - x*y -z=0
Input= x*y+z
```
- Now you apply this to the rest and you will get your flag!
### code:
- Here is the python script written by the author blackkader that I adjusted based on my own approach to the challenge:
```
f="]<>[---------------------------------------------------------------------------------------------<]>-<-[<]->++++<[++++++++>>,]<>[-----<]>-<-[<]->++<[+++++++++++++++++++++++>>,]<>[-----------------------------------------------------------------<]>-<-[<]->++<[+++++++++++++++++++>>,]<>[----------------------<]>-<-[<]->+++++<[++++++>>,]<>[---------------------------------------------------------------------<]>-<-[<]->++++++<[++++++++>>,]<>[----------------------------------------------------------------<]>-<-[<]->+++<[+++++++++++++>>,]<>[--------------------------------------------------------------<]>-<-[<]->++++++<[++++++++>>,]<>[------<]>-<-[<]->++<[+++++++++++++++++++++++>>,]<>[---------------------------------------------------------<]>-<-[<]->+++<[+++++++++++++++++>>,]<>[---------------------------<]>-<-[<]->++++<[+++++++++++++++++>>,]<>[---------------------------------------------------------------------------------<]>-<-[<]->++<[+++++++++++++++++>>,]<>[+++++++++++++++++++<]>-<-[<]->++++<[+++++++++++++++++>>,]<>[----------------------------------------------------------------------<]>-<-[<]->++<[+++++++++++++++++>>,]<>[------------------------<]>-<-[<]->++++++<[++++++++++>>,]<>[-----------------------------------------------------<]>-<-[<]->++++++<[+++++++>>,]<>[------------------------------------------------------------<]>-<-[<]->++<[+++++++++++++++++++++++++++++>>,]<>[-----------------------------------------------------------------<]>-<-[<]->++++<[+++++++++++++>>,]<>[----------------------------------------<]>-<-[<]->++++++<[++++++>>,]<>[-----------------------------------------------------------<]>-<-[<]->++++++<[++++++>>,]<>[---------------<]>-<-[<]->++<[+++++++++++++++++++++++++++++>>,]<>[------------------------------------------------------<]>-<-[<]->+++<[+++++++++++++++++++++++>>,]<>[------------------------------------------------------<]>-<-[<]->++++++<[++++++>>,]<>[------------------------------<]>-<-[<]->++++++<[+++++++++>>,]<>[+++++++++++++++++++<]>-<-[<]->++++<[+++++++++++++++++>>,]<>[--------------------------------------------<]>-<-[<]->++++<[++++++++>>,]<>[----------<]>-<-[<]->++++++<[++++++++++>>,"
f=f[::-1]
f=f[1:]+","
def p(ch):
    return ch.find("[")-2
def n(ch):#[p(ch)+4:]
    return ch.find(">")
def m(ch):
    x=ch.rfind("[")-ch.rfind("<")-1
    if ch[ch.rfind("<")+1]=="+":
        x=-x
    return x
l=f.split(",")
l.pop()
l=[e[:-3] for e in l]
l=[chr(m(e)+p(e)*n(e[p(e)+4:])) for e in l]
print("".join(l)) 
```
