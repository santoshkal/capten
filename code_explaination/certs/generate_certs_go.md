# Certificate Generation Functions

The provided Go package is responsible for generating SSL/TLS certificates and certificate chains for a Capten application. The package consists of several functions that generate certificates, including the root CA, intermediate CA, agent certificate, and client certificate.

1. PrepareCerts Function

The PrepareCerts function is the entry point for the certificate generation process. It checks if the certificates exist in the specified directory and if not, generates them. If the certificates already exist, it skips the generation process and returns an error.

2. checkCertsExist Function

The checkCertsExist function checks if the certificates exist in the specified directory. It returns a boolean value indicating whether the certificates exist or not.

3. generateCerts Function

The generateCerts function generates the certificates, including the root CA, intermediate CA, agent certificate, and client certificate. It calls the generateCACert, generateIntermediateCACert, generateAgentCert, and generateClientCert functions to generate the certificates.

4. generateCACert Function

The generateCACert function generates the root CA certificate and key. It uses the rsa.GenerateKey function to generate a 4096-bit RSA key and the x509.CreateCertificate function to create the root CA certificate.

5. generateIntermediateCACert Function

The generateIntermediateCACert function generates the intermediate CA certificate and key. It uses the rsa.GenerateKey function to generate a 4096-bit RSA key and the x509.CreateCertificate function to create the intermediate CA certificate.

6. generateAgentCert Function

The generateAgentCert function generates the agent certificate and key. It uses the rsa.GenerateKey function to generate a 2048-bit RSA key and the x509.CreateCertificate function to create the agent certificate.

7. generateClientCert Function

The generateClientCert function generates the client certificate and key. It uses the rsa.GenerateKey function to generate a 2048-bit RSA key and the x509.CreateCertificate function to create the client certificate.

8. generateCACertChain Function

The generateCACertChain function generates the certificate chain for the CA. It reads the root CA and intermediate CA certificates from the file system and appends them to a new file.

9. generateCaptenClientCertZip Function

The generateCaptenClientCertZip function generates a ZIP file containing the client certificate, key, and CA certificate. It uses the zip package to create a ZIP file and adds the files to it using the addFileToZip function.

10. addFileToZip Function

The addFileToZip function adds a file to a ZIP file. It uses the zip package to create a new file in the ZIP file and copies the contents of the file to it.

### In summary, the provided Go package generates SSL/TLS certificates and certificate chains for a Capten application. It consists of several functions that generate certificates, including the root CA, intermediate CA, agent certificate, and client certificate. The package also generates a ZIP file containing the client certificate, key, and CA certificate.