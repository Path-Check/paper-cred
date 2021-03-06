# PathCheck Vaccination Credential - Version 1
**© 2021 PathCheck Foundation**<br/>
Authors: [Justin Dosse](mailto:justin.dossey@pathcheck.org) and [Vitor Pamplona](vitor.pamplona@pathcheck.org)<br/>
Status: *DRAFT*<br/>
Date: Feb 26, 2021<br/>

# Purpose
This document describes the format to compress Verifiable Credentials for space-limited applications, such as QR Codes, NFC tags and SMS Messages.

## Terms and Definitions

For the purposes of brevity, this document refers to the following terms which are defined as follows:
1. **HOLDER**: The **HOLDER** is the party who has been or will be vaccinated, and is holding a pre-printed vaccination credential card.
1. **ISSUER**: The **ISSUER** is the party who delivers the vaccine and credential to a **HOLDER**.

## Global Data Types

This document will use the following terms to define data types.
1. **NUMERIC**: The **NUMERIC** data type is a sequence of integers between 0 and 99999999, inclusive.
2. **STRING**: The **STRING** data type is a sequence of UTF-8, [NFC Normalized](https://www.unicode.org/faq/normalization.html) characters, up to 255 bytes after encoding.
3. **HASH**: The **HASH** data type is a sequence of 52 alphanumeric characters containing a base32-encoded hash.
3. **SIGNATUREBASE32**: The **SIGNATUREBASE32** data type is a sequence of up-to-102 alphanumeric characters containing a base32url-encoded digest.
4. **DATE**: a date, in [ISO 8601 (YYYYMMDD) Basic Notation](https://en.wikipedia.org/wiki/ISO_8601). Example: `20200201` is 1 February, 2020.
5. **SHORTSTRING**: a sequence of US-ASCII characters which is limited to 8 bytes in length.
6. **SHORTNUMERIC**: a **NUMERIC** with a maximum value of 9.
7. **PHONE**: a E.164 formatted phone number as string. US-ASCII, maximum 15 characters.

# General Structure

All QR codes contain a message with: 
1. the **type** of the certificate
1. the **version**
1. the **payload** itself
1. the reference to a **public key**
1. and a cryptographic **signature** of the payload

The **type** field declares the [payload](payloads) type (e.g. `COUPON`, `PASSKEY`, `BADGE` or `STATUS`) and the **version** is a **NUMERIC** field defining the version of the type of certificate. The **payload** section contains the information related to the certificate itself. The cryptographic signature is a SHA256 signature in Base32 form, calculated using the private key of the **ISSUER**. 

The reference to the **public** key can be: 
1. a FQDN to a DNS TXT Record containing the key for download (TODO: Need to specify the format of the TXT Record).
1. a URL address to download keys from. 
1. a database and key ID to facilitate trusted lists of issuers (TODO: Need to specify this keyId database format). 

For signature verification, devices should maintain an indexed local key-value stores of approved public keys in PEM format. 

Data represented in QR codes can be encoded in JSON and URI formats, but URI formats are strongly preferred due to their smaller message size. Examples are in JSON for easier readability but both encodings are described in this document.

## Payload Types

All payload type specifications must be submitted as pull requests to the [payload](payloads) folder in this repository. Payload specifications define the syntax and semantic meaning of the fields as well as their order in the serialization process. 

## Case Insensitivity

All fields (keys as well as values) are case-insensitive in both JSON and URI format. When performing operations such as hash comparison, a case-insensitive comparison function MUST be used. Additionally, the Alphanumeric QR Code character set does not include lowercase characters, so implementations MUST encode output in uppercase only.

For clarity and ease of reading, examples in this document are given in mixed case. 

## Payload Percent Encoding

Percent encoding of the upcased payload is used to address QR code character set limitations, while supporting the URI spec. 

## Base32URL Encoding

A version of the Base32 ([RFC4648](https://tools.ietf.org/html/rfc4648)) without padding (Base32URL) is used to encode hashes and signatures. The removal of the padding is due to the fact that `=` is not a supported character on both URI and alphanumeric QR codes. 

## Signing and Hashing

Data to be used for signing and hashes is serialized in the specified order this payload describes. Cryptographic signatures and hashes **MUST** be calculated against **uppercased**, **percent encoded** versions of the underlying payload, as they appear in the final URI. This permits signature verification before any decoding. 

The cryptographic tools must sign and verify a SHA256 hash of the UTF-8 byte array of the **uppercased**, **percent-encoded** payload. The resulting signature in Distinguished Encoding Rules (DER) format must be then encoded in Base32 ([RFC4648](https://tools.ietf.org/html/rfc4648)), removed the added padding (`=`) and added to the URI.

# The JSON Format

When data length is not at issue, this format is simple to read and parse. With the Json format, the payload is organized in the following schema:
```json
{
  "type": "",
  "version": 0,
  "data": {},
  "signature": {}
}
```

The payload section is designated the **DATA** block and the cryptographic signature is contained in the **SIGNATURE** block. A block is an object containing a number of key-value pairs. 

## Order of Fields
In the JSON format, blocks and key/value pairs may occur in any order. For example, a JSON document with the **DATA** block after the **SIGNATURE** block is equivalent to a document with the **SIGNATURE** block after the **DATA** block. Similarly, the key/value pairs within a block may appear in any order.

## The **SIGNATURE** Block
The signature block contains the Base32URL signature digest of the prepared **DATA** block and a *keyId* referencing the database and public key used to verify the signature. In the example below, the public key used to verify the signature is `1a9` in the `cdc` (local key/value) store. The period character (`.`) is used as a delimiter to separate the key-value store identifier and the key identifier.

The algorithms used for the signature as well as hashing functions (i.e. ECDSA, RSA, others) are defined by the PEM-formatted content found at the Public Key reference. At the moment of writting this document, we strongly suggest ECDSA signatures. 

1. *keyId*: **SHORTSTRING**. a string describing the database and index, the url to download the PEM, or the DNS TXT record of the
   public key to be used when verifying the cryptographic signature of the **DATA** block.
2. *base32*: **SIGNATUREBASE32**. The Base32URL-encoded SHA256 digest signature of the
   **DATA** block, calculated according to the rules above.

### JSON Example
```json
{
  "type": "coupon",
  "version": 1,
  "data": { },
  "signature": {
    "keyId": "1a9.cdc",
    "base32": "GBDAEIIA42QDQ5BDUUXVMSQ4VIMMA7RETIZSXB573OL24M4L67LYB24CZYVQEIIA2EZ5W2QXLR7LUSLQW6MLAFV3N7OTT3BDAZCNCRMYBMUYC6WMXMNQ"
  }
}
```

# The URI Format
When data length is a concern, this format may use fewer bytes than JSON. Keys should be compressed according to the URI Compression rules in this document. Values in the URI format are encoded per the standard using [Percent Encoding](https://en.wikipedia.org/wiki/Percent-encoding). Note that all characters not present in the Alphanumeric QR scheme must be percent encoded.

## URI Schema
With URI format, payload is organized according to the following URI schema starting with "cred":
```
cred:type:version:signature:keyId:payload
```

The payload should be represented as a series of **uppercased**, **percent-encoded** values delimited by the slash (`/`) character. The serialization order is defined in each type of payload specification and key names are omitted.

## URI Example

The example below is a valid credential
```
CRED:COUPON:1:GBDAEIIA42QDQ5BDUUXVMSQ4VIMMA7RETIZSXB573OL24M4L67LYB24CZYVQEIIA2EZ5W2QXLR7LUSLQW6MLAFV3N7OTT3BDAZCNCRMYBMUYC6WMXMNQ:1a9.cdc:37/5000/San%20Francisco/1B/Teacher
```

## Percent Encoding Payload Fields

All characters not present in the Alphanumeric QR scheme must be percent
encoded when presented in data fields. RFC 2936 requires percent encoding a
number of characters, but some of the characters not required to be encoded are
not included in the Alphanumeric QR character set. As a result, those characters
MUST also be percent encoded. 

## Serializing Fields on the URI format

Unfilled fields MUST be submitted as empty between slash (`/`) characters. Only add empty delimiters if there is data after. Given fields A (required), B (optional), C (optional) the implementation MUST follow the following example:

| A | B | C | Output        |
|-----|-----|-----|---------|
| `1` |     |     | `1`     |
| `1` | `2` |     | `1/2`   |
| `1` | `2` | `3` | `1/2/3` |
| `1` |     | `3` | `1//3`  |

## Which Characters Need Encoding?

The columns in the table below indicate encoding requirements for each
representable character. Any non-listed characters MUST be percent-encoded. The
"URI Requires" column indicates whether the URI format rules (RFC 2396) requires encoding the character.
The "Alphanumeric QR Requires" column indicates whether the character is missing from the
Alphanumeric QR character set (thus, requiring encoding). The "Must Encode?" column indicates whether this
specification requires percent-encoding of the character. The "Output Value"
column indicates the expected output from processing the listed character.

| Character | URI Requires | Alphanumeric QR Requires | Must Encode? | Output Value |
| --------- | -------- | --------------- | ---- | ------------ |
| ` ` | YES | NO  | YES | `%20` |
| `!` | YES | YES | YES | `%21` |
| `"` | YES | YES | YES | `%22` |
| `#` | YES | YES | YES | `%23` |
| `$` | YES | NO  | YES | `%24` |
| `%` | YES | NO  | YES | `%25` |
| `&` | YES | YES | YES | `%26` |
| `'` | YES | YES | YES | `%27` |
| `(` | YES | YES | YES | `%28` |
| `)` | YES | YES | YES | `%29` |
| `*` | YES | NO  | YES | `%2A` |
| `+` | YES | NO  | YES | `%2B` |
| `,` | YES | YES | YES | `%2C` |
| `-` | YES | NO  | YES | `%2D` |
| `.` | YES | NO  | YES | `%2E` |
| `/` | YES | NO  | YES | `%2F` |
| `0` | NO  | NO  | NO  | `0`   |
| `1` | NO  | NO  | NO  | `1`   |
| `2` | NO  | NO  | NO  | `2`   |
| `3` | NO  | NO  | NO  | `3`   |
| `4` | NO  | NO  | NO  | `4`   |
| `5` | NO  | NO  | NO  | `5`   |
| `6` | NO  | NO  | NO  | `6`   |
| `7` | NO  | NO  | NO  | `7`   |
| `8` | NO  | NO  | NO  | `8`   |
| `9` | NO  | NO  | NO  | `9`   |
| `:` | YES | NO  | YES | `%3A` |
| `;` | YES | YES | YES | `%3B` |
| `<` | YES | YES | YES | `%3C` |
| `=` | YES | YES | YES | `%3D` |
| `>` | YES | YES | YES | `%3E` |
| `?` | YES | YES | YES | `%3F` |
| `@` | YES | YES | YES | `%40` |
| `A` | NO  | NO  | NO  | `A`   |
| `B` | NO  | NO  | NO  | `B`   |
| `C` | NO  | NO  | NO  | `C`   |
| `D` | NO  | NO  | NO  | `D`   |
| `E` | NO  | NO  | NO  | `E`   |
| `F` | NO  | NO  | NO  | `F`   |
| `G` | NO  | NO  | NO  | `G`   |
| `H` | NO  | NO  | NO  | `H`   |
| `I` | NO  | NO  | NO  | `I`   |
| `J` | NO  | NO  | NO  | `J`   |
| `K` | NO  | NO  | NO  | `K`   |
| `L` | NO  | NO  | NO  | `L`   |
| `M` | NO  | NO  | NO  | `M`   |
| `N` | NO  | NO  | NO  | `N`   |
| `O` | NO  | NO  | NO  | `O`   |
| `P` | NO  | NO  | NO  | `P`   |
| `Q` | NO  | NO  | NO  | `Q`   |
| `R` | NO  | NO  | NO  | `R`   |
| `S` | NO  | NO  | NO  | `S`   |
| `T` | NO  | NO  | NO  | `T`   |
| `U` | NO  | NO  | NO  | `U`   |
| `V` | NO  | NO  | NO  | `V`   |
| `W` | NO  | NO  | NO  | `W`   |
| `X` | NO  | NO  | NO  | `X`   |
| `Y` | NO  | NO  | NO  | `Y`   |
| `Z` | NO  | NO  | NO  | `Z`   |
| `[` | YES | YES | YES | `%5B` |
| `\` | YES | YES | YES | `%5C` |
| `]` | YES | YES | YES | `%5D` |
| `^` | YES | YES | YES | `%5E` |
| `_` | NO  | YES | YES | `%5F` |
| `{` | YES | YES | YES | `%7C` |
| `}` | YES | YES | YES | `%7D` |
| `~` | NO  | YES | YES | `%7E` |

## Pseudo-Code describing signing and assembly of the URI:

```bash
$payload ::= [$number, $total, $city, $phase, $indicator];
for ($i ::= 0; $i < length($payload); $i += 1) do
  $upcasedValue ::= upcase($payload[$i]);
  $encodedValue ::= percentEncode($payload[$i]);
  $payload[$i] ::= $encodedValue;
end

$payloadString ::= join("/", $payload);
$payloadHash ::= sha256($payloadString.to("utf-8"));

$signatureDER ::= ecdsaSign($payloadHash);

$signature ::= removePadding(b32encode($signatureDER))
$base ::= "cred:coupon:" + $version + ":" + $signature + ":" + $keyId;
$upcasedBase ::= upcase($base);

$uri ::= $upcasedBase + ":" + $payloadString;
```

## Pseudo-Code describing parsing and verifying of the URI:

```bash
[$schema, $type, $version, $signature, $keyId, $payloadString] ::= qr.split(':')
$payload ::= $payloadString.split('/')

$payloadHash ::= sha256(payload.to("utf-8")))
$signatureDER ::= b32decode(addPadding($signature))

$valid ::= ecdsaVerify($signatureDER, $payloadHash)

for ($i ::= 0; $i < length($payload); $i += 1) do
  $payload[$i] ::= percentDecode($payload[$i]);  
end
```

## Pseudo-Code describing Base32 to Base32URL Mapping

To remove padding from Base32-encoded strings do: 
```bash
$base32URL ::= $base32.replaceAll("=", "");
```

To add padding back to Base32-encoded strings do:
```bash
switch ($base32NoPad.length % 8) {
    case 2: $base32 ::= $base32URL + "======"; break;
    case 4: $base32 ::= $base32URL + "====";  break;
    case 5: $base32 ::= $base32URL + "==="; break;
    case 7: $base32 ::= $base32URL + "="; break;
    default: $base32 ::= $base32URL;
}
```   


