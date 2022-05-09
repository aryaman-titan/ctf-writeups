# Lit - part 1 and 2

## Requirements

- jadx-gui
- knowledge of firebase APIs
- pyrebase

## Description

```
What's in a name?

Really?

Link: https://devctf-binaries.s3.ap-south-1.amazonaws.com/apks/cloud.apk
```

The first step for reversing any android APK is to decompile it for view it using jadx-gui. I decided to go with the latter one for this step. Let's open the apk in jadx-gui

![jadx](jadx.png)

## Observation

This line, `e4.d c5 = a5.b("rCpOr6eRlLDWW0pfAeQV/").a("sharString").a(format).c();` seems like firebase is trying to get a document from it's database. So, maybe `rCpOr6eRlLDWW0pfAeQV` is a docID? Anyways, let's move on. 

Also, we can see that res/values/strings.xml has some interesting creds:

![creds](creds.png)

I tried to setup [pyrebase](https://github.com/thisbejim/Pyrebase) and interacted with firebase DB using it.

## Exploitation

```py
import pyrebase

config = {
  "apiKey": "AIzaSyDsUKgpqs9KAjZy_EE7dfHpG57X-f9P7wo",
  "authDomain": "mongodb-e98ea.firebaseapp.com",
  "databaseURL": "https://mongodb-e98ea-default-rtdb.firebaseio.com",
  "storageBucket": "mongodb-e98ea.appspot.com"
}

firebase = pyrebase.initialize_app(config)
```

I ran it in an interactive python shell, so that I can further try out stuff.

![Flag](flag_dhamaka.png)

I tried out `flag` to randomly check if there lies a document like it, in the misconfigured firebase. And guess what?? It was there, I contacted the admin for it, but seemed like this was an unintended exploit of this challenge, and so I got part-2 first, part-1 was trivial as we had the docID to retrieve the string.

After some time, I also realized from the error, that both the flags can be directly accessed from:

Part 1: `https://mongodb-e98ea-default-rtdb.firebaseio.com/rCpOr6eRlLDWW0pfAeQV/.json`
Part 2: `https://mongodb-e98ea-default-rtdb.firebaseio.com/flag.json`

Lit-Part1: ```ctf{d0n't_g1ve_us3rs_fireb@sedb_acc33s}```
Lit-Part2: ```ctf{fireb@s3-mi$sconfigurati0n}```

It would be interesting to see, what the intended exploit for this challenge would be?!

