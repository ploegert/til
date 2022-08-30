# Generate a new CSR

## Generate A Private Key

### ECC:

```bash
keyfilename=ec_key.pem
cert=ec_cert.crt

openssl ecparam -genkey -name prime256v1 -out $keyfilename

openssl ecparam -genkey -name secp256r1 -out $keyfilename

# List of Avail curves
openssl ecparam --list_curves

# Gen Cert:
openssl req -new -x509 -key $keyfilename -sha256 -nodes -out $cert -days 365

# Combine Private & Public in one file:
cat $keyfilename $cert > ec.pem

# verify
openssl x509 -in ec.pem -noout -text
```

### RSA:

#### Generate Private Key

```bash
#=============================================
# Generate Key
openssl genrsa -out yourdomain.key 2048

# Output key File
cat yourdomain.key

# Validate Key
openssl rsa -text -in yourdomain.key -noout

#=============================================
# Extract Public Key
openssl rsa -in yourdomain.key -pubout -out yourdomain_public.key

#=============================================
# Create CSR
openssl req -new -key yourdomain.key -out yourdomain.csr

#=============================================
# verify CSR
openssl req -text -in yourdomain.csr -noout -verify

# send CSR to CA:
cat yourdomain.csr

# View Cert info:
openssl x509 -text -in yourdomain.crt -noout

# Verify Public & Private Keys Match:
openssl pkey -pubout -in .\private.key | openssl sha256
openssl req -pubkey -in .\request.csr -noout | openssl sha256
openssl x509 -pubkey -in .\certificate.crt -noout | openssl sha256
```

#### Verification should output something like this:

```bash
verify OK
Certificate Request:
    Data:
        Version: 0 (0x0)
        Subject: C=US, ST=Utah, L=Lehi, O=Your Company, Inc., OU=IT, CN=yourdomain.com
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:bb:31:71:40:81:2c:8e:fb:89:25:7c:0e:cb:76:
                    [...17 lines removed]
                Exponent: 65537 (0x10001)
        Attributes:
            a0:00
    Signature Algorithm: sha256WithRSAEncryption
         0b:9b:23:b5:1f:8d:c9:cd:59:bf:b7:e5:11:ab:f0:e8:b9:f6:
         [...14 lines removed]
```

#### Using -Subj switch

```bash
openssl req -new -key yourdomain.key -out yourdomain.csr \
-subj "/C=US/ST=Utah/L=Lehi/O=Your Company, Inc./OU=IT/CN=yourdomain.com"
```

#### Create CSR with one line

```bash
openssl req -new \
-newkey rsa:2048 -nodes -keyout yourdomain.key \
-out yourdomain.csr \
-subj "/C=US/ST=Utah/L=Lehi/O=Your Company, Inc./OU=IT/CN=yourdomain.com"
```

### Converting Certificate Formats

File Formats & Cert Types

|                                |                                                                                                                                                                                                                                                                                                                                                                       |                                                                                                                                   |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| .pem                           | PEM, which stands for privacy-enhanced mail                                                                                                                                                                                                                                                                                                                           | <p>—- BEGIN RSA PRIVATE KEY—– and —–END RSA PRIVATE KEY—–<br><br>–BEGIN CERTIFICATE REQUEST—– <br>—–END CERTIFICATE REQUEST—–</p> |
| p7b / pkcs#7                   | Certificates in P7B/PKCS#7 formats are encoded in Base64 ASCII encoding and they usually have .p7b or .p7c as the file extension. **The thing that separates PKCS#7 formatted certificates is that only certificates can be stored in this format, not private keys.** In other words, a P7B file will only consist of certificates and chain certificates.           | <p>"—–BEGIN PKCS7—–” and </p><p>“—–END PKCS7—–”</p>                                                                               |
| <p>.pfx<br>.p12<br>.pkcs12</p> | The PKCS#12 format is an archival file that stores both the certificate and the private key.                                                                                                                                                                                                                                                                          | Binary File                                                                                                                       |
| der                            | <p>The DER format uses ASN.1 encoding to store certificate or key information. Similar to the PEM format, DER stores key and certificate information in two separate files and typically uses the same file extensions (i.e., <em>.key</em>, <em>.crt</em>, and <em>.csr</em>). <br><br>The DER certificate format is most commonly used in Java-based platforms.</p> |                                                                                                                                   |
| .crt                           | Certificate                                                                                                                                                                                                                                                                                                                                                           |                                                                                                                                   |
| .csr                           | Certificate Signing Request                                                                                                                                                                                                                                                                                                                                           |                                                                                                                                   |

#### The following is an excerpt from [https://www.digicert.com/kb/ssl-support/openssl-quick-reference-guide.htm](https://www.digicert.com/kb/ssl-support/openssl-quick-reference-guide.htm):

#### PEM to PKCS#12

&#x20;This format is useful for migrating certificates and keys from one system to another as it contains all the necessary files. PKCS#12 files use either the _.pfx_ or _.p12_ file extension.

Use the following command to convert your PEM key and certificate into the PKCS#12 format (i.e., a single .pfx file):

```bash
openssl pkcs12 -export -name "yourdomain-digicert-(expiration date)" \
-out yourdomain.pfx -inkey yourdomain.key -in yourdomain.crt
```

#### PKCS#12 to PEM

Because the PKCS#12 format contains both the certificate and private key, you need to use two separate commands to convert a .pfx file back into the PEM format.

Use the following command to extract the private key from a PKCS#12 (.pfx) file and convert it into a PEM encoded private key:

```
openssl pkcs12 -in yourdomain.pfx -nocerts -out yourdomain.key -nodes
```

Use the following command to extract the certificate from a PKCS#12 (.pfx) file and convert it into a PEM encoded certificate:

```
openssl pkcs12 -in yourdomain.pfx -nokeys -clcerts -out yourdomain.crt
```

#### PEM to DER

The DER format uses ASN.1 encoding to store certificate or key information. Similar to the PEM format, DER stores key and certificate information in two separate files and typically uses the same file extensions (i.e., _.key_, _.crt_, and _.csr_). The file extension _.der_ was used in the below examples for clarity.

Use the following command to convert a PEM encoded certificate into a DER encoded certificate:

```
openssl x509 -inform PEM -in yourdomain.crt -outform DER -out yourdomain.der
```

Use the following command to convert a PEM encoded private key into a DER encoded private key:

```
openssl rsa -inform PEM -in yourdomain.key -outform DER -out yourdomain_key.der
```

#### DER to PEM

Use the following command to convert a DER encoded certificate into a PEM encoded certificate:

```
openssl x509 -inform DER -in yourdomain.der -outform PEM -out yourdomain.crt
```

Use the following command to convert a DER encoded private key into a PEM encoded private key:

```
openssl rsa -inform DER -in yourdomain_key.der -outform PEM -out yourdomain.key
```



Sources:

#### [https://www.digicert.com/kb/ssl-support/openssl-quick-reference-guide.htm](https://www.digicert.com/kb/ssl-support/openssl-quick-reference-guide.htm)

#### [https://comodosslstore.com/resources/a-ssl-certificate-file-extension-explanation-pem-pkcs7-der-and-pkcs12/](https://comodosslstore.com/resources/a-ssl-certificate-file-extension-explanation-pem-pkcs7-der-and-pkcs12/)

#### [https://www.adamintech.com/openssl-how-to-generate-a-self-signed-certificate-and-key-with-elliptic-curves/](https://www.adamintech.com/openssl-how-to-generate-a-self-signed-certificate-and-key-with-elliptic-curves/)

####
