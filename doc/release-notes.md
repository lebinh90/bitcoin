(note: this is a temporary file, to be added-to by anybody, and moved to
release-notes at release time)

Bitcoin Core version *version* is now available from:

  <https://bitcoincore.org/bin/bitcoin-core-*version*/>

This is a new major version release, including new features, various bugfixes
and performance improvements, as well as updated translations.

Please report bugs using the issue tracker at GitHub:

  <https://github.com/bitcoin/bitcoin/issues>

To receive security and update notifications, please subscribe to:

  <https://bitcoincore.org/en/list/announcements/join/>

How to Upgrade
==============

If you are running an older version, shut it down. Wait until it has completely
shut down (which might take a few minutes for older versions), then run the
installer (on Windows) or just copy over `/Applications/Bitcoin-Qt` (on Mac)
or `bitcoind`/`bitcoin-qt` (on Linux).

The first time you run version 0.15.0, your chainstate database will be converted to a
new format, which will take anywhere from a few minutes to half an hour,
depending on the speed of your machine.

Note that the block database format also changed in version 0.8.0 and there is no
automatic upgrade code from before version 0.8 to version 0.15.0. Upgrading
directly from 0.7.x and earlier without redownloading the blockchain is not supported.
However, as usual, old wallet versions are still supported.

Downgrading warning
-------------------

The chainstate database for this release is not compatible with previous
releases, so if you run 0.15 and then decide to switch back to any
older version, you will need to run the old release with the `-reindex-chainstate`
option to rebuild the chainstate data structures in the old format.

If your node has pruning enabled, this will entail re-downloading and
processing the entire blockchain.

Compatibility
==============

Bitcoin Core is extensively tested on multiple operating systems using
the Linux kernel, macOS 10.8+, and Windows 7 and newer (Windows XP is not supported).

Bitcoin Core should also work on most other Unix-like systems but is not
frequently tested on them.

Notable changes
===============

RPC changes
------------

### Low-level changes

- The `createrawtransaction` RPC will now accept an array or dictionary (kept for compatibility) for the `outputs` parameter. This means the order of transaction outputs can be specified by the client.
- The `fundrawtransaction` RPC will reject the previously deprecated `reserveChangeKey` option.
- Wallet `getnewaddress` and `addmultisigaddress` RPC `account` named
  parameters have been renamed to `label` with no change in behavior.
- Wallet `getlabeladdress`, `getreceivedbylabel`, `listreceivedbylabel`, and
  `setlabel` RPCs have been added to replace `getaccountaddress`,
  `getreceivedbyaccount`, `listreceivedbyaccount`, and `setaccount` RPCs,
  which are now deprecated. There is no change in behavior between the
  new RPCs and deprecated RPCs.
- Wallet `listreceivedbylabel`, `listreceivedbyaccount` and `listunspent` RPCs
  add `label` fields to returned JSON objects that previously only had
  `account` fields.

External wallet files
---------------------

The `-wallet=<path>` option now accepts full paths instead of requiring wallets
to be located in the -walletdir directory.

Newly created wallet format
---------------------------

If `-wallet=<path>` is specified with a path that does not exist, it will now
create a wallet directory at the specified location (containing a wallet.dat
data file, a db.log file, and database/log.?????????? files) instead of just
creating a data file at the path and storing log files in the parent
directory. This should make backing up wallets more straightforward than
before because the specified wallet path can just be directly archived without
having to look in the parent directory for transaction log files.

For backwards compatibility, wallet paths that are names of existing data files
in the `-walletdir` directory will continue to be accepted and interpreted the
same as before.

Low-level RPC changes
---------------------

- When bitcoin is not started with any `-wallet=<path>` options, the name of
  the default wallet returned by `getwalletinfo` and `listwallets` RPCs is
  now the empty string `""` instead of `"wallet.dat"`. If bitcoin is started
  with any `-wallet=<path>` options, there is no change in behavior, and the
  name of any wallet is just its `<path>` string.

### Logging

- The log timestamp format is now ISO 8601 (e.g. "2018-02-28T12:34:56Z").

Credits
=======

Thanks to everyone who directly contributed to this release:


As well as everyone that helped translating on [Transifex](https://www.transifex.com/projects/p/bitcoin/).
