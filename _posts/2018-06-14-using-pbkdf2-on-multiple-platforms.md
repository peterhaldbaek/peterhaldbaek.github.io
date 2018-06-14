---
layout: post
title:  'Using PBKDF2 on multiple platforms'
date:   2018-06-14
---
[PBKDF2](https://en.wikipedia.org/wiki/PBKDF2) is a popular choice when it comes to password hashing. I recently looked into PBKDF2 and wanted to use it across multiple platforms and spent some time trying to figure out how to implement it and how to make sure that hashes were the same no matter which platform they were generated on. The programming languages I used was Swift, Java and JavaScript (Node).

I have created a [GitHub repo](https://github.com/peterhaldbaek/pbkdf2) where all code referenced here can be found.


# Swift

First language up was Swift. Since iOS is my main playground these days this was where I started out. PBKDF2 is more or less supported out of the box using CommonCrypto. We need to add a bridging header to the project and import CommonCrypto in the header.

```swift
#import <CommonCrypto/CommonCrypto.h>
```

Now CommonCrypto is available in the project and we can reference it as any other framework. The code for creating the PBKDF2 hash looks like this

```swift
func pbkdf2(hash: CCPBKDFAlgorithm, password: String, salt: String, keyByteCount: Int, rounds: Int) -> String? {
    let passwordData = password.data(using: .utf8)!
    let saltData = salt.data(using: .utf8)!
    var derivedKeyData = Data(repeating: 0, count: keyByteCount)
    
    var localDerivedKeyData = derivedKeyData
    
    let derivationStatus = derivedKeyData.withUnsafeMutableBytes { derivedKeyBytes in
        saltData.withUnsafeBytes { saltBytes in
            
            CCKeyDerivationPBKDF(
                CCPBKDFAlgorithm(kCCPBKDF2),
                password, passwordData.count,
                saltBytes, saltData.count,
                hash,
                UInt32(rounds),
                derivedKeyBytes, localDerivedKeyData.count)
        }
    }
    if (derivationStatus != kCCSuccess) {
        print("Error: \(derivationStatus)")
        return nil;
    }
    
    return toHex(derivedKeyData)
}

// Converts data to a hexadecimal string
private func toHex(_ data: Data) -> String {
    return data.map { String(format: "%02x", $0) }.joined()
}
```

I also added some helper functions for easily creating hashes based on which SHA algorithm I wanted to use.

```swift
func pbkdf2sha1(password: String, salt: String, keyByteCount: Int, rounds: Int) -> String? {
    return pbkdf2(hash: CCPBKDFAlgorithm(kCCPRFHmacAlgSHA1), password: password, salt: salt, keyByteCount: keyByteCount, rounds: rounds)
}

func pbkdf2sha256(password: String, salt: String, keyByteCount: Int, rounds: Int) -> String? {
    return pbkdf2(hash: CCPBKDFAlgorithm(kCCPRFHmacAlgSHA256), password: password, salt: salt, keyByteCount: keyByteCount, rounds: rounds)
}

func pbkdf2sha512(password: String, salt: String, keyByteCount: Int, rounds: Int) -> String? {
    return pbkdf2(hash: CCPBKDFAlgorithm(kCCPRFHmacAlgSHA512), password: password, salt: salt, keyByteCount: keyByteCount, rounds: rounds)
}
```

Theoretically using SHA256 or SHA512 should be preferred but in reality it should not make a big difference since the security of PBKDF2 also lies in stretching of the key and not only the SHA algorithm used.

The result of testing the function looks like this
```swift
pbkdf2sha1(password: "password", salt: "salt", keyByteCount: 20, rounds: 5000)
// => edf738254821c55da61e6afa20efd0c657cb941c
```


# Node

Now lets see if we can produce similar results using Node.

Hashing using PBKDF2 is part of Node so no need to use other modules. I have seen modules on NPM offering PBKDF2 hashing but unless there are very specific reasons for using these modules just use the methods provided by Node.

PBKDF2 hashing resides in the crypto module so start by requiring (or importing) it.

```javascript
const crypto = require('crypto');
```

After that it is just a matter of calling the pbkdf2 function (or its synchronous cousin pbkdf2Sync).

```javascript
crypto.pbkdf2Sync("password", "salt", 5000, 20, 'sha1').toString('hex');
// => edf738254821c55da61e6afa20efd0c657cb941c
```

As you can see the hash matches the one produced by Swift so it looks like we are heading in the right direction!

# Java

Finally we need to be able to produce the same hash using Java.

Java also supports PBKDF2 out of the box using the javax.crypto package. The examples used here were meant to run on the server so I honestly do not know if it will run on an Android device since the Dalvik VM is a scaled down version of what runs in a standard JVM. It might work, it might not.

Start out by importing the SecretKeyFactory and the PBEKeySpec.

```java
import javax.crypto.SecretKeyFactory;
import javax.crypto.spec.PBEKeySpec;
```

After that it is just a matter of using the snippet below.

```java
public static String pbkdf2(String password, String salt, int iterations, int keyLength) throws NoSuchAlgorithmException, InvalidKeySpecException {
    char[] chars = password.toCharArray();

    PBEKeySpec spec = new PBEKeySpec(chars, salt.getBytes(), iterations, keyLength);
    SecretKeyFactory skf = SecretKeyFactory.getInstance("PBKDF2WithHmacSHA1");
    byte[] hash = skf.generateSecret(spec).getEncoded();
    return toHex(hash);
}

// Converts byte array to a hexadecimal string
private static String toHex(byte[] array) {
    StringBuffer sb = new StringBuffer();
    for (int i = 0; i < array.length; i++) {
        sb.append(Integer.toString((array[i] & 0xff) + 0x100, 16).substring(1));
    }
    return sb.toString();
}
```

Testing it reveals this surprising result

```java
pbkdf2("password", "salt", 5000, 20);
// => edf7
```

Hmmm. Not quite what we expected. After some fiddling with the method it turns out we have to multiply the keyLength with 8 to provide the same results as with Swift and Node.

```java
public static String pbkdf2(String password, String salt, int iterations, int keyLength) throws NoSuchAlgorithmException, InvalidKeySpecException {
    char[] chars = password.toCharArray();

    PBEKeySpec spec = new PBEKeySpec(chars, salt.getBytes(), iterations, keyLength*8);
    SecretKeyFactory skf = SecretKeyFactory.getInstance("PBKDF2WithHmacSHA1");
    byte[] hash = skf.generateSecret(spec).getEncoded();
    return toHex(hash);
}
```

This produces the following result which aligns nicely with the results from Swift and Node.

```java
pbkdf2("password", "salt", 5000, 20);
// => edf738254821c55da61e6afa20efd0c657cb941c
```

That's it. Again, everything is available at [GitHub](https://github.com/peterhaldbaek/pbkdf2), enjoy!