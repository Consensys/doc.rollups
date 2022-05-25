---
description: Manager and engine configuration files
---

# Use configuration files

Configuration files are used to configure the manager and engine components of ConsenSys Rollups. The files are
TOML formatted, and each component requires its own configuration file.

## Manager configuration file

The manager receives transactions and communicates with the engine. The [configuration file options](../../Reference/Configuration-File.md#manager)
allow you to specify the address of the engine and manager APIs.

{!global/Manager-Configuration-File.md!}

### Override configuration options

You can override any manager configuration file options by:

- Specifying multiple configuration files on the command line.
  If the same configuration options are specified across multiple files, the values in the file specified latest on the
  command line are used.
  For example, if you specify `config.toml overrides1.toml overrides2.toml` on the command line,
  the values in `overrides2.toml` override the values in `overrides1.toml`, which override the values in `config.toml`.

- Using the `-Dconfig.override` command line option prefix.
  For example, to override the [Kafka brokers](../../Reference/Configuration-File.md#brokers) option with the value
  `localhost:9092`, specify `-Dconfig.override.kafka.brokers=localhost:9092` on the command line.
  This option overrides values specified in configuration files.

## Engine configuration file

The engine builds a batch of transactions and sends it to the blockchain. The
[configuration file options](../../Reference/Configuration-File.md#engine) allow you to configure the rollup details,
and access the blockchain.

{!global/Engine-Configuration-File.md!}
