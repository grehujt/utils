# Notes on Salted Password Hashing - Doing it Right

ref: [here](https://crackstation.net/hashing-security.htm)

## Best Practice to protect user password
- DO NOT WRITE YOUR OWN CRYPTO
- Do not use short salt, which should at least as long as the hash output
- Do not reuse salt, regeneration is needed when user creates or changes password
- Salt should be generated using a Cryptographically Secure Pseudo-Random Number Generator (CSPRNG).
- Procedure on storing password
    + Generate a long random salt using a CSPRNG.
    + Prepend the salt to the password and hash it with a standard password hashing function like __Argon2, bcrypt, scrypt, or PBKDF2__.
    + Save both the salt and the hash in the user's database record.
- Always hash on the server side
- Consider to use key stretching, which is implemented using a special type of CPU-intensive hash function to make running dictionary or brute-force attacks less effective.Use a standard algorithm like __PBKDF2 or bcrypt__. 
- Why do I have to use a special algorithm like HMAC? Why can't I just append the password to the secret key?
    + Use HMAC to avoid length extension attacks.
