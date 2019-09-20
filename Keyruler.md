# Keyruler

Keyruler is a project with two main parts. The Keyruler server handles creating and storing key while the Keyruler client (library)
handles encryption and decryption of data using the keys.

## Keyruler server

### Context
When a key is created a context for that key is supplied. That context will tell the server how that key can to be handled. Currently the
onlt thing this changes is how long the key exists for.

### Key and key id
The key is 32 byte value saved as a Base64 string and the key id is a 5 byte value saved as a Base64 string.

### API
The server has the following HTTP endpoints for handling keys.

#### POST /newKey
This creates and returns a key and a key id (kid). The user is requied to supply a `context` as a query parameter which tells the server
how the key should be handled. All incorrect requests return HTTP code 500.

#### GET /getKey
Returns the key connected to the given key id (kid) query parameter. All incorrect requests return HTTP code 500.

#### DELETE /deleteKey
Removes the key of the given key id (kid) query parameter. All incorrect requests return HTTP code 500.

## Keyruler client

### API

#### encrypt(host, context, data)
Encrypts the `data` using a new key from the server at `host`. The key will have the context of `context`. The encryption is AES-256-GCM
using a random 16 byte IV (Initialization Vector). The returned data is a string of the structure `IV:CIPHER_TEXT:AAD:AUTH_TAG` where AAD
currently is just the key id (kid).

#### decrypt(host, encrypted)
Decrypts `encypted` using the data in the string. The keys are fetched from the server at `host`. The decyption is done with the same
tools as the encryption.

#### doHMAC(host, data)
Creates a HMAC value of the `data` using a key with the context "hmac" from the server at `host`. The HAMC is done with SHA-256.
