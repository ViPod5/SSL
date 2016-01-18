SSL
====================


### The intermediate certificate

#### Override openssl.cnf default configuration
Once a CA has been setup, `openssl.cnf` must be edited before the intermediate CA certificate .csr (certificate signing request) 
is made. This can be found at `\openssl\share\openssl.cnf`. Edit the following:

```
[ usr_cert ]

# These extensions are added when 'ca' signs a request.

# This goes against PKIX guidelines but some CAs do it and some software
# requires this to avoid interpreting an end user certificate as a CA.

# basicConstraints=CA:FALSE
basicConstraints=CA:TRUE
```
and
```
[ v3_req ]

# Extensions to add to a certificate request

# basicConstraints = CA:FALSE
basicConstraints = CA:TRUE
keyUsage = nonRepudiation, digitalSignature, keyEncipherment
```
This will mark the intermediate able to act as a CA itself.

1.```
cd openssl\share\private
```
2. Generate the new intermediate private key```
..\..\bin\openssl.exe genrsa -des3 -out private.key 2048
```
3. Generate the .csr```
..\..\bin\openssl req -new -key private.key -out intermediate.csr -config ../openssl.cnf
```

Common name (eg. YOUR name) enter: `gw.izimobil.si`

4. Sign the intermediate with the CA
```
..\..\bin\openssl ca -policy policy_anything -config ..\openssl.cnf -keyfile ./CA/ca.key -cert ./CA/ca.crt -infiles intermediate.csr
```

The certificate can be foind in the `\openssl\share\certs\CA\newcerts` named something like `09.pem`.
Copy it to `\openssl\share\certs\CA\certs` and rename it to `intermediate.crt`

