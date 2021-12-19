---
title: "Attending Atala Prism Pioneers"
author: "Denzil Ferreira"
date: '2021-11-01'
slug: []
categories: Cardano
tags: learning
Description: ''
Tags: []
Categories: []
DisableComments: no
---

These are my lecture notes from attending Atala Prism Pioneers program, by IOHK and taught by Dr. Lars Brünjes.

# Lecture 1

[![Lecture 1](https://img.youtube.com/vi/9MoWZ_dHqpE/0.jpg)](https://www.youtube.com/watch?v=9MoWZ_dHqpE)

## Concepts

### DID - Digital Identifiers

A globally unique URI. It can identify a person, organization, thing, data model, abstract entity. A DID-Controller can cryptographically prove being in control. It is a spec from WWW Consortium at <https://www.w3.org/TR/did-core>. In Prism, DIDs are managed by Cardano's blockchain.

An example DID follows this structure:

> scheme:DID Method:DID method-specific identifier

e.g., did:<example:123456789abcdefghi>

### Verified Credentials

Can be *issued by* an **Issuer**, identified by a DID. Can be *issued to* a **Holder**, the subject, identified by a DID. Can be passed to and verified by a **Verifier** and it is specified by WWWC: <https://w3c.github.io/vc-data-model>

e.g., a university (Issuer) provides to a student (Holder) a degree certificate (Credential). This credential can be verified (using a Verifier) by an employer.

### Idea for Atala PRISM project

A **search engine for employers to find certified professionals** for a role (vertical: career/reputation). The **Holder** is the job seeker/professional. The credentials that would be issued could be:

-   Proof of Employment at company Xpto
-   Recommendation from another verified user
-   Proof of Degree
-   Proof of certification taken by AWS/Azure/Google Cloud/etc
-   ...

The **Issuers** are the companies/universities/training entities. The **Verifier** is the search engine, used by employers.

# Lecture 2

[![Lecture 2](https://img.youtube.com/vi/KjWORZnbllE/0.jpg)](https://www.youtube.com/watch?v=KjWORZnbllE)

### DID Operations

> Specified in: <https://w3c-ccg.github.io/did-resolution/>

-   **Read**: - DID Resolution, resolve and retrieving a [DID payload](#did-document)
-   **Create** - create a DID with payload
-   **Update** - update a DID payload
-   **Deactivate** - in practice we render the DID payload useless

### Types of DIDs

> In PRISM, all DIDs follow "did:prism:CONTENT"

-   **Long Form**: not stored in the blockchain, document payload is stored in the DID itself

    -   did:prism:09sdf8g098sdf0g98sd0f9g80sd9f8g09sd8fg:vf089vdggokajvf+0dvkdnvfllkhosdkfh

-   **Canonical**: stored in the Cardano blockchain

    -   did:prism:09sdf8g098sdf0g98sd0f9g80sd9f8g09sd8fg

#### Things to keep in mind

-   **If you want to issue a certificate,** the Issuer's DID issue credentials need to be stored on the blockchain.
-   **If you want to revoke a credential,** the Issuer's DID revoke credential need to be stored on the blockchain.
-   **If you are the recipient/Holder of a credential,** then it's not necessarily required that your Holder DID is stored on the blockchain.
-   **If we read an Long Form DID**, for Verifiers, if we find the DID in the blockchain, that is the source of truth and overrides the document payload that is stored in the DID itself.

### DID document

This is a JSON-based document. Contains:

-   Id

-   publicKeys

    -   id

    -   usage - one of master, issuing, revocation

    -   ecKeyData
