# _Keystore terminology_

 * *Certificate*:
   
        An electronic document used to prove the ownership of a public key. 
    * *CA-Signed Certificate*: 
   
            A certificate authority (CA) electronically signs a certificate to affirm that a public key belongs to the owner
            named in the certificate. Someone receiving a signed certificate can verify that the signature does belong to 
            the CA, and determine whether anyone tampered with the certificate after the CA signed it. 
   
    * *Certificate Chain*: 
   
            One signed certificate affirms that the attached public key belongs to its owner. A second signed certificate 
            affirms the trustworthiness of the first signer, a third affirms the second, and so on. The top of the chain is 
            a self-signed but widely trusted root certificate.
   
    * *Root Certificate*: 
   
            A certificate trusted to end a certificate chain. Operating systems and web browsers typically have a 
            built-in set of trusted root certificates. When your server sends a chain of certificates and one of them 
            matches one of a browser's trusted root certificates, then the browser trusts your server. When the browser 
            encrypts data with your public key, the browser is assured that only your server can read it. 
 
    * *Self-Signed Certificate*: 
   
            A file that contains a public key and identifies who owns that key and its corresponding private key. 
 
 * *Key*: 
        
        A unique string of characters that provides essential input to a mathematical process for encrypting data.
    
    * *Key Pair*: 
        
            A public encryption key and a private encryption key, in a matched set.
    * *Keystore*: 
        
            A file that holds a combination of keys and certificates. File formats:
    
        * *Java Keystore*: 
        
                A binary file format for use by Java applications. Typical file names are .keystore
                and *.jks
    
        * *PKCS*: 
        
                A binary file format typically associated with Windows systems. Typical file names are *.pkcs, *.p12, *.p7b,
                *.pfx
    
        * *PEM*: 
            
                An ASCII text file that holds keys, certificates, or both. PEM files are common on Linux systems and Apache. 
                Typical file extensions are *.pem, *.key, *.csr, and *.cert. To identify a PEM file, open it with a console 
                or text editor. If you see ASCII text, it's a PEM file.
      
    * *Public Key*: 
            
            Allows a sender (client or server) to encrypt a message for a specific recipient (server or client). When 
            your server sends a browser its public key, the browser can encrypt messages that only your server can read,
            because only your server has the matching private key.

