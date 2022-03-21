---
Description: Rollups SDK
---

# Rollups SDK

The Rollups SDK library provides access to specific APIs and features.

View the [Rollups SDK documentation](https://consensys.github.io/sumo/) for more information about the available APIs and features.

## Prerequisites

* MacOS or Linux based system (the SDK is not supported on Windows)
* [Docker](https://docs.docker.com/get-docker/)
* [Java JDK](https://www.oracle.com/java/technologies/downloads/) (11 or greater)

## Configuration

Configure Docker to use the ConsenSys [registry](https://help.cloudsmith.io/docs/docker-registry#setup).
If you need access to the registry, contact us on the [ConsenSys Rollups Discord channel](https://discord.gg/9mCVSY6).

Ensure the [Gradle build file](https://github.com/ConsenSys/sumo/blob/main/manager/web3j-sdk-examples/build.gradle)
contains the `https://dl.cloudsmith.io/public/consensys/maven/maven/` repository
and the `net.consensys:web3j-sumo:0.4.0` dependency.

## Example

[This example](https://github.com/ConsenSys/sumo/blob/main/manager/web3j-sdk-examples/src/main/java/net/consensys/sumo/web3j/sdk/example/Web3jSdkExample.java)
demonstrates how to use the SDK by performing the following actions:

* Create accounts
* Retrieve account IDs
* Request money order creation
* Redeem money order
