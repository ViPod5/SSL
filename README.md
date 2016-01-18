SSL
====================


### The intermediate certificate

1. Go here

   ```
   cd \openssl\share\private\CA\clients\intermediate
   ```
2. Generate the new intermediate private key. !!!DON'T DO THIS FOR AN EXISTING INTERMEDIATE!!!

   ```
   \openssl\bin\openssl.exe genrsa -des3 -out intermediate.key 2048
   ```
3. Generate the .csr !!!DON'T DO THIS FOR AN EXISTING INTERMEDIATE!!!

   ```
   \openssl\bin\openssl req -new -key intermediate.key -out intermediate.csr -config d:/openssl/share/openssl.cnf
   ```

   Common name (eg. YOUR name) enter FQDN: `gw.companyname.com`

4. Sign the intermediate with the CA

   ```
   \openssl\bin\openssl ca -policy policy_anything -config d:/openssl/share/openssl.cnf -keyfile ..\..\ca.key -cert ..\..\ca.crt -infiles intermediate.csr
   ```

Openssl does not allow the same certificate to be issued twice. If you get bad txt db error you'll have to edit the `\openssl\share\certs\CA\index.txt` file and remove the existing certificate.

The certificate can be found in the `\openssl\share\certs\CA\newcerts` named something like `09.pem`.
Copy it to `\openssl\share\certs\CA\certs` and rename it to `intermediate.crt`



### Client certificates

1. Go here
   ```
   cd \openssl\share\private\clients\test
   ```
   
2. Make client directory
   ```
   mkdir new_client_name
   cd new_client_name
   ```
   
3. Generate new client key
   ```
   \openssl\bin\openssl.exe genrsa -des3 -out client.key 2048
   ```

4. Generate new certificate signing request
   ```
   \openssl\bin\openssl req -new -key client.key -out client.csr -config d:/openssl/share/intermediate.cnf
   ```

5. Sign the request using intermediate key and certificate
   ```
   openssl x509 -req -in client.csr -CA \openssl\share\certs\CA\certs\intermediate.crt -CAkey \openssl\share\private\CA\clients\intermediate\intermediate.clear.key -out client.crt -set_serial 0x439873645 -days 3655
   ```

6. Create PKCS12 key exchange file
   ```
   openssl pkcs12 -export -in client.crt -inkey client.key -out client.p12 -name "Client Certificate Name"
   ```

7. Export the public key
   ```
   openssl rsa -in client.key -pubout > public.key
   ```

8. Copy the .pfx and .crt to destination directory
   ```
   mkdir \openssl\share\certs\clients\new_client_name
   copy client.crt \openssl\share\certs\clients\new_client_name\
   copy client.p12 \openssl\share\certs\clients\new_client_name\
   ```
