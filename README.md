Tutorial - Install a mining pool on Ubuntu Server 18.04
Install a mining pool on Ubuntu Server 18.04 with the following tutorial.

Update your Ubuntu server with the following command:

sudo apt-get update && sudo apt-get upgrade -y

Install the required dependencies with the following command:

sudo apt-get install build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils python3 libboost-system-dev libboost-filesystem-dev libboost-chrono-dev libboost-test-dev libboost-thread-dev libboost-all-dev libboost-program-options-dev -y

Install the additional dependencies with the following command:

sudo apt-get install libminiupnpc-dev libzmq3-dev libprotobuf-dev protobuf-compiler unzip software-properties-common redis-server npm git nano cmake screen -y

Install the repository ppa:bitcoin/bitcoin with the following command:

sudo add-apt-repository ppa:bitcoin/bitcoin

Confirm the installation of the repository by pressing on the enter key. enter

Install Berkeley DB with the following command:

sudo apt-get update && sudo apt-get install libdb4.8-dev libdb4.8++-dev -y

Download the Linux daemon for your wallet with the following command:

wget "https://github.com/bitcoinrand/bitcoinrand/releases/download/V1.0.0/bitcoinrand-daemon-linux.tar.gz" -O bitcoinrand-daemon-linux.tar.gz

Extract the tar file with the following command:

tar -xzvf bitcoinrand-daemon-linux.tar.gz

Download the Linux tools for your wallet with the following command:

wget "https://github.com/bitcoinrand/bitcoinrand/releases/download/V1.0.0/bitcoinrand-daemon-linux.tar.gz" -O bitcoinrand-qt-linux.tar.gz

Extract the tar file with the following command:

tar -xzvf bitcoinrand-qt-linux.tar.gz

Type the following command to install the daemon and tools for your wallet:

sudo mv bitcoinrandd bitcoinrand-cli bitcoinrand-tx /usr/bin/

Type the following command to open your home directory:

cd $HOME

Create the data directory for your coin with the following command:

mkdir $HOME/.bitcoinrand

Open nano.

nano $HOME/.bitcoinrand/bitcoinrand.conf -t

Paste the following into nano.

rpcuser=rpc_bitcoinrand
rpcpassword=dR2oBQ3K1zYMZQtJFZeAerhWxaJ5Lqeq9J2
rpcallowip=127.0.0.1
listen=1
server=1
txindex=1
daemon=1
paytxfee=0.0002
deprecatedrpc=accounts
addresstype=legacy
addnode=node1.walletbuilders.com

Save the file with the keyboard shortcut ctrl + x.

Type the following command to start your daemon:

bitcoinrandd

Type the following command to show the receiving address of your daemon:

bitcoinrand-cli getaccountaddress ""

Example output.

4UyrFQrAoNQMEMqNhZTareRmjeU68bLiop

Type the following commands to install NOMP:

git clone https://github.com/walletbuilders/node-open-mining-portal.git nomp
cd nomp
npm update

Type the following command to create the file settings.json:

cp config_example.json config.json

Open nano.

nano config.json -t

Modify the following values in the file config.json.

host - Change the value “0.0.0.0” with the IPv4 address of your server.

stratumHost - Change the value “cryppit.com” with the IPv4 address of your server.

Save the file with the keyboard shortcut ctrl + x.

Type the following commands to create the config file for your coin:

cd coins
cp bitcoin.json bitcoinrand.json

Open nano.

nano bitcoinrand.json -t

Modify the following values in the file bitcoinrand.json.

name - “Bitcoin” -> “Bitcoinrand”.

symbol - “BTC” -> “BZAR”.

Save the file with the keyboard shortcut ctrl + x.

Type the following commands to create the config file for your pool:

cd ../pool_configs
cp litecoin_example.json bitcoinrand_pool.json

Open nano.

nano bitcoinrand_pool.json -t

Modify the following values in the file bitcoinrand.json.

enabled - “false” -> “true”.

coin - “litecoin.json” -> “bitcoinrand.json”.

address - “n4jSe18kZMCdGcZqaYprShXW6EH1wivUK1” -> Enter the receiving address from the RPC command “getaccountaddress ""”.

rewardRecipients - Remove all recipients.

minimumPayment - “70” -> “0.01”.

"paymentProcessing" -> port - “19332” -> “19451”.

"paymentProcessing" -> user - “testuser” -> “rpc_bitcoinrand”.

"paymentProcessing" -> password - “testpass” -> “dR2oBQ3K1zYMZQtJFZeAerhWxaJ5Lqeq9J2”.

"daemons" -> port - “19332” -> “19451”.

"daemons" -> user - “testuser” -> “rpc_bitcoinrand”.

"daemons" -> password - “testpass” -> “dR2oBQ3K1zYMZQtJFZeAerhWxaJ5Lqeq9J2”.

p2p - “true” -> “false”.

Save the file with the keyboard shortcut ctrl + x.

Type the following command to open a screen session:

screen

Type the following commands to start your mining pool:

cd $HOME/nomp
sudo node init.js

Press the keyboard shortcut ctrl + a + d to disconnect from your screen session.


Instructions to mine with your mining pool.

Open your wallet.

Go to Help -> Debug Window.
Click on the tab Console.
This is the console where you execute RPC commands.

Type the following command, to create a legacy receiving address for your miner:

getnewaddress "" "legacy"

Example output.

4UyrFQrAoNQMEMqNhZTareRmjeU68bLiop

Click here to download cpuminer and extract the zip file.

Open "Run" with the keyboard shortcut winkey + r.

Enter the following text behind "Open": notepad

Press on the button "OK".

Modify the following values in the following text.

minerd -a sha256d -o stratum+tcp://203.0.113.53:3008 -u 4UyrFQrAoNQMEMqNhZTareRmjeU68bLiop -p anything

203.0.113.53 - “203.0.113.53” -> IPv4 address of your VPS.

4UyrFQrAoNQMEMqNhZTareRmjeU68bLiop - “TP56yqPtRTkse49B96sHzo2B6v48MV24vP” -> Receiving address from the RPC command “getnewaddress” for your miner.

Paste the modified text into notepad.

Click on the menu item "File" -> "Save As...".

The Open dialog box will appear, click on "Save as type" and select the option "All Files (*.*)".

Enter the following text behind "File name": mine.bat

Click on the menu bar, open the directory where you extracted pooler-cpuminer-2.5.0-win64.zip and press on the enter key. enter

Press on the button "Save".

Execute mine.bat to mine with your mining pool.

# s-nomp: Some New Open Mining Portal

> *NOTE*:
> We're working on putting together an "official" s-nomp which can be supported by many coins and pools instead of so many running their own flavors. More to come!

This is a Equihash mining pool based off Node Open Mining Portal.

#### Production Usage Notice
This is beta software. All of the following are things that can change and break an existing s-nomp setup: functionality of any feature, structure of configuration files and structure of redis data. If you use this software in production then *DO NOT* pull new code straight into production usage because it can and often will break your setup and require you to tweak things like config files or redis data. *Only tagged releases are considered stable.*

#### Paid Solution
Usage of this software requires abilities with sysadmin, database admin, coin daemons, and sometimes a bit of programming. Running a production pool can literally be more work than a full-time job. 

### Community / Support

Please join our Discord to follow development. Any support questions can be answered here quickly as well.

https://discord.gg/4mVaTsH

# Usage

#### Requirements
* Coin daemon(s) (find the coin's repo and build latest version from source)
* [Node.js](http://nodejs.org/) v8.11 ([follow these installation instructions](https://github.com/nodejs/node))
* [Redis](http://redis.io/) key-value store v2.6+ ([follow these instructions](http://redis.io/topics/quickstart))

##### Seriously
These are legitimate requirements. If you use old versions of Node.js or Redis that may come with your system package manager then you will have problems. Follow the linked instructions to get the last stable versions.

[**Redis security warning**](http://redis.io/topics/security): be sure firewall access to redis - an easy way is to
include `bind 127.0.0.1` in your `redis.conf` file. Also it's a good idea to learn about and understand software that
you are using - a good place to start with redis is [data persistence](http://redis.io/topics/persistence).

#### 0) Setting up coin daemon
Follow the build/install instructions for your coin daemon. Your coin.conf file should end up looking something like this:
```
daemon=1
rpcuser=zclassicrpc
rpcpassword=securepassword
rpcport=8232
```
For redundancy, its recommended to have at least two daemon instances running in case one drops out-of-sync or offline,
all instances will be polled for block/transaction updates and be used for submitting blocks. Creating a backup daemon
involves spawning a daemon using the `-datadir=/backup` command-line argument which creates a new daemon instance with
it's own config directory and coin.conf file. Learn about the daemon, how to use it and how it works if you want to be
a good pool operator. For starters be sure to read:
   * https://en.bitcoin.it/wiki/Running_bitcoind
   * https://en.bitcoin.it/wiki/Data_directory
   * https://en.bitcoin.it/wiki/Original_Bitcoin_client/API_Calls_list
   * https://en.bitcoin.it/wiki/Difficulty


#### 1) Downloading & Installing

Clone the repository and run `npm update` for all the dependencies to be installed:

```bash
sudo apt-get install build-essential libsodium-dev npm libboost-all-dev
sudo npm install n -g
sudo n stable
git clone https://github.com/s-nomp/s-nomp.git s-nomp
cd s-nomp
npm update
npm install
```

##### Pool config
Take a look at the example json file inside the `pool_configs` directory. Rename it to `zclassic.json` and change the
example fields to fit your setup.

```
Please Note that: 1 Difficulty is actually 8192, 0.125 Difficulty is actually 1024.

Whenever a miner submits a share, the pool counts the difficulty and keeps adding them as the shares. 

ie: Miner 1 mines at 0.1 difficulty and finds 10 shares, the pool sees it as 1 share. Miner 2 mines at 0.5 difficulty and finds 5 shares, the pool sees it as 2.5 shares. 
```


##### [Optional, recommended] Setting up blocknotify
1. In `config.json` set the port and password for `blockNotifyListener`
2. In your daemon conf file set the `blocknotify` command to use:
```
node [path to cli.js] [coin name in config] [block hash symbol]
```
Example: inside `zclassic.conf` add the line
```
blocknotify=node /home/user/s-nomp/scripts/cli.js blocknotify zclassic %s
```

Alternatively, you can use a more efficient block notify script written in pure C. Build and usage instructions
are commented in [scripts/blocknotify.c](scripts/blocknotify.c).


#### 3) Start the portal

```bash
npm start
```

###### Optional enhancements for your awesome new mining pool server setup:
* Use something like [forever](https://github.com/nodejitsu/forever) to keep the node script running
in case the master process crashes. 
* Use something like [redis-commander](https://github.com/joeferner/redis-commander) to have a nice GUI
for exploring your redis database.
* Use something like [logrotator](http://www.thegeekstuff.com/2010/07/logrotate-examples/) to rotate log 
output from s-nomp.
* Use [New Relic](http://newrelic.com/) to monitor your s-nomp instance and server performance.


#### Upgrading s-nomp
When updating s-nomp to the latest code its important to not only `git pull` the latest from this repo, but to also update
the `node-stratum-pool` and `node-multi-hashing` modules, and any config files that may have been changed.
* Inside your s-nomp directory (where the init.js script is) do `git pull` to get the latest s-nomp code.
* Remove the dependenices by deleting the `node_modules` directory with `rm -r node_modules`.
* Run `npm update` to force updating/reinstalling of the dependencies.
* Compare your `config.json` and `pool_configs/coin.json` configurations to the latest example ones in this repo or the ones in the setup instructions where each config field is explained. <b>You may need to modify or add any new changes.</b>


Credits
-------
### s-nomp
* [egyptianbman](https://github.com/egyptianbman)
* [nettts](https://github.com/nettts)
* [potato](https://github.com/zzzpotato)
* You belong here. Join us!

### z-nomp
* [Joshua Yabut / movrcx](https://github.com/joshuayabut)
* [Aayan L / anarch3](https://github.com/aayanl)
* [hellcatz](https://github.com/hellcatz)

### NOMP
* [Matthew Little / zone117x](https://github.com/zone117x) - developer of NOMP
* [Jerry Brady / mintyfresh68](https://github.com/bluecircle) - got coin-switching fully working and developed proxy-per-algo feature
* [Tony Dobbs](http://anthonydobbs.com) - designs for front-end and created the NOMP logo
* [LucasJones](//github.com/LucasJones) - got p2p block notify working and implemented additional hashing algos
* [vekexasia](//github.com/vekexasia) - co-developer & great tester
* [TheSeven](//github.com/TheSeven) - answering an absurd amount of my questions and being a very helpful gentleman
* [UdjinM6](//github.com/UdjinM6) - helped implement fee withdrawal in payment processing
* [Alex Petrov / sysmanalex](https://github.com/sysmanalex) - contributed the pure C block notify script
* [svirusxxx](//github.com/svirusxxx) - sponsored development of MPOS mode
* [icecube45](//github.com/icecube45) - helping out with the repo wiki
* [Fcases](//github.com/Fcases) - ordered me a pizza <3
* Those that contributed to [node-stratum-pool](//github.com/zone117x/node-stratum-pool#credits)

License
-------
Released under the MIT License. See LICENSE file.
