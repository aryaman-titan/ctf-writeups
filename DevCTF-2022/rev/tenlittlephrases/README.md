# Ten little phrases

On initial static analysis, it seems to resemble the old one excess chars. The process to exploit this was pretty similar to that of ExcessChars one.

But again going for the same old way?? Nah...let's try this awesome tool that I've recently heard of called radare2

```
r2 -w crackme3  -->  opens the binary in write mode
aaa             -->  auto analysis of the binary
s main          -->  seek to the address of the main function 
```

Now, press p to see the instructions. 


## Exploit

Patch the binary using radare2, add a `jmp 0x80491a6` instruction where `0x80491a6` is the address of `printFlag`

So yeah, now we have the flag: `CTF{L3ts_g3t_X_R3verS!ng}`
