# Paper-first Verifiable Vaccination Credentials using QR Codes. 

We present a specification to communicate COVID-related Verifiable Credentials via digitally signed QR Code stickers on paper cards. Due to its physical nature and simplicity, digitally-signed QR codes may be a convenient and nonintrusive modality for some users seeking vaccination while enabling verification of user-controlled immunization records. The protocol expects the verifier app to be off-line and that the user doesn't need anything more than a Paper Card (no electronic needs on the user side). Personally identifiable contact and health information are stored on the QR Codes themselves, allowing a user to go through the vaccination procedures in a Peer-to-Peer fashion without the need to ever transfer the user's information to any centralized, or private, or public blockchain system. The proposed system improves the efficiency, privacy, equity, and effectiveness by augmenting the existing protocols to work with fully off-line information flows. 

<p align="center"><img src="https://github.com/Path-Check/vaccine-diary/blob/main/Resources/card_visualization.gif" alt="App_gif" width="650" style="margin: auto"/></p>

# Introduction

Vaccination coordination is facing daunting challenges. Citizens are expected to navigate an array of websites and health authorities are using disconnected health IT systems. Reporting lags by several days. Following up with vaccinated subjects to monitor side effects is difficult. The systems to monitor ineffective batches of vaccines are yet to become mature. Vaccine verifications documents are prone to fraud.

We developed a modification of today's vaccination cards to add 4 signed QR Codes that manage the user journey. The separate QR Codes are intended to decouple the health information (PHI) and personally identifiable information (PII) as well as separate the eligibility of the vaccination from the distribution of it. The card dramatically simplifies phased vaccinations, second dose coordination, reporting of side effects, and credentials while allowing fully privacy-centered systems. It also creates data-rich monitoring of vaccination progress while eliminating red tape, privacy-concerns, and fraud. It is ideally suited for vulnerable populations, rural areas, labor unions of essential workers, and employers helping their own employees.

# Patient Journey

The patient journey goes through 3 major stages:
1. Eligibility Check/Scheduling: Vaccination coupon QR codes are distributed to everyone by the appropriate regional vaccination administrator. This can be done either with paper vaccination cards, by SMS, using a web site, or downloading an application. The coupon code behaves as a User ID for the entire vaccination flow. 
2. User Check-in: At the vaccination site, the patient arrives and their vaccination coupon is scanned and their eligibility and appointment are verified.
3. Vaccination Certificate: Once the vaccination is administered, the patient receives another QR code in the form of a sticker. This QR code, a badge, indicates the vaccination was administered providing “proof” of vaccination along with other important information.

The 4 QR Codes designed for vaccinations procedures create a possibility of selective disclosure of health information by choosing which one to show at any point in time. 

## The Coupon QR Code

The distribution of a vaccination coupon signifies the beginning of the vaccination process. When everyone is given a vaccination coupon the fear and anxiety around the vaccination process and when and if someone will be vaccinated is alleviated. By starting the patient journey with the distribution of the vaccination coupon, everyone has more peace of mind with the knowledge that they are on the list to be vaccinated, and that they can play an active role in the process. The coupon only has information about the vaccination phase this user has been approved for and nothing else. 

## The Badge QR Code

The badge contains information about the vaccination itself. It's a full health record, including vaccine brand, dosage, site, etc. It is signed by the vaccination provider and serves as a Verifiable Credential informing that the user in the badge was vaccinated under those conditions. This QR Code should be shown when the User goes to see his Primary Care Provider or other health care professionals that need to see the details of his procedure. 

## The Passkey QR Code

The PassKey contains personally identifiable information. It's what links all other QR Codes to the person. A hash of the signed PassKey is used as an identifier on the other QR Codes. 

## The Status QR Code

The Status QR Code contains only one field: vaccinated or not. This QR Code is ideal to use as a check-in credential that verifies the user has been vaccinated, without revealing anymore more information about the vaccine or the user itself. 

# Protocol Releases

## V1

* [Specification](SPECIFICATION.md)
* [FAQ](FAQ.md)

# Contributing
