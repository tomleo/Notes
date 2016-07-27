# Security

## Asymetric Hashing Functions

[Why are hash functions one way?](http://security.stackexchange.com/questions/11717/why-are-hash-functions-one-way-if-i-know-the-algorithm-why-cant-i-calculate-t/19658#19658)

## Key Stretching
- makes weak keys more secure by increasing the time it takes to test each possible key
- creates an enhanced key
- Implementation: A cryptographic hash function or a block cipher may be repeatedly applied in a loop

## Salt-n-pepper

Peppering is less secure, [http://stackoverflow.com/a/16896216/465270](http://stackoverflow.com/a/16896216/465270)

Salts protect against rainbow table attacks, that's not to say an
individual hash with a salt is stronger, but the table of hashes as a
whole is more secure. [http://crypto.stackexchange.com/a/1716/31430](http://crypto.stackexchange.com/a/1716/31430)

## GPG

``` cmd
~> gpg --import bob.gpg
~> gpg --list-keys
/users/alice/.gnupg/pubring.gpg
---------------------------------------
pub  1024D/9E98BC16 1999-06-04 Blake (Executioner) <blake@cyb.org>
sub  1024g/5C8CBD41 1999-06-04
~> gpg --edit-key blake@cyb.org
Command> fpr
pub  1024D/9E98BC16 1999-06-04 Blake (Executioner) <blake@cyb.org>
             Fingerprint: 268F 448F CCD7 AF34 183E  52D8 9BDE 1A08 9E98 BC16
Command> sign
Command> check
uid  Blake (Executioner) <blake@cyb.org>
sig!       9E98BC16 1999-06-04   [self-signature]
sig!       BB7576AC 1999-06-04   Alice (Judge) <alice@cyb.org>
```

1. A DSA keypair is the primary keypair usable only for making
   signatures. An ElGamal subordinate keypair is also created for
   encryption.
2. creates only a DSA keypair

Keys are on a keyring located @ ~/.gnupg/pubring.gpg

Generate Plain Text Public Key:
gpg --armor --export opower

If you want to encrypt a message to Alice, you encrypt it using
Alice's public key, and she decrypts it with her private key. If Alice
wants to send you a message, she encrypts it using your public key,
and you decrypt it with your private key.

A signature is created using the private key of the signer. The
signature is verified using the corresponding public key.

Signing public key components with the corresponding private master
signing key is called self-signing, and a public key that has
self-signed user IDs bound to it is called a certificate.




### Generate Entropy on Remote Server

``` cmd
sudo apt-get install rng-tools
```



    
