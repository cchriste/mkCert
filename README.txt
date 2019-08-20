Friday, November 16, 2018
Inspired by https://alexanderzeitler.com/articles/Fixing-Chrome-missing_subjectAltName-selfsigned-cert-openssl/
-------------------------------------------------------------------------------

In order to create your cert, first run createRootCA.sh which we created first. 

> enter passphrase (three times)
> enter cert info (the only necessary part is 'Common name' which must be your full url, which could simply be 'localhost'). 

Next, run createselfsignedcertificate.sh to create the self signed cert using the previously entered 'Common name' as the SAN and CN.
Note you must have sudo access for this (2019.02.27 - actually, quite the opposite now, so I dumped sudo... https://github.com/openssl/openssl/issues/3363)



---

Now we've created ~/ssl/rootCA.[key|pem|srl] and will need to add the .pem to our list of trusted CAs (e.g., in Chrome).
In Chrome:
 - open Settings -> Advanced -> Manage certificates
 - Authorities -> Import
 - select rootCA.pem file

In the visus/anaconda, just cut and paste it into /usr/local/anaconda2/ssl/cacert.pem.

Finally, utilize the server.key and server.crt in '.' by specifying them in the <VirtualHost... section of your website.conf.

---

After adding the rootCA.pem to the list of your trusted root CAs, you can use the server.key and server.crt in your web server and browse https://localhost using Chrome 58 or later:
<chrome screenshot>

You can also verify your certificate to contain the SAN by calling verifyCert.sh.

This should look like this:

Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number: 17237690484651272016 (0xef38942aa5c52750)
    Signature Algorithm: sha256WithRSAEncryption
        Issuer: C=US, ST=New York, L=Rochester, O=End Point, CN=localhost/your-administrative-address@your-awesome-existing-domain.com
        Validity
            Not Before: Apr 23 16:07:38 2017 GMT
            Not After : Sep  5 16:07:38 2018 GMT
        Subject: C=US, ST=New York, L=Rochester, O=End Point, OU=Testing Domain/emailAddress=your-administrative-address@your-awesome-existing-domain.com, CN=localhost
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:b2:e3:bd:ed:28:04:85:ea:75:ee:d2:82:e1:eb:
                    f5:5f:7f:cf:7e:cb:70:de:86:9f:75:7c:f3:71:e7:
                    da:16:fb:bc:1f:89:bc:47:08:77:ca:33:20:f1:c1:
                    9e:e3:20:8d:89:14:7e:c1:0a:12:d2:59:24:56:9b:
                    77:90:5f:69:d1:a5:f1:00:38:93:1b:a7:75:f1:33:
                    e2:da:dc:32:a9:0a:85:7d:9a:20:81:ca:20:ee:86:
                    ce:e2:a0:52:d2:ab:11:34:e5:52:99:3a:81:c6:9f:
                    6b:0f:6a:02:2b:38:a6:84:c9:ba:fa:9b:ef:0a:89:
                    22:4b:79:86:3c:bd:44:a5:54:fb:cf:4d:8b:d1:44:
                    03:35:22:de:69:77:c8:fa:4d:c6:01:25:08:9f:4d:
                    a9:79:7a:aa:ca:03:b6:e4:51:57:22:27:5f:a7:12:
                    11:f3:e6:00:29:f6:58:be:2c:aa:09:e4:06:45:d9:
                    3f:75:a7:f0:75:bd:2b:a6:bb:6d:ad:93:bb:b9:1d:
                    d7:75:39:4e:9b:1d:0e:39:cc:17:74:88:f7:e2:b7:
                    85:12:96:e0:cb:42:56:d0:11:e0:84:86:e5:14:a5:
                    f2:6d:43:5d:f9:59:ae:61:7f:01:ae:95:b8:92:27:
                    1d:1c:02:d7:ad:fb:ee:f6:25:38:60:c8:41:20:17:
                    80:69
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Authority Key Identifier: 
                keyid:5A:8D:89:64:BD:F2:3E:C2:D7:7B:BE:17:84:F4:29:E8:C5:32:35:34

            X509v3 Basic Constraints: 
                CA:FALSE
            X509v3 Key Usage: 
                Digital Signature, Non Repudiation, Key Encipherment, Data Encipherment
            X509v3 Subject Alternative Name: 
                DNS:localhost
    Signature Algorithm: sha256WithRSAEncryption
         27:1d:d6:84:50:33:d2:ff:b1:06:9b:fa:f1:40:7d:47:11:bc:
         f7:80:fd:26:87:0e:91:9f:14:be:1f:1d:9b:32:d1:fb:d6:8d:
         af:30:8a:88:38:8c:1c:bf:77:98:8e:cd:06:48:82:fa:09:b9:
         3c:0d:38:c4:a0:da:b7:4d:f5:81:5f:5a:76:04:61:f8:c2:1a:
         17:ad:56:7c:72:ba:f6:65:7f:7f:e7:5e:b2:34:ba:13:23:57:
         84:f1:c5:ca:dd:5b:55:69:95:71:44:4a:30:53:61:5c:ad:47:
         d8:9c:d5:a2:1b:18:2d:e1:19:35:3e:3f:b2:7e:fd:bf:f3:d0:
         45:dc:f5:57:f0:1b:cd:70:1b:e0:34:de:27:98:89:b4:a5:25:
         a5:6c:29:c3:89:a6:a5:c5:4d:f5:45:3b:47:8e:13:45:23:07:
         5e:d6:59:0d:96:c6:a3:f0:c5:3d:ee:a8:ad:36:96:43:13:a1:
         b8:55:f6:c7:10:7e:8f:5d:09:ef:61:17:2a:9c:3b:50:28:c8:
         e3:8d:a6:34:06:50:d4:3e:d5:17:ea:7d:31:97:d3:ee:df:b5:
         23:66:5e:22:b7:e4:fa:36:4f:9a:d5:f0:a3:f9:b4:2b:27:02:
         0b:41:94:d1:a1:f7:1b:2c:7e:74:e6:14:c3:b5:67:15:d2:ca:
         02:77:57:a6

Watch for this line Version: 3 (0x2) as well as X509v3 Subject Alternative Name: (and below).
