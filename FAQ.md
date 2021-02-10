# Answers to Frequently Asked Questions
### Why does this specification support JSON and URI formats, but not XML?
While there are some benefits to XML for data interchange, we found it unsuitable for this purpose for the following reasons: XML-formatted messages are more verbose than JSON or URI, and space is at a premium in a QR code. Parsing XML tends to be slower and more memory-intensive than parsing JSON or URI. XML’s extensibility is not needed (or wanted) for this specification.

### Why does this specification use ECDSA over RSA? 
While there are many benefits of RSA for encryption, we adopted ECDSA for several reasons, including the following: 
 * ECDSA offers the same level of security with smaller key sizes. Encrypted message is a function of key size and data size for both RSA and ECDSA and since ECDSA key size is relatively smaller than RSA key size, thus encrypted message in ECDSA is smaller. The length of the public and private keys is much shorter in ECDSA. This results in faster processing times, and lower demands on memory(QR Codes) and bandwidth. 
 * It has easy hardware implementations and provides relatively fast operations in key generation, signature generation and signature verification.

### Why does this specification target the Alphanumeric QR Code type?
The Alphanumeric QR Code type imposes significant limitations on the data which can be represented, but allows for the generation of a lower-resolution QR code. Smaller QR codes will be scannable with older hardware and lower-resolution scanners, and smaller data sets allow for more aggressive error correction. This promotes usability and equity.

### Which characters are supported by the QR Code Alphanumeric specification?
The following characters are supported by the specification. To manage data set size, lowercase characters should be converted to uppercase characters before QR generation. Characters in the following list should not be percent encoded,as this increases bytes-per-character from one to three. Any characters not appearing in the following list MUST be percent encoded before QR code generation. 

```
0 1 2 3 4 5 6 7 8 9 A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
$ % * + - . / :
```
