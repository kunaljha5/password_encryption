

 # _Key / Password encryption_

---
_As we know, we have multiple passwords for many different accounts and environments. 
This is about how can we secure those passwords and store them in encrypted format in a git repo itself.
While doing online research about **how can we store passwords in our git repository and not expose to others in raw format**, I have gone through the below links ._

- _[How to Generate/Encrypt/Decrypt Random Passwords in Linux](https://www.tecmint.com/generate-encrypt-decrypt-random-passwords-in-linux/)_
- _[Encrypt files using AES with OPENSSL](https://medium.com/@kekayan/encrypt-files-using-aes-with-openssl-dabb86d5b748)_
- _[How To Generate Random Numbers and Password with OpenSSL Rand](https://www.poftut.com/generate-random-numbers-password-openssl-rand/)_
- _[Password-based OpenSSL Encryption](https://courses.csail.mit.edu/6.857/2018/project/Ainane-Barrett-Johnson-Vivar-OpenSSL.pdf)_

---

## OpenSSL Encryption
_To Understand What is OpenSSL Encryption is,  Please visit this [link](https://courses.csail.mit.edu/6.857/2018/project/Ainane-Barrett-Johnson-Vivar-OpenSSL.pdf). The author has explained it in the most understandable way possible._ 

## Example 1

Lets say that we have three environments 
  - dev 
  - uat 
  - prod 

To use a web-service in each environment, we want to use an **API-KEY** while doing the execution. We are supposed to use different passwords for each environment. 
We don't want to keep the same **API-KEY** for each environment.

What options do we have? 

- Store the Credentials in some S3 Bucket as a file and encrypt it and restrict its access by IAM roles. 
- use AWS Secrets
- use AWS parameters
- use the ENV variable in each environment
- Store the credentials in [Jenkins Credentials plugin](https://plugins.jenkins.io/credentials/)
- Store the credentials in [Vault](https://www.vaultproject.io/) like the application.
- Store the credentials in encrypted files inside the git repository. 


I may have listed a few options here but there can be more options available, I apologize in advance if it does not covers all the approaches. 
I know each cloud provider has its secrets management tool/service since I mostly work AWS that is why we have AWS related options.


I will be concentrating the last option here, **"Store the credentials in encrypted files inside the git repository"**

---
### Store the credentials in encrypted files inside the git repository

Lets say that our web-service **API-KEY** String are below for each environments: 
- DEV KEY `ILoveMyDreamsDev`
- UAT KEY `ILoveMyDreamsUat`
- PROD KEY `ILoveMyDreamsProd`

We want to store these **API-KEY** for each environment in a repository. Let us see how can we achieve that.

**How to Encrypt the password and store it in a file ?**

```
# To Generate Dev Environment encrypted password file. 
echo "ILoveMyDreamsDev" |  openssl enc -aes-256-cbc -pass pass:${SomethingReallySecret}  -out ./secrets/app1_dev.key

# To Generate Uat Environment encrypted password file. 
echo "ILoveMyDreamsUat" |  openssl enc -aes-256-cbc -pass pass:${SomethingReallySecret}  -out ./secrets/app1_uat.key

# To Generate Prod Environment encrypted password file. 
echo "ILoveMyDreamsProd" |  openssl enc -aes-256-cbc -pass pass:${SomethingReallySecret}   -out ./secrets/app1_prod.key

```

---


**How to Decrypt the above-encrypted file to retrieve the password ?**

```
# To retrieve the Dev Environment password from the encrypted file. 
WEBAPIKEY=$(openssl enc -aes-256-cbc -pass pass:${SomethingReallySecret}  -d -A -in ./secrets/app1_dev.key)
echo $WEBAPIKEY
ILoveMyDreamsDev

# To retrieve the Uat Environment password from an encrypted file. 
WEBAPIKEY=$(openssl enc -aes-256-cbc -pass pass:${SomethingReallySecret}  -d -A -in ./secrets/app1_uat.key)
echo $WEBAPIKEY
ILoveMyDreamsUat

# To retrieve the Prod Environment password from the encrypted file. 
WEBAPIKEY=$(openssl enc -aes-256-cbc -pass pass:${SomethingReallySecret}  -d -A -in ./secrets/app1_prod.key)
echo $WEBAPIKEY
ILoveMyDreamsProd
```

In the above example, we can manage the secrets via version control and need not manage multiple passwords in Jenkins credentials plugins but can use a single password **${SomethingReallySecret}** in there to get all the encrypted details. 


Command related details you can get from  _[Encrypt files using AES with OPENSSL](https://medium.com/@kekayan/encrypt-files-using-aes-with-openssl-dabb86d5b748)_.
