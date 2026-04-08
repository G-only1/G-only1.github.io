---
{"dg-publish":true,"permalink":"/sec-110/notes/mod-7-cryptography-basics/","tags":["SEC-110","Notes"]}
---

# Notes
- MITRE ATT&CK framework will be a good resource for the final project
- Cryptography makes every MFA system secure under the hood
- MFA uses 2 of the 3 below
	- What you know
		- Username, password, pin, security question, etc.
	- What you have
		- Key card, USB, smart phone, email address, etc.
	- What you are (biometric)
		- Fingerprint, facial recognition, voice recognition, retina scan, etc. 
- 2 basic encryption methods
	- Symmetric Encryption
		- One secret key
			- Applied to a message to change the content in a particular way 
			- If sender and recipient know the secret key, they can encrypt and decrypt messages 
		- Difficult to securely exchange encryption key
		- very fast
		- usually used for data at rest
		- Atbash Cipher
			- Atbash Cipher is a cipher that simply works by reversing the alphabet.
			- also known as the mirror code
			-  the word “Apple” would be decoded to “Zkkov”
			- Caesar Cipher
				- Shifts the alphabet by a fixed key
	- Asymmetric Encryption
		- Uses key pairs
			- Public key – A key made available to anyone 
			- Private key – Only the key owner knows
		- solves key transfer problem
		- slower
		- used for data in transit
		- Any message encrypted using the public key can only be decrypted by the matching private key.
		- Any message encrypted using the private key can only be decrypted by the matching public key. 
		- Pros of Asymmetric Encryption: no need to exchange keys 
		- Cons of Asymmetric Encryption: slow, requires more processing
		- Asymmetric encryption can be used to exchange a Symmetric encryption key securely
	- Digital Certificates
		- A digital certificate is an electronic file that verifies the identity of of a user, device, or website.
		- Ensure that a party cannot deny the sending of a message that they originated.
			- a “virtual fingerprint” unique to a person or entity 
			- use both Digital Certificates and Hashes
		- Contains the public key for the connection
		- Signed by a certificate authority (a trusted source for creating and issuing certificates)
		- Systems exchange certificates or public keys using SSL/TLS to establish an encrypted connection
	- Hash Functions
		- Hash Function: An algorithm that computes a fixed-bit-length string from a block of data and used to test the integrity of a file
		- Popular hashing algorithms: 
			- MD5 – creates 128-bit message digest
			- SHA-1: Creates 160-bit message digest 
			- SHA-2: 256 and 512-bit message digests 
## Vocabulary & Key Terms
- Cryptography: The art of writing and solving codes
- Cryptology: The study of cryptography 
- Cipher: Method/code used to disguise text 
- Plaintext: the original text 
- Encrypt/Encode: the process of disguising 
- Ciphertext: the disguised text 
- Decrypt/Decode: Remove disguise
- Hash Function: An algorithm that computes a fixed-bit-length string from a block of data and used to test the integrity of a file
### New CIA Terms
- Confidentiality: Using cryptography to keep data private
- Integrity: Using cryptography to ensure that data has not been altered
- Availability: Timely and ready access to information 
- Authentication: Verifying that user is who they claim 
- Non-repudiation: Ensure that user performed activity by proof that they cannot deny 
	- Like a digital receipt you cant throw away
	- digitally signing a document

# Slides
[[SEC-110/Notes/Mod 7- Cryptography Basics\|Mod 7- Cryptography Basics]]