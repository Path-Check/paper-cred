# **STATUS** Payload

The **STATUS** QR code is designated to contain information about the vaccination status of the **HOLDER**.

Fields in the **serialization** order:
1. `vaccinated`: *Required.* **SHORTNUMERIC**. The vaccination status of the **HOLDER**. Currently designated values are below. Future versions of this specification may designate other values as required.
   * 0: The **HOLDER** has not received any vaccination.
   * 1: The **HOLDER** has started, but not completed, a course of vaccination.
   * 2: The **HOLDER** has completed the full vaccination course.
1. `passkey`: *Required.* **HASH**. The cryptographic hash of the data in the **Passkey**, as defined by the **Passkey** Specification.

## Example:

```
CRED:STATUS:1:GBCAEIDMK3P56TZVRCJNOKC62ZDDTWKXVKLRGGQQIGNZPGRUPMHT7O4TW4BCA45HQE45XOSXW6GGG56
2N4YT5YVHBG4YZWXXEIGFPH5ANKT5X2O7:KEYS.PATHCHECK.ORG:1/W4XL4HM7VV3G6TXSALXZNPUVAZD2RZP6Y2Q
LNKLXD5NA7LSVQAVQ
```
