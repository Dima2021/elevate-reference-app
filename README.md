# Elevate Accelerator Reference App


## Table of Contents
- [Overview](#overview)
- [Requirements](#requirements)
- [Frameworks/Libraries](#frameworks)
- [Integrating with OpenAPI Generator](#OpenAPI_Generator)
- [Configuration](#configuration)
- [Use-Cases](#use-cases)
- [Execute the Use-Cases](#execute-the-use-cases)
- [Service Documentation](#documentation)
- [API Reference](#api-reference)
- [Support](#support)
- [License](#license)

## Overview  <a name="overview"></a>
This is a reference application to demonstrate how Elevate Accelerator Proxy APIs can be used.
To call these APIs, consumer key and .p12 file are required from your project on Mastercard Developers.

## Requirements  <a name="requirements"></a>

- Java 11
- IntelliJ IDEA (or any other IDE)

## Frameworks/Libraries <a name="frameworks"></a>
- Spring Boot
- Apache Maven
- OpenAPI Generator

## Integrating with OpenAPI Generator <a name="OpenAPI_Generator"></a>

OpenAPI Generator generates API client libraries from OpenAPI Specs. It provides generators and library templates for supporting multiple languages and frameworks.
Check [Generating and Configuring a Mastercard API Client](https://developer.mastercard.com/platform/documentation/security-and-authentication/generating-and-configuring-a-mastercard-api-client/) to know more about how to generate a simple API client for consuming APIs.


### Configuring Payload Encryption
The [Mastercard Encryption Library](https://github.com/Mastercard/client-encryption-java) provides interceptor class that you can use when configuring your API client. This [interceptor](https://github.com/Mastercard/client-encryption-java#usage-of-the-okhttpfieldlevelencryptioninterceptor-openapi-generator-4xy) will encrypt payload before sending the request.

**Encryption Config**
```
FieldLevelEncryptionConfig config = FieldLevelEncryptionConfigBuilder
                    .aFieldLevelEncryptionConfig()
                    .withEncryptionCertificate(cert)
                    .withEncryptionPath("$", "$")
                    .withEncryptedValueFieldName("encryptedData")
                    .withEncryptedKeyFieldName("encryptedKey")
                    .withOaepPaddingDigestAlgorithmFieldName("oaepHashingAlgorithm")
                    .withOaepPaddingDigestAlgorithm("SHA-256")
                    .withEncryptionKeyFingerprintFieldName("publicKeyFingerprint")
                    .withIvFieldName("iv")
                    .withFieldValueEncoding(FieldLevelEncryptionConfig.FieldValueEncoding.HEX)
                    .build();
```

See also: 
- [Securing Sensitive Data Using Payload Encryption](https://developer.mastercard.com/platform/documentation/security-and-authentication/securing-sensitive-data-using-payload-encryption/).

## Configuration <a name="configuration"></a>
1. Create your account on [Mastercard Developers](https://developer.mastercard.com/) if you don't have it already.
2. Create a new project here and add ***Elevate*** to it and click continue.
3. Download Sandbox Signing Key, a ```.p12``` file will be downloaded.
4. In the Client Encryption Keys section of the dashboard, click on the ```Actions``` dropdown and download the client encryption key, a ``.pem``` file will be downloaded. 
5. Copy the downloaded ```.p12``` and ```.pem``` files to ```src/main/resources``` folder in your code.
6. Open ```src/main/resources/application.yml``` and configure:
    - ```mastercard.api.environment.key-file ```- Path to keystore (.p12) file, just change the name as per the downloaded file in step 5. 
    - ```mastercard.api.authentication.consumer-key``` - Copy the Consumer key from "Sandbox/Production Keys" section on your project page
    - ```mastercard.api.authentication.keystore-alias``` - Alias of your key. Default key alias for sandbox is ```keyalias```.
    - ```mastercard.api.authentication.keystore-password``` -  Password of your Keystore. Default keystore password for sandbox project is ```keystorepassword```.
    - ```mastercard.api.encryption.key-file ```- Path to encryption key (.pem) file, just change the name as per the downloaded file in step 5. 
    ```mastercard.api.encryption.fingerprint ```- Fingerprint, copy the fingerprint from Client Encryption Keys section. If you have multiple client encryption keys then copy the fingerprint of the key which you want to use.

## Use-Cases <a name="use-cases"></a>
1. **Check Eligibility**
endpoint "/eligibility"   
Used to check eligibility of a credit card in the elevate program for a specific benefit.

2. **create Redemptions**    
endpoint "/redemptions"
Used to create a redemption for a credit card that was previously enrolled through the eligibilities resource.

More details can be found [here](https://stage.developer.mastercard.com/elevate/documentation/use-cases/).    


## Execute the Use-Cases   <a name="execute-the-use-cases"></a>
1. Run ```mvn clean install``` from the root of the project directory.
2. There are two ways to execute the use-cases:
    1. Execute the use-cases(test cases):  
        - Go to ```src/main/java/com/mastercard/developer/elevate/accelerator/``` folder.  
        - Execute each test cases.
    
    2. Use REST API based Client( such as [Insomnia](https://insomnia.rest/download/core/) or [Postman](https://www.postman.com/downloads/))  
        - Run ```mvn spring-boot:run``` command to run the application.  
        - Use any REST API based Client to test the functionality. Below are the APIs exposed by this application:  
                - POST <Host>/elevate/eligibility
                - POST <Host>/elevate/redemptions            
             
                                                                               
## Service Documentation <a name="documentation"></a>

Elevate Accelerator Proxy documentation can be found [here](https://stage.developer.mastercard.com/elevate/documentation/use-cases/).


## API Reference <a name="api-reference"></a>
The Swagger API specification can be found [here](https://stage.developer.mastercard.com/elevate/documentation/api-reference/).

## Support <a name="support"></a>
Please send an email to **apisupport@mastercard.com** "need to check" with any questions or feedback you may have.


## License <a name="license"></a>
<p>Copyright 2021 Mastercard</p>
<p>Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with
the License. You may obtain a copy of the License at:</p>
<pre><code>   http://www.apache.org/licenses/LICENSE-2.0
</code></pre>
<p>Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on
an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the
specific language governing permissions and limitations under the License.</p>
