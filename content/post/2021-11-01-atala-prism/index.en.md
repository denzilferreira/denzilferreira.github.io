---
title: "Atala Prism Pioneers - Lecture 1 & 2"
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

-   **Read**: - DID Resolution, resolve and retrieving a [DID document](#did-document)
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
-   **If we read an Long Form DID**, for Verifiers, if we find the DID in the blockchain, that is the source of truth and overrides the document payload that is stored on the DID itself.

### DID document

In PRISM, the DID payload contains the list of public keys and their roles:

-   Id: String URI of the DID

-   publicKeys

    -   id

    -   usage: e.g., master, issuing, revocation

        -   **master**: modify, update and perform operations on the DID itself

        -   **issuing**: to create new credentials

        -   **revocation**: to revoke credentials

    -   ecKeyData: elliptic curve encrypted key

> A DID **Controller** contains the private master key that identifies as owner of this DID document. Only they can update this DID document.

**You need PRISM SDK access to work with Atala PRISM. Since I joined the cohort, I have access to it. This will be open to everyone once IOHK works out the documentation.**

## Creating a DID

To create DIDs, you first need a seed and a master key. The master key allows you to create DIDs, derived from the same seed passphrase. In this example, the derived seed (random mnemonic + "passphrase") is stored into a file, which we provided as argument to the main function. This allows us to issue a master key and then as many DID we want. In this example, we create an unpublished DID (off the blockchain - Long Form DID).

``` Kotlin
@PrismSdkInternal
fun main(args: Array<String>) {
    val seedFile = try { args[0] } catch (e: Exception) {throw Exception("expected seed file path as argument")}
    val seed = KeyDerivation.binarySeed(KeyDerivation.randomMnemonicCode(), "passphrase")
    File(seedFile).writeBytes(seed)
    println("wrote seed to file $seedFile")
    println()

    val masterKeyPair = KeyGenerator.deriveKeyFromFullPath(seed, 0, PrismKeyType.MASTER_KEY, 0)
    val unpublishedDid = PrismDid.buildLongFormFromMasterPublicKey(masterKeyPair.publicKey)

    val didCanonical = unpublishedDid.asCanonical().did
    val didLongForm = unpublishedDid.did

    println("canonical: $didCanonical")
    println("long form: $didLongForm")
    println()
}
```

## Reading a DID from blockchain

PRISM SDK uses a Node backend API to interact with the blockchain ([ppp.atalaprism.io](https://ppp.atalaprism.io)). In this example, we search for a DID using the URI.

``` Kotlin
val environment = "ppp.atalaprism.io"
val grpcOptions = GrpcOptions("https", environment, 50053)
val nodeAuthApi = NodeAuthApiImpl(grpcOptions)

@PrismSdkInternal
fun main(args: Array<String>) {
    if (args.size != 1) {
        throw Exception("expected exactly one command line argument, the DID")
    }

    val did = try { Did.fromString(args[0]) } catch (e: Exception) { throw Exception("illegal DID: ${args[0]}") }
    val prismDid = try { PrismDid.fromDid(did) } catch (e: Exception) { throw Exception("not a Prism DID: $did") }

    println("trying to retrieve document for $did")
    try {
        val model = runBlocking { nodeAuthApi.getDidDocument(prismDid) }
        println(model.publicKeys.size)
        println(model.didDataModel)
    } catch (e: Exception) {
        println("unknown prism DID")
    }
}
```

When interacting with the blockchain, the operations are asynchronous, thus we need to wait for the result from the blockchain (*runBlocking*) before continueing. The model variable contains the public keys of this DID (*publicKeys*) and if there is a document, we can retrieve it (*didDataModel*).

> **If the DID we are retrieving is Long Form**, and it's not published on the blockchain, we get the document that is embedded on the DID document payload. If published on the blockchain, we ALWAYS get the blockchain document.
>
> **If the DID we are retrieving is Canonical**, then the nodeAuthApi will only return the blockchain document if it's found.

## Publishing a DID to the blockchain

Publishing a DID in Atala PRISM blockchain can take a while because we need to wait for a few confirmations (4-6) to guarantee the blockchain has recorded your entry. To do this, a couple of utility functions: *waitUntilConfirmed* and *transactionId.*

The **waitUntilConfirmed** runs a loop every 10 seconds to check whether we have a **confirmed and applied** or **confirmed and rejected** operation on the Prism blockchain.

``` Kotlin
// Waits until an operation is confirmed by the Cardano network.
// NOTE: Confirmation doesn't necessarily mean that operation was applied.
// For example, it could be rejected because of an incorrect signature or other reasons.
@PrismSdkInternal
fun waitUntilConfirmed(nodePublicApi: NodePublicApi, operationId: AtalaOperationId) {
    var tid = ""
    var status = runBlocking {
            nodePublicApi.getOperationStatus(operationId)
    }
    while (status != AtalaOperationStatus.CONFIRMED_AND_APPLIED &&
        status != AtalaOperationStatus.CONFIRMED_AND_REJECTED
    ) {
        println("Current operation status: ${AtalaOperationStatus.asString(status)}")
        if (tid.isNullOrEmpty()) {
            tid = transactionId(operationId)
            if (!tid.isNullOrEmpty()) {
                println("Transaction id: $tid")
                println("Track the transaction in:\n- https://explorer.cardano-testnet.iohkdev.io/en/transaction?id=$tid")
            }
        }

        Thread.sleep(10000)
        status = runBlocking {
            nodePublicApi.getOperationStatus(operationId)
        }
    }
}
```

To get the Cardano transaction ID from an Atala operation ID, we use *transactionId*:

``` Kotlin
@PrismSdkInternal
fun transactionId(oid: AtalaOperationId): String {
    val node = NodeServiceCoroutine.Client(GrpcClient(grpcOptions))
    val response = runBlocking {
            node.GetOperationInfo(GetOperationInfoRequest(ByteArr(oid.value())))
        }
    return response.transactionId
}
```

Now, to publish a DID, we'll need the encoded seed file. The encoded seed file is used to restore the master key and DID that we used in [creating a DID](#creating-a-did). In this example, the main function uses two arguments, one for where the seed file is located and the second to where we keep a record of the Atala operation Ids (val *hashFile*).

> To perform an operation in a DID, you need to know the ID of the previous operation. This is to support concurrency and we can have two agents trying to modify the same DID at the same time.

``` Kotlin
@PrismSdkInternal
fun main(args: Array<String>) {
    val seedFile = try { args[0] } catch (e: Exception) { throw Exception("expected seed file path as first argument") }
    val hashFile = try { args[1] } catch (e: Exception) { throw Exception("expected hash file path as second argument") }
    val seed = File(seedFile).readBytes()
    println("read seed from file $seedFile")

    val masterKeyPair = KeyGenerator.deriveKeyFromFullPath(seed, 0, PrismKeyType.MASTER_KEY, 0)
    val unpublishedDid = PrismDid.buildLongFormFromMasterPublicKey(masterKeyPair.publicKey)

    val didCanonical = unpublishedDid.asCanonical().did
    val didLongForm = unpublishedDid.did

    println("canonical: $didCanonical")
    println("long form: $didLongForm")
    println()

    println("publishing DID...")
    var nodePayloadGenerator = NodePayloadGenerator(
            unpublishedDid,
            mapOf(PrismDid.DEFAULT_MASTER_KEY_ID to masterKeyPair.privateKey))
    val createDidInfo = nodePayloadGenerator.createDid()
    val createDidOperationId = runBlocking {
            nodeAuthApi.createDid(
                createDidInfo.payload,
                unpublishedDid,
                PrismDid.DEFAULT_MASTER_KEY_ID)
        }

    println(
        """
        - Sent a request to create a new DID to PRISM Node.
        - The transaction can take up to 10 minutes to be confirmed by the Cardano network.
        - Operation identifier: ${createDidOperationId.hexValue()}
        """.trimIndent())
    println()
    waitUntilConfirmed(nodeAuthApi, createDidOperationId)

    val status = runBlocking { nodeAuthApi.getOperationStatus(createDidOperationId) }
    require(status == AtalaOperationStatus.CONFIRMED_AND_APPLIED) {
        "expected publishing to be applied"
    }

    println("DID published")

    val hash = createDidInfo.operationHash.hexValue
    println("hash: $hash")
    File(hashFile).writeText(hash)
    println("wrote oid hash to file $hashFile")
    println()
}
```

To publish a DID, we need to create a NodePayload, which takes an unpublished DID and a map of all the private keys for the operations we want to support on the DID.

``` Kotlin
...
var nodePayloadGenerator = NodePayloadGenerator(
            unpublishedDid,
            mapOf(PrismDid.DEFAULT_MASTER_KEY_ID to masterKeyPair.privateKey))
...
```

We should always have the master key ID, otherwise no-one will be authorised to change this DID in the future. We can have the revocation and issuance private keys as well so we can use this DID to issue and revoke credentials.

## Updating a DID

We will update here the list of private keys associated with a DID. When restoring the DID from the seed file, we this time create an *issuingKeyPair* so we can use it to issue credentials as well.

``` Kotlin
@PrismSdkInternal
fun main(args: Array<String>) {
    val seedFile = try { args[0] } catch (e: Exception) { throw Exception("expected seed file path as first argument") }
    val oldHashFile = try { args[1] } catch (e: Exception) { throw Exception("expected old hash file path as second argument") }
    val newHashFile = try { args[2] } catch (e: Exception) { throw Exception("expected new hash file path as third argument") }

    val seed = File(seedFile).readBytes()
    println("read seed from file $seedFile")
    val oldHash = Sha256Digest.fromHex(File(oldHashFile).readText())
    println("read old hash from $oldHashFile: ${oldHash.hexValue}")

    val masterKeyPair = KeyGenerator.deriveKeyFromFullPath(seed, 0, PrismKeyType.MASTER_KEY, 0)
    val issuingKeyPair = KeyGenerator.deriveKeyFromFullPath(seed, 0, PrismKeyType.ISSUING_KEY, 0)
    val unpublishedDid = PrismDid.buildLongFormFromMasterPublicKey(masterKeyPair.publicKey)

    val didCanonical = unpublishedDid.asCanonical().did
    val didLongForm = unpublishedDid.did

    println("canonical: $didCanonical")
    println("long form: $didLongForm")
    println()

    println("updating DID...")
    var nodePayloadGenerator = NodePayloadGenerator(
            unpublishedDid,
            mapOf(PrismDid.DEFAULT_MASTER_KEY_ID to masterKeyPair.privateKey))
    val issuingKeyInfo = PrismKeyInformation(
            PrismDid.DEFAULT_ISSUING_KEY_ID,
            PrismKeyType.ISSUING_KEY,
            issuingKeyPair.publicKey)
    val updateDidInfo = nodePayloadGenerator.updateDid(
            previousHash = oldHash,
            masterKeyId = PrismDid.DEFAULT_MASTER_KEY_ID,
            keysToAdd = arrayOf(issuingKeyInfo))
    val updateDidOperationId = runBlocking {
            nodeAuthApi.updateDid(
                payload = updateDidInfo.payload,
                did = unpublishedDid.asCanonical(),
                masterKeyId = PrismDid.DEFAULT_MASTER_KEY_ID,
                previousOperationHash = oldHash,
                keysToAdd = arrayOf(issuingKeyInfo),
                keysToRevoke = arrayOf())
        }

    println(
        """
        - Sent a request to update the DID to PRISM Node.
        - The transaction can take up to 10 minutes to be confirmed by the Cardano network.
        - Operation identifier: ${updateDidOperationId.hexValue()}
        """.trimIndent())
    println()
    waitUntilConfirmed(nodeAuthApi, updateDidOperationId)

    val status = runBlocking { nodeAuthApi.getOperationStatus(updateDidOperationId) }
    require(status == AtalaOperationStatus.CONFIRMED_AND_APPLIED) {
        "expected updating to be applied"
    }

    println("DID updated")
    val newHash = updateDidInfo.operationHash.hexValue
    File(newHashFile).writeText(newHash)
    println("wrote new hash $newHash to file $newHashFile")
    println()
}
```

The NodePayloadGenerator will create a payload of operations on the Prism blockchain and then updateDid function will package the payload for the Node API. The Node API updateDid function can both add and revoke keys.
