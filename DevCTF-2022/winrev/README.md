# WinRev

This category was kinda new to me. I looked for tools, found windbg and some other, but is there any binary that IDA can't reverse? Let's test xD

## Payload

In this challenge, we deal with some (malicious?) payload. The challenge need the resource name where the *payload* is located. So, I thought that `Resource` API function might have to do something with it. I browsed through the .dll using IDA Pro and found some interesting stuff.

![payload](payload.png)

```
hResInfo = FindResourceW(::hModule, (LPCWSTR)0x66, L"asdasdasdasdsad");
```

Gotcha! The flag is `asdasdasdasdsad`


## Mutex

We need the name of the mutex used by malware. On decompiling the function `sub_10002300`, we can find that.

```
hObject = CreateMutexA(0, 0, "avcjkvcnmvcnmcvnmjdfkjfd");
```

which gives the mutex name as `avcjkvcnmvcnmcvnmjdfkjfd` --> turned out to be the flag


## WhichAddress

Here, we need to get the WinAPI function, that is sent to `CreateRemoteThreadAPI` as a 4th param at `0x10002174`

Peeking at this particular location using IDA Pro, we find the following code

```
hHandle = CreateRemoteThread(hProcess, 0, 0, lpStartAddress, lpAddress, 0, ThreadId);
where,
lpStartAddress = (LPTHREAD_START_ROUTINE)GetProcAddress(hModule, "LoadLibraryA");
```

`LoadLibraryA` is our required flag.


## Privileged

Here, we have to find the privileges gained by the malware. Again, we need to explore through the code using IDA Pro. I found a function `sub_100019a0` which contains

```
if ( OpenProcessToken(CurrentProcess, 0x28u, &TokenHandle) )
  {
    LookupPrivilegeValueA(0, "SeDebugPrivilege", &NewState.Privileges[0].Luid);
    NewState.PrivilegeCount = 1;
    NewState.Privileges[0].Attributes = 2;
    AdjustTokenPrivileges(TokenHandle, 0, &NewState, 0, 0, 0);
  }
```

So, it is clear that the token privilege gained by the malware is `SeDebugPrivilege`

## DropThis

Now, we need to find the file name of the dropped payload. On decompiling the function
`sub_10002250`, we find that 
```
sub_10002B3B(a3, 260, "iexplore-1.dat"); 
```                                     
So, the filename is `iexplore-1.dat`


## LevelUp

Here, we need to find the process in which tries to inject the dropped payload. So, this is easy as well. Since the injected payload was iexplore-1.dat, I googled it to find the process name and luckily it came out to be `iexplore.exe` which is the flag.


## SHA1

I loaded the dll in the site https://manalyzer.org/ which gives various details including the SHA1 hash of dropped payload. Navigate to the resources section and get the hash
