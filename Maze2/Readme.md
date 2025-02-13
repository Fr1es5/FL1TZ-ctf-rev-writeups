# Maze2 challenge
## Challenge Description:
- **… the key resides here , but it is protected by the two spells : -A6H5S2 -obfuscation …**
## Required tools:
- Any C editor.
## Inportant note!!:
-  maze1 flag is required in solving this challenge:
```
FL1TZ{k3yl0ver5_UN1T3!:_t0geth3eR_w3_wILL_sOlv3_tH3_nExT_0n3!!}
```
## Steps to solve:
- In the maze2.c we have a very long c code but thankfully most of it can be ignored except 3 main functions: abc123_def456, ghi789_jkl012, print_ln and main. Let’s try to understand each one:
    ```
    int64_t abc123_def456(char* arg1, int64_t arg2)
    {
      int64_t rax = EVP_MD_CTX_new();
      EVP_DigestInit_ex(rax, EVP_sha256(), 0);
      EVP_DigestUpdate(rax, arg1, strlen(arg1));
      EVP_DigestFinal_ex(rax, arg2, 0);
      return EVP_MD_CTX_free(rax);
    }
``
  -- For me personally this was too complicated but I saw the sha256 part and I assumed it stores the sha256 of arg1 and stores in in arg2 
  But the description of the challenge confirmed my assumptions since A6H5S2 is just SHA256 but scrambled xD.

  
- Moving on to ghi789_jkl012:
  ```
  void ghi789_jkl012(void* arg1, int64_t arg2)
  {
    for (int32_t i = 0; i <= 0x1f; i += 1)
        sprintf(i * 2 + arg2, "%02x", *(arg1 + i));
  }
  ```
  - This one converts each element of arg1 into a 2 character hexadecimal string and stores it in arg2. You can tell by the “%02x”.
- Moving on to print_ln:
  - This function is quite complicated as well: it does a lot of shifting, xoring and basic arithmetic operations on arg1 and arg2 and returns either 0 or 1 according to this part:
    ```
    int64_t rax_44;
    rax_44 = (var_80 ^ var_78) == var_78;
    return rax_44;
    ```
- For now let's ignore it and move on to main:
  ```
   void buf;
    int32_t result;
    
    if (fgets(&buf, 0x64, __TMC_END__))
    {
        *(&buf + strcspn(&buf, u"\n…")) = 0;
        void var_178;
        abc123_def456("FL1TZ{y4_w3ld1_Jib_elFl4g_mel_MAZE}", &var_178);
        void var_138;
        ghi789_jkl012(&var_178, &var_138);
  ```
  - This part takes the input of the user (buf) and does sha256 on this placeholder flag and converts it into a hexadecimal format.
  ```
  int64_t s;
        __builtin_memset(&s, 0, 0x64);
        int32_t var_194_1 = 0;
        int32_t var_190_1 = 0;
        
        while (*(&var_138 + var_194_1))
        {
            if (print_ln(*(&var_138 + var_194_1), 4))
            {
                *(&s + var_190_1) = *(&var_138 + var_194_1);
                var_190_1 += 1;
            }
            
            var_194_1 += 1;
        }
        
        *(&s + var_190_1) = 0;
        
        if (strcmp(&buf, &s))
        {
            puts("dsl");
            result = 0;
        }
        else
        {
            FILE* fp = fopen("flag.txt", u"r…");
    ```
  - This part iterates over the converted sha256 of the placeholder flag and applies print_In over it with the value 4 and prints the hex character into s if print_In returns 1. It then compares s with the input of the user and returns the flag if they match. 
    Let’s call this part ‘filter’.
- As you have already guessed the placeholder flag will be replaced with the maze1 flag and will be hashed and converted into hex. 
There are two methods into solving this challenge regarding print_In:
### Method 1: If you choose to ignore what print_In does:
- You can hash the maze1 flag with 256 online, apply **filter** to it, concatenate the corresponding elements to get this result:
- ```
  d408d48448d8.
  ```
- You then put it into the server and it will give you the flag.
## Method 2: if you choose to understand print_ln (which I didn’t xD):
- When breaking down print_In with arg2 as 4, you will understand that **it’s a simple division by 4** (hhhhhhh). So basically ‘filter’ checks if the elements of the converted hex of the hashed flag are divisible by 4 according to their ascii values.
and so you get ```d408d48448d8```, put it into the server and get the flag xD
## Conclusion:
- The hints are A6H5S2 - obfuscation, with A6H5S2 being sha256 but scrambled and obfuscation is a division by 4 (bruh).









 


