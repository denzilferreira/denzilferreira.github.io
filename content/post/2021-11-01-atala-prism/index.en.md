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

A globally unique URI. It can identify a person, organization, thing, data model, abstract entity. A DID-Controller can cryptographically prove being in control. It is a spec from WWW Consortium at https://www.w3.org/TR/did-core. In Prism, DIDs are managed by Cardano's blockchain.

An example DID follows this structure:

> scheme:DID Method:DID method-specific identifier

e.g.,
did:example:123456789abcdefghi

### Verified Credentials

Can be *issued by* an **Issuer**, identified by a DID. Can be *issued to* a **Holder**, the subject, identified by a DID. Can be passed to and verified by a **Verifier** and it is specified by WWWC: https://w3c.github.io/vc-data-model

e.g., a university (Issuer) provides to a student (Holder) a degree certificate (Credential). This credential can be verified (using a Verifier) by an employer.