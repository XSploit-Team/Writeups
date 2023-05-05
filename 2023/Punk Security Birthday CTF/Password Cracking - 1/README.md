# Password Cracking - 1

Writeup by: [j4asper](https://github.com/j4asper)

---

## Challenge Description

The flag is the password for this vault.

[supersecrets.yml](./files/supersecrets.yml)

## Challenge Solution

John the ripper has a tool for making john compatible hashes from ansible vault files, but it isn't a standard command, you will need to run the python file directly. Use the command below depending on where your john source files are placed:

```console
python /usr/share/john/ansible2john.py supersecrets.yml > ansiblehash.txt
```

If you somehow can't find the john files, you can download the `ansible2john.py` file from [here](https://github.com/openwall/john/blob/bleeding-jumbo/run/ansible2john.py)

This will make a john compatible password hash from the ansible vault file and output it into `ansiblehash.txt`.

Now we can run john to bruteforce the password:

```console
john --wordlist=new_rockyou.txt ansiblehash.txt
```

The `new_rockyou.txt` wordlist is from the Password Cracking 2 writeup, check it out.

When john is done, you should have gotten the password `zebracrossing`.
