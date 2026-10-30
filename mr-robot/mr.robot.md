## Machine Overview

This machine is designed for **beginner/intermediate** users, and there are **three hidden keys** on the system. Can you find them? The machine is part of the **Mr. Robot** series, and the report was created with permission from [Leon Johnson](https://twitter.com/@sho_luv).

---

### Key 1

The first key was found by running a **nmap scan** on the target machine (IP: `10.10.134.214`). The scan revealed the following open ports:

```bash
PORT STATE SERVICE REASON VERSION
22/tcp closed ssh reset ttl 63 
80/tcp open http syn-ack ttl 63 Apache httpd 
443/tcp open ssl/http syn-ack ttl 63 Apache httpd
```
Running `dirb` on the web server at port 80 revealed a hidden file `key-1-of-3.txt` located at `http://10.10.134.214/key-1-of-3.txt`.

The contents of `key-1-of-3.txt` are:

073403c8a58a1f80d943455fb30724b9

This looks like a **hash**—possibly MD5. After decoding, you will retrieve **Key 1**.

### Key 2

Upon further exploration of the machine, we find a **WordPress login page** at `http://10.10.134.214/wp-login.php`. After logging in using the credentials extracted from the base64-encoded string found at `http://10.10.134.214/license`, which decoded to:

### elliot:ER28-0652

### Uploading Reverse Shell

After gaining admin access to the WordPress site, you attempted to upload a **reverse shell** but were blocked due to file upload restrictions:
We gain access to the WordPress admin interface. After scanning the site with **wpscan**, we discover the theme in use is **Twenty Fifteen**. Exploiting a vulnerability or file upload issue, we manage to gain a **shell** on the machine.


{Sorry, this file type is not permitted for security reasons.}


However, you managed to gather more information by running **wpscan** on the WordPress site, revealing the theme **Twenty Fifteen**. The theme was outdated and vulnerable, which led to further access.

### Shell Access via File Inclusion

You found the `404.php` file and used it to upload a **reverse shell**, ultimately gaining a **shell** on the machine.

Exploring the `/home` directory reveals a hidden `robot` folder, which contains:

- `key-2-of-3.txt`
- `password.raw-md5`

After running `cat key-2-of-3.txt`, we were initially denied permission. However, after cracking the **MD5 hash** in `password.raw-md5`:

## robot:c3fcd3d76192e4007dfb496cca67e13bda

We successfully log in as **robot** and retrieve **Key 2**:
822c73956184f694993bede3eb39f959

### Key 3

### Privilege Escalation to Root

To escalate privileges, you ran **linpeas.sh** on the target machine, which highlighted that **nmap** could be used to gain **root** access. By running the following:

## nmap> !sh

We successfully escalate to **root** and navigate to `/root`. There, we find the final key, **key-3-of-3.txt**.

```bash
$ cd /root 
$ ls firstboot_done key-3-of-3.txt 
$ cat key-3-of-3.txt 
04787ddef27c3dee1ee161b21670b4e4
```

This gives us **Key 3**:
04787ddef27c3dee1ee161b21670b4e4

### Conclusion

You have successfully retrieved all three keys from the **Mr. Robot** machine:

1. **Key 1**: `073403c8a58a1f80d943455fb30724b9`
2. **Key 2**: `822c73956184f694993bede3eb39f959`
3. **Key 3**: `04787ddef27c3dee1ee161b21670b4e4`
