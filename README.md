Note <br>
This is a copy from a repository created by The-Crocop. All credit goes to The-Crocop. I inspired the project, but all work was done by The-Crocop. The original repository can be found here:https://github.com/The-Crocop/pyaes256

# PyAes256

This project aims to encrypt text with AES-256 and put it into a pdf similar to a paperwallet.
It is useful for example to encrypt seeds or passwords to a piece of paper and print it.
The output is base64 encoded and follows the same scheme that is generated by openssl.
Eg. the base64 string starts with "Salt__" followed by 8 bytes of salt. The Rest is the ciphertext.

## Disclaimer

<b>Use at your own risk.</b> The author takes no responsibility for any damage, lost funds or similar due to errors in encryption or any other mistakes.
Always be cautios when using this tool for important or high value data. Always verify that the decryption script yields the encrypted plaintext.
Additionally, crosscheck with openssl if you can decrypt your encrypted data before using it. 

## Usage

The simplest way is to get the file from pip.
`pip install pyaes256`

### Encryption

Then execute it with: `pyaes256 encrypt myplaintext`

Then give it a password and confirm. You can optionally specify a 
`--password <mypassword>` argument.

Then we will not ask you to confirm it. 

To set a different output file use 
`--output targetFile`

To see the generated key and IV use
`--showKey`

You can download a single executable standalone exe of the latest release [here](https://github.com/The-Crocop/pyaes256/releases)

Unpack it and run:
`pyaes256.exe encrypt "myplaintext"`

As mentioned below it is important to install gtk3 before running the tool!
You can download it here https://github.com/tschoonj/GTK-for-Windows-Runtime-Environment-Installer/releases

### Decryption

`pyaes256.exe decrypt <base64encryptedcyphertext>`

Then just type in your password you set when encrypting.


## Getting Started
1. Install python >3.8
1. Install [pipenv](https://pypi.python.org/pypi/pipenv) by executing `pip install pipenv`.
1. Create a new virtual environment with all dependencies by executing `pipenv install`.
1. Important: PyAes256 uses [weasyprint](https://github.com/Kozea/WeasyPrint) to generate the pdf with the encrypted QR Code.
   In order to make it work on different Operating Systems it is required  to install certain libraries, that are needed for rendering..
   
   For Windows you have to install the GTK+ Libraries.
   You can download it here https://github.com/tschoonj/GTK-for-Windows-Runtime-Environment-Installer/releases
   
   Pick the gtk3 runtime and install it. After that restart your terminal. Also select the PATH option
   
   More information at:
   [https://weasyprint.readthedocs.io/en/stable/install.html#step-4-install-the-gtk-libraries](https://weasyprint.readthedocs.io/en/stable/install.html#step-4-install-the-gtk-libraries)
   
## Activating the virtual environment
Before executing any of the commands below, you need to activate the virtual environment.
You can do so by executing `pipenv shell`.
Your command prompt should now indicate that you've activated the virtual environment.
It can be deactivated by executing `exit`.  

## Build single executable
run `pyinstaller pyaes256.spec`

the executable will be generated in dist folder.
Run it with in linux
`./dist/pyaes256  encrypt "hhhhh"`

On Windows: 
`dist/pyaes256.exe  encrypt "hhhhh"`

The paper wallet is generated into the output folder.

## Generating encrypted passwords 
to encode text with aes256-cbc. Pick any password.

## Additional Notes
To verify that this script outputs the same cyphertexts as other tools and it is reproducible you can verify the output with openssl

generate with openssl:
`echo -n 'mysupersecretseedphrase' | openssl enc  -aes-256-cbc -base64 -salt -pbkdf2 -out secretphrase-enc.txt`

Then type in your password.

to decode it run: 
`openssl enc -aes-256-cbc -d -base64 -salt -pbkdf2  -in secretphrase-enc.txt`

### Parameters used for AES-256 encryption

1. pbkdf2 with 10000 iterations: to derive the pass
1. pkcf#7 to pad the plaintext to the correct blocksize which must be multiple of 16
