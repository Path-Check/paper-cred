# **STATUS** Payload

Fields in the **serialization** order:
1. `vaccinated`: *Required.* **SHORTNUMERIC**. The vaccination status of the **HOLDER**. Currently designated values are below. Future versions of this specification may designate other values as required.
   * 0: The **HOLDER** has not received any vaccination.
   * 1: The **HOLDER** has started, but not completed, a course of vaccination.
   * 2: The **HOLDER** has completed the full vaccination course.
1. `passkey`: *Required.* **HASH**. The cryptographic hash of the data in the **Passkey**, as defined by the **Passkey** Specification.

## JSON example:
```json
{
  "type": "status",
  "version": 1,
  "data": {
    "vaccinated": 2,
    "passkey": "d9116bbdf7e33414b23ce81b2d4b9079a111d7119be010a5dcde68a1e5414d2d"
  },
  "signature": {
    "keyId": "1a9.cdc",
    "base32": "GBCAEIBS5LZ5JUYHBF3HGJABGROYE7QCP6YOZKTLDE67INBSVZVDBJ6ZQIBCALCKC3LQBOFB2P7TM4RG26526Z5ANE5Y5CPZAPFLM4XPLLPRJYXG"
  }
}
```

