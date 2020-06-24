### Android keystore system
The Android Keystore system lets you store cryptographic keys in a container to make it more difficult to extract from the device.

Android Keystore system protects key material from unauthorized use. Firstly, Android Keystore mitigates unauthorized use of key material outside of the Android device by preventing extraction of the key material from application processes and from the Android device as a whole. Secondly, Android KeyStore mitigates unauthorized use of key material on the Android device by making apps specify authorized uses of their keys and then enforcing these restrictions outside of the apps' processes.

### Keymaster  Understanding:
#### Storing Fixed Keys
To understand the Android Keystore API, you must first understand that encrypting secrets requires both public key and symmetric cryptography. In public key cryptography, data can be encrypted with one key and decrypted with the other key. In symmetric cryptography, the same key is used to encrypt and decrypt the data. The Keystore API uses both types of cryptography in order to safeguard secrets.
A public/private key RSA pair is generated, which is stored in the Android device's keystore and protected usually by the device PIN. An AES-based symmetric key is also generated, which is used to encrypt and decrypt the secrets. The app needs to store this AES symmetric key to later decode, so it is encrypted by the RSA public key first before persisted. When the app runs, it gives this encrypted AES key to the Keystore API, which will decode the data using its private RSA key. In this way, data cannot be decoded without the use of the device keystore.


#### Key Generation
Generate a pair of RSA keys;<br/>
Generate a random AES key;<br/>
Encrypt the AES key using the RSA public key;<br/>
Store the encrypted AES key in Preferences.<br/>

#### Encrypting and Storing the data
Retrieve the encrypted AES key from Preferences;<br/>
Decrypt the above to obtain the AES key using the private RSA key;<br/>
Encrypt the data using the AES key;<br/>

#### Retrieving and decrypting the data
Retrieve the encrypted AES key from Preferences;<br/>
Decrypt the above to obtain the AES key using the private RSA key;<br/>
Decrypt the data using the AES key<br/>



------

#### Keymaster  Understanding:
Storing Fixed Keys
To understand the Android Keystore API, you must first understand that encrypting secrets requires both public key and symmetric cryptography. In public key cryptography, data can be encrypted with one key and decrypted with the other key. In symmetric cryptography, the same key is used to encrypt and decrypt the data. The Keystore API uses both types of cryptography in order to safeguard secrets.
A public/private key RSA pair is generated, which is stored in the Android device's keystore and protected usually by the device PIN. An AES-based symmetric key is also generated, which is used to encrypt and decrypt the secrets. The app needs to store this AES symmetric key to later decode, so it is encrypted by the RSA public key first before persisted. When the app runs, it gives this encrypted AES key to the Keystore API, which will decode the data using its private RSA key. In this way, data cannot be decoded without the use of the device keystore.
