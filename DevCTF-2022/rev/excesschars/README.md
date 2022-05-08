# Excess Chars

Seems like a simple buffer overfow challenge from the name.

On executing, it asks for an input, and entering random values gives wrong password.

I used `Ghidra` for decompiling the program. Here's the useful code
```c
    if (**(char **)(param_2 + 8) == '\0') {
      uVar3 = 0xffffffff;
    }
    else {
      for (local_24 = 0;
          ("AJi9VL0C4p"[local_24] != '\0' &&
          (*(char *)((long)local_24 + *(long *)(param_2 + 8)) != '\0')); local_24 = local_24 + 1) {
        cVar1 = "AJi9VL0C4p"[local_24];
        iVar2 = QM9Nq();
        if (cVar1 - iVar2 != (int)*(char *)((long)local_24 + *(long *)(param_2 + 8))) {
          printf("No, %s is not correct.\n",*(undefined8 *)(param_2 + 8));
          return 1;
        }
      }
      printf("Yes, %s is correct!\n",*(undefined8 *)(param_2 + 8));
      uVar3 = 0;
    }
```


It looks very complex, but the code is just looping over all the characters of our input and comparing it with a constant string `AJi9VL0C4p` with some offset.  
This offset is calculated with another function which is given below.

```c
int QM9Nq(void)
{
  int local_1c;
  
  for (local_1c = 1; (local_1c < 0xb && (((local_1c % 0xb) * 4) % 0xb != 1));
      local_1c = local_1c + 1) {
  }
  return local_1c;
}
```

I saved it as offset.c and ran it with gcc and got the output simply as `3`.

# Final exploit

```c
int main(){
    char str1[] = "AJi9VL0C4p";
    for(int i=0;i<strlen(str1);i++){
        putchar(str1[i]+QM9Nq());
    }
    return 0;
}
```

So, we got the password as `>Gf6SI-@1m`. This turned out to be the flag.