# Dev Kit Development Environment

## Intro

This project is for developers.  It's purpose is to help them set up a local node where they can work on the Block Explorer and API.  Some instructions may be assuming you're on a MacBook.

## Instructions

* Clone the following into directories next to this one:
    * [meowcoincore-node](https://github.com/meowcoinDevKit/meowcoincore-node)
    * [insight-api](https://github.com/meowcoinDevKit/insight-api)
    * [insight-ui](https://github.com/meowcoinDevKit/insight-ui)

    Your directory structure should now look like this:
    ```bash
    ├── devnode
    ├── insight-api
    ├── insight-ui
    └── meowcoincore-node
    ```

* Go into each of the three new directories and run `npm install`.

* Install [meowcoin](https://meowcoin.org/wallet/).

* Copy `meowcoincore-node.json.template` to `meowcoincore-node.json` and fill in the missing configuration:
    - `servicesConfig/meowcoind/exec`: set to the absolute path to the `meowcoind` executible you just installed.
    - `servicesConfig/meowcoind/datadir`: set to the absulute path to your `devnode/data` directory.

* Copy `meowcoin.conf.template` to `data/meowcoin.conf`.

* Install and set up MongoDB for stats support
    - `brew tap mongodb/brew`
    - `brew install mongodb-community`
    - `brew services start mongodb-community`
    - `mongo`
        - `>use meowcoin-api-regtest`
        - `>db.createUser( { user: "test", pwd: "test1234", roles: [ "readWrite" ] } )`
        - `>exit`

* Make sure you're in the `devnode` directory and run `bin/start`.  You should see some logging scroll by as the node loads up.  When it's ready try out the [Block Explorer](http://localhost:3001/blocks) and the [API](http://localhost:3001/api/status?q=getInfo).

## Troubleshooting

Some quirks we've come across with helpful hints that might help.

* `Error: Cannot find module '../build/Release/zmq.node'`:
    - Try running `npm install zeromq` from the `meowcoin-node` directory.

## Extras

* Test Data
    - Run `ps ax | grep meowcoind`.
    - Copy out the entire command (with --conf, --datadir and --regtest flags).
    - Replace "meowcoind" with "meowcoin-cli"
    - Bind it to an alias e.g. `alias MEWC="/../meowcoin-cli --conf=..."`
    - Use your new command to activate assets and generate test transactions:
        - `MEWC getblockchaininfo`
        - `MEWC generate 500`
        - `MEWC issue TEST 1000`
        - `MEWC generate 1`
        - `MEWC listmyassets`