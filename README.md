#PyAes256

This project aims to encrypt text with AES-256 and put it into a pdf similar to a paperwallet.
It is useful for example to encrypt seeds or passowords to a piece of paper and print it.
The output is base64 encoded and follows the same schme that is generated by openssl.
Eg. the base64 string starts with "Salt__" then is followed by 8 bytes of salt. The Rest is the cypertext.

## Getting Started
1. Install python >3.8
1. Install [pipenv](https://pypi.python.org/pypi/pipenv) by executing `pip install pipenv`.
1. Create a new virtual environment with all dependencies by executing `pipenv install`.

## Activating the virtual environment
Before executing any of the commands below, you need to activate the virtual environment.
You can do so by executing `pipenv shell`.
Your command prompt should now indicate that you've activated the virtual environment.
It can be deactivated by executing `exit`.  

## Generating encrypted passwords 
to encode text with aes256-ecb. Pick any password.

To run the encryption:
``
Example password = `abcdefghijklmnopqrstuvwxyz`

## Additional Notes
To verify that this script outputs the same cyphertexts as other tools and it is reproducible you can verify the output with openssl

generate with openssl:
`echo -n 'mysupersecretseedphrase' | openssl enc  -aes-256-ecb -base64 -salt -pbkdf2 -out secretphrase-enc.txt`

Then type in your password.

to decode it run: 
`openssl enc -aes-256-ecb -d -base64 -salt -pbkdf2  -in secretphrase-enc.txt`

### Parameters used for AES-256 encryption

1. pbkdf2 with 10000 iterations: to derive the pass
1. pkcf#7 to pad the plaintext to the correct blocksize which must be multiple of 16
