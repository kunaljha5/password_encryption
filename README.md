 # _Key / Password encryption_

---
_As we know, we have multiple password in for many different account and environments. 
This is about how can we secure those password and store them in encrypted format in a git repo itself.
While doing online research about **how can we store passwords in our git repository and not expose to others in raw format**,  i have gone throught below links ._

- _[How to Generate/Encrypt/Decrypt Random Passwords in Linux](https://www.tecmint.com/generate-encrypt-decrypt-random-passwords-in-linux/)_
- _[Encrypt files using AES with OPENSSL](https://medium.com/@kekayan/encrypt-files-using-aes-with-openssl-dabb86d5b748)_
- _[How To Generate Random Numbers and Password with OpenSSL Rand](https://www.poftut.com/generate-random-numbers-password-openssl-rand/)_
- _[Password-based OpenSSL Encryption](https://courses.csail.mit.edu/6.857/2018/project/Ainane-Barrett-Johnson-Vivar-OpenSSL.pdf)_

---

## OpenSSL Encryption
_To Understand What is OpenSSL Encryption is,  Please vist to this [link](https://courses.csail.mit.edu/6.857/2018/project/Ainane-Barrett-Johnson-Vivar-OpenSSL.pdf). Author has explained about it in most understandable way possible._ 

## Example 1

Lets say that we have three environments 
  - dev 
  - uat 
  - prod 

To use a web-service in each environment, we want to use a **API-KEY** while doing the execution. We are suppose to use different passwords for each environment. 
We don't want to keep same **API-KEY** for each environment.

What options do we have . 

- Store the Credentials in some S3 Bucket as file and encrypt it and restrct its access by iam roles. 
- use AWS Secrets
- use AWS parameters
- use ENV variable in each environment
- Store the credentials in [Jenkins Credentials plugin](https://plugins.jenkins.io/credentials/)
- Store the credentials in [Vault](https://www.vaultproject.io/) like application.
- Store the credentials in encrypted files inside the git repository. 


I may have listed few options here but there can be more options available, i appologies in advance if it does not covers all the approach. 
I know each cloud provider has its secrets management tool/service since i mostly work AWS that is why we have AWS related options.


I will be concentrating the last option here, **"Store the credentials in encrypted files inside the git repository"**

---
### Store the credentials in encrypted files inside the git repository

Lets say that our web-service **API-KEY** String are below for each environments: 
- DEV KEY `ILoveMyDreamsDev`
- UAT KEY `ILoveMyDreamsUat`
- PROD KEY `ILoveMyDreamsProd`

We want to store these **API-KEY** for each environemnt in repository. Lets See how can we achive that.

**How to Encrypt the password and store in file ?**

```
# To Genrate Dev Environment encrypted passowrd file. 
echo "ILoveMyDreamsDev" |  openssl enc -aes-256-cbc -pass pass:${SomethingReallySecret}  -out ./secrets/app1_dev.key

# To Genrate Uat Environment encrypted passowrd file. 
echo "ILoveMyDreamsUat" |  openssl enc -aes-256-cbc -pass pass:${SomethingReallySecret}  -out ./secrets/app1_uat.key

# To Genrate Prod Environment encrypted passowrd file. 
echo "ILoveMyDreamsProd" |  openssl enc -aes-256-cbc -pass pass:${SomethingReallySecret}   -out ./secrets/app1_prod.key

```

---


**How to Decrypt the above encrypted file to retrive the password ?**

```
# To retrive the Dev Environment passowrd from encrypted file. 
WEBAPIKEY=$(openssl enc -aes-256-cbc -pass pass:${SomethingReallySecret}  -d -A -in ./secrets/app1_dev.key)
echo $WEBAPIKEY

# To retrive the Uat Environment passowrd from encrypted file. 
WEBAPIKEY=$(openssl enc -aes-256-cbc -pass pass:${SomethingReallySecret}  -d -A -in ./secrets/app1_uat.key)
echo $WEBAPIKEY

# To retrive the Prod Environment passowrd from encrypted file. 
WEBAPIKEY=$(openssl enc -aes-256-cbc -pass pass:${SomethingReallySecret}  -d -A -in ./secrets/app1_prod.key)
echo $WEBAPIKEY

```

In above example we can manage the secrets via version control and need not to manage multiple passwords in jenkins credentials plugins but can use single password ${SomethingReallySecret} in there to get all the encrypted details. 


Command related details you can get from  _[Encrypt files using AES with OPENSSL](https://medium.com/@kekayan/encrypt-files-using-aes-with-openssl-dabb86d5b748)_.
