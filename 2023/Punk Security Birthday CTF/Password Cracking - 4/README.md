# Password Cracking - 4

Writeup by: [j4asper](https://github.com/j4asper)

---

## Challenge Description

`$6$XM4TZ5vb6W/0SjIl$PsddDrA8bOKbVXApHrz9NKaF9BH92Fs1aKn6MFHelf1he8z7rbR9Af12FqynqlU2lHILU/FgNaDVUFCK2yc4B0`

The flag is the password corresponding with this hash.

## Challenge Solution

For this challenge i first needed to find out what type of hash that was, so i used [hashes.com/en/tools/hash_identifier](https://hashes.com/en/tools/hash_identifier) to identify the hash type. It came back as `sha512crypt $6$` so I googled that. I found [this page](https://asecuritysite.com/hash/splunk_hash) that explained it quite well. We can split the hash into 3 parts, the hash method, salt and hash. I made a python script to crack the password for this one using pythons passlib.hash library. I have split the hash into the salt and hash, because the hash type is not really needed when we know that we need to use sha512. Also, i didn't know the amount of rounds the hash went through, so just to be sure, i made it go from 1.000 to 10.000 rounds.

*Read Password Cracking 2 to see what `new_rockyou.txt` is.*

```py
import passlib.hash

salt = "XM4TZ5vb6W/0SjIl"
hashed_flag = "PsddDrA8bOKbVXApHrz9NKaF9BH92Fs1aKn6MFHelf1he8z7rbR9Af12FqynqlU2lHILU/FgNaDVUFCK2yc4B0"
full_hash = "$6$XM4TZ5vb6W/0SjIl$PsddDrA8bOKbVXApHrz9NKaF9BH92Fs1aKn6MFHelf1he8z7rbR9Af12FqynqlU2lHILU/FgNaDVUFCK2yc4B0"


with open("new_rockyou.txt", "r", errors="ignore") as f:
    passwords = [pw.strip() for pw in f.readlines()]

for n in range(1,10):
    n = n * 1000
    print("Rounds:", n)
    for password in passwords:
        h = passlib.hash.sha512_crypt.hash(password, salt=salt,rounds=n)
        if h == full_hash:
            print(password)
            exit(0)
```

When running the script, it tries to go through our wordlist and hashes with the salt. If the new hash matches the full hash, then it prints the flag and exits the script.
From the run we see that the amount of rounds is 5.000 and the password is `zebrasrule`.
