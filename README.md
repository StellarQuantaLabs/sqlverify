# SQL Verify DID Specification (Development Beta App)

[![](https://img.shields.io/badge/Status-Draft-pink.svg?style=flat-square)](#Status)

This document (currently in work) outlines the DID specification for **SQL Verify**, a React-based single-page application (SPA) that will be hosted at `https://stellarquantalabs.org/sql-verify` and through Microsoft Marketplace when ready for production. SQL Verify leverages the `did:web` method to manage its decentralized identity and verification processes.

## Overview

- **Application Name**: SQL Verify
- **Platform**: React-based Single Page Application (SPA)
- **Production Host**: `https://stellarquantalabs.org/sql-verify`
- **DID ID**: `did:web:stellarquantalabs.org`
- **DID Document**: Hosted at `https://stellarquantalabs.github.io/.well-known/did.json`
- **DID Configuration**: Available at `https://stellarquantalabs.github.io/.well-known/did-configuration.json`
- **Identity Verification**: Uses Microsoft Authenticator for Multi-Factor Authentication (MFA)
- **Key Management**: Integrates with Microsoft Entra Verified ID and Microsoft Azure Key Vault for secure key storage and rotation.

## Authentication Architecture
The following diagram illustrates the Microsoft Entra Verified ID architecture that is taking place while a user is utilizing the SQL Verify App in order to retain their Verified ID for Microsoft Verification:
![SQLverify authentication microsoft](https://github.com/StellarQuantaLabs/sqlverify/blob/main/SQL-Verify-authorization.png)

## DID Document

The DID Document for SQL Verify is hosted by Stellar Quanta Labs utilizing its verified domain `stellarquantalabs.org`, and is as follows:
```
{
  "id": "did:web:stellarquantalabs.org",
  "@context": [
    "https://www.w3.org/ns/did/v1",
    {
      "@base": "did:web:stellarquantalabs.org"
    }
  ],
  "service": [
    {
      "id": "#linkeddomains",
      "type": "LinkedDomains",
      "serviceEndpoint": {
        "origins": [
          "https://stellarquantalabs.org/"
        ]
      }
    },
    {
      "id": "#hub",
      "type": "IdentityHub",
      "serviceEndpoint": {
        "instances": [
          "https://hub.did.msidentity.com/v1.0/f708479f-d874-4393-8c8f-8714e8b926a9"
        ]
      }
    }
  ],
  "verificationMethod": [
    {
      "id": "#8c096cd800e5459c873c07e96f0c9146vcSigningKey-c17d2",
      "controller": "did:web:stellarquantalabs.org",
      "type": "JsonWebKey2020",
      "publicKeyJwk": {
        "crv": "P-256",
        "kty": "EC",
        "x": "Ir9QTyM4YcFg1BUuX88Glfmztt-PrOYmSV8axSWuWSk",
        "y": "k6_CXsAmNFx29vHVPV5_BUpwqIRes_xVD80r_BoZ3Kc"
      }
    }
  ],
  "authentication": [
    "#8c096cd800e5459c873c07e96f0c9146vcSigningKey-c17d2"
  ],
  "assertionMethod": [
    "#8c096cd800e5459c873c07e96f0c9146vcSigningKey-c17d2"
  ]
}
```
## Hosted Files

1. **DID Document**:
    - URL: `https://stellarquantalabs.github.io/.well-known/did.json`
    - Contains verification keys and service endpoints for SQL Verify.
2. **DID Configuration**:
    - URL: `https://stellarquantalabs.github.io/.well-known/did-configuration.json`
    - Describes the DID's configuration and links it to the hosting domain.

## Authentication and Verification
SQL Verify uses Microsoft Authenticator Application for multi-factor authentication (MFA) to verify the identity of authorized employees. This approach ensures secure identity verification and access control.

### Key Features:

- **Identity Hub**:
    - Service endpoint: https://hub.did.msidentity.com/v1.0/f708479f-d874-4393-8c8f-8714e8b926a9
    - Acts as a data storage and management hub for decentralized identity data.

- **Linked Domains**:
    - Validates the association of did:web:stellarquantalabs.org with https://stellarquantalabs.org/sql-verify.

- **Key Management**:
    - Uses Microsoft Azure Key Vault for secure storage and management of cryptographic keys.
    - Keys are rotated periodically in accordance with best practices, as documented [here](https://learn.microsoft.com/en-us/entra/verified-id/how-to-rotate-keys).

- **Microsoft Entra Verified ID**:
    - Provides integration for issuing and verifying decentralized credentials.

## Integration Details

### React-Based SPA Development
The React app was developed following the guidelines provided in the [Microsoft SPA Tutorial](https://learn.microsoft.com/en-us/entra/identity-platform/tutorial-single-page-app-react-prepare-spa?tabs=visual-studio). Key integration steps included:

- **Registering the App in Azure AD**:
    - Configured app registration for identity platform support.

- **Adding MSAL.js Library**:
    - Implemented @azure/msal-react for handling authentication.

- **Configuring Routes and Authentication Flow**:
    - Setup routes to include protected pages, requiring users to authenticate via Microsoft Authenticator.

## Verifiable Credentials Configuration
Following the [[Microsoft Verified ID Documentation](https://learn.microsoft.com/en-us/entra/verified-id/verifiable-credentials-configure-issuer)](https://learn.microsoft.com/en-us/entra/verified-id/verifiable-credentials-configure-issuer):

- Configured the issuance and verification of verifiable credentials.
- Ensured compatibility with `did:web` for decentralized identity.
    
## CRUD Operations
SQL Verify supports basic Create, Read, Update, and Delete (CRUD) operations for managing DID documents and related data. Below is an example of how these operations are implemented:

### **Create**
Adding a new DID document to the system:
```
{
  "@context": "https://w3id.org/did/v1",
  "id": "did:web:stellarquantalabs.org",
  "verificationMethod": [
    {
      "id": "did:web:stellarquantalabs.org#keys-2",
      "type": "JsonWebKey2020",
      "controller": "did:web:stellarquantalabs.org",
      "publicKeyJwk": {
        "kty": "RSA",
        "n": "new-public-key-value",
        "e": "AQAB"
      }
    }
  ]
}
```
### **Read**
Retrieving the existing DID document:
```
GET https://stellarquantalabs.github.io/.well-known/did.json
```

### **Update**
Updating the verification method in the DID document:
```
{
  "@context": "https://w3id.org/did/v1",
  "id": "did:web:stellarquantalabs.org",
  "verificationMethod": [
    {
      "id": "did:web:stellarquantalabs.org#keys-1",
      "type": "JsonWebKey2020",
      "controller": "did:web:stellarquantalabs.org",
      "publicKeyJwk": {
        "kty": "RSA",
        "n": "updated-public-key-value",
        "e": "AQAB"
      }
    }
  ]
}
```
### **Delete**
Removing a verification method from the DID document:
```
{
  "@context": "https://w3id.org/did/v1",
  "id": "did:web:stellarquantalabs.org",
  "verificationMethod": []
}

```
## Deployment

### Hosting Environment
- The React SPA code utilizes Visual Studio Code 2019 as its IDE
- The app (when ready for production) will be hosted at: `https://stellarquantalabs.org/sql-verify`.
- The `.well-known` directory is accessible at: `https://stellarquantalabs.github.io/.well-known/`.

### Build and Deployment

1. **React Build**:
    - Production Build will be created after development build (using `npm start`) compiles successfully and has been optimized, by using `npm run build`.
    - Ensures all assets are prepared for production hosting.
2. **Deployment Process**:
    - Files (will be) deployed to the `/sql-verify` subdirectory on `stellarquantalabs.org` when Production build is ready.

## Conclusion

This specification establishes SQL Verify's identity and operational structure in compliance with W3C DID standards. Leveraging did:web, robust authentication via Microsoft Authenticator, and secure key management with Microsoft Azure Key Vault, SQL Verify ensures secure, verifiable access for employees and users.

## References

Will be addressing all References here (in work).
