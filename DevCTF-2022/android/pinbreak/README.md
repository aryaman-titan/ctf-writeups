# Pinbreak

# Requirements

- apktool
- sqliteDB browser

# Description

```
Can you find the pin?

https://devctf-binaries.s3.ap-south-1.amazonaws.com/apks/pinbreak.apk
```

The first step for reversing any android APK is to decompile it for view it using jadx-gui. I decided to go with the former one for this step. 

# Observation

On looking for some interesting files in the directory, I found a SQLite DB

```
❯ file assets/pinlock.db
assets/pinlock.db: SQLite 3.x database, last written using SQLite version 3035005
```

Opening it in any SQLiteDB browser, and then browsing the data of `pinDB`, we found a md5 hash `bae5c9d883433f2d1e926ef693831dafa1664306`.

# Exploitation

Now since we have the hash of the password, let's try to get the password using [crackstation](https://crackstation.net/), we get the PIN as 9264.

Running the app in genymotion or any android emulator, entered the PIN and got the flag!

Flag: ```ctf{Th@ts_H0w_y0U_br3@k_p!ns}```
