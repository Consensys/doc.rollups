---
description: Manager and engine configuration files
---

# Configuration files

Configuration files are used to configure the manager and engine components of ConsenSys Rollups. The files are
TOML formatted, and each component requires its own configuration file.

## Manager configuration file

The manager receives transactions and communicates with the engine. The [configuration file options](../../Reference/Configuration-File.md#manager)
allow you to specify the address of the engine and manager APIs.

{!global/Manager-Configuration-File.md!}

## Engine configuration file

The engine builds a batch of transactions and sends it to the blockchain. The
[configuration file options](../../Reference/Configuration-File.md#engine) allow you to configure the rollup details,
and access the blockchain.

{!global/Engine-Configuration-File.md!}
