SSL
====================


### The intermediate certificate

1. Go here

   ```
   cd D:\openssl\share\private\CA\clients\intermediate
   ```
2. Generate the new intermediate private key. !!!DON'T DO THIS FOR AN EXISTING INTERMEDIATE!!!

   ```
   D:\openssl\bin\openssl.exe genrsa -des3 -out intermediate.key 2048
   ```
3. Generate the .csr !!!DON'T DO THIS FOR AN EXISTING INTERMEDIATE!!!

   ```
   D:\openssl\bin\openssl req -new -key intermediate.key -out intermediate.csr -config d:/openssl/share/openssl.cnf
   ```

   Common name (eg. YOUR name) enter: `gw.izimobil.si`

4. Sign the intermediate with the CA

   ```
   ..\..\bin\openssl ca -policy policy_anything -config d:/openssl/share/openssl.cnf -keyfile ./CA/ca.key -cert ./CA/ca.crt -infiles intermediate.csr
   ```

The certificate can be found in the `\openssl\share\certs\CA\newcerts` named something like `09.pem`.
Copy it to `\openssl\share\certs\CA\certs` and rename it to `intermediate.crt`



### Client certificates

1. Go here
   ```
   cd openssl\share\private
   ```
