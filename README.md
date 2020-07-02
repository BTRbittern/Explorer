Explorer Bittern
================

An open source block explorer written in node.js.

### Requires

*  node.js >= 8.17.0 (12.14.0 is advised for updated dependencies)
*  mongodb 4.2.x
*  *coind

Use the following tutorial to setup a block explorer.

Make sure that you have the following requirement.

- A server or VPS with Ubuntu Server 18.04 installed

Update your Ubuntu machine.

sudo apt-get update
sudo apt-get upgrade

Install the required dependencies.

sudo apt-get install build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils python3 libboost-system-dev libboost-filesystem-dev libboost-chrono-dev libboost-test-dev libboost-thread-dev libboost-all-dev libboost-program-options-dev
sudo apt-get install libminiupnpc-dev libzmq3-dev libprotobuf-dev protobuf-compiler unzip software-properties-common libkrb5-dev mongodb nodejs npm git nano screen

Install Berkeley DB.

sudo add-apt-repository ppa:bitcoin/bitcoin
sudo apt-get update
sudo apt-get install libdb4.8-dev libdb4.8++-dev

Go to your home directory.

cd $HOME

Download the daemon and tools from Bittern. 

wget "https://github.com/BTRbittern/BITTERN/releases/download/V1.0.0.0/bittern-ubuntu18-cli-d.tar.gz" -O bittern-ubuntu18-cli-d.tar.gz

Extract the tar files.

tar -xzvf bittern-ubuntu18-cli-d.tar.gz

Install the daemon and tools.

sudo mv bitternd bittern-cli /usr/bin/

Create the config file.

mkdir $HOME/.bittern
nano $HOME/.bittern/bittern.conf

Paste the following lines in bittern.conf.

rpcuser=rpc_bittern
rpcpassword=CNsahKWiZpMzFxdvb9R7vKuAvYWRXqr24EfCkdjz
rpcallowip=127.0.0.1
listen=1
server=1
txindex=1
daemon=1
addnode=80.78.248.132
addnode=194.58.97.180
addnode=194.58.97.98

Start your daemon with the following command.

bitternd

Open mongodb.

mongo

Create the database “explorerdb”.

use explorerdb

Note: replace the value “414uq3EhKDNX76f7DZIMszvHrDMytCnzFevRgtAv” with a unique password.
Create the database user.

db.createUser( { user: "iquidus", pwd: "414uq3EhKDNX76f7DZIMszvHrDMytCnzFevRgtAv", roles: [ "readWrite" ] } )

Close mongodb.

exit

Go to your home directory.

cd $HOME

Download iquidus explorer.

git clone https://github.com/BTRbittern/Explorer explorer

Install iquidus explorer.

cd explorer && npm install --production

Create the settings file.

cp ./settings.json.template ./settings.json

Open the settings file.

nano settings.json

Change the marked values.

title - Change the value “BTR - Bittern” with the name of your coin.

address - Change the value “127.0.0.1” with the IP address of your server.

coin - Change the value “Bittern” with the name of your coin.

symbol - Change the value “BTR” with the abbreviation of your coin.

password - Change the value “3xp!0reR” with the mongodb password.

user - Change the value “bitternrpc” with the RPC username of your coin.

pass - Change the value “123gfjk3R3pCCVjHtbRde2s5kzdf233sa” with the RPC password of your coin.

confirmations - Change the value “40” with the transaction confirmations of your coin.

api - Change the value “true” to “false”.

markets - Change the value “true” to “false”.

twitter - Change the value “true” to “false”.


Original settings.json.


/*
  This file must be valid JSON. But comments are allowed

  Please edit settings.json, not settings.json.template
*/
{
  // name your instance!
  "title": "BTR - Bittern",

  "address": "127.0.0.1:3001",

  // coin name
  "coin": "Bittern",

  // coin symbol
  "symbol": "BTR",

...

  // database settings (MongoDB)
  "dbsettings": {
    "user": "iquidus",
    "password": "3xp!0reR",
    "database": "explorerdb",
    "address": "localhost",
    "port": 27017
  },

  //update script settings
  "update_timeout": 10,
  "check_timeout": 250,

  // wallet settings
  "wallet": {
    "host": "localhost",
    "port": 9332,
    "user": "bitternrpc",
    "pass": "123gfjk3R3pCCVjHtbRde2s5kzdf233sa"
  },

  // confirmations
  "confirmations": 40,

  // language settings
  "locale": "locale/en.json",

  // menu settings
  "display": {
    "api": true,
    "markets": true,
    "richlist": true,
    "twitter": true,
    "facebook": false,
    "googleplus": false,
    "youtube": false,
    "search": true,
    "movement": true,
    "network": true
  },

Save the settings file.


----------optional----------

Replace the default logo with your own logo.

Note: The file must be a PNG file with a width and height of 128 px.

Overwrite the file “logo.png” inside the path “explorer/public/images/”.


Replace the default favicon with your own favicon.

Note 1: The file must be an ICO file with a width and height of 16 px.
Note 2: You can convert your PNG file to an ICO file on Online ICO converter.

Overwrite the file “favicon.ico” inside the path “explorer/public/”.

----------optional----------


Get the path to your block explorer.

cd $HOME/explorer
pwd

Example output

/root/explorer

Open crontab

crontab -e

Change the path “/root/explorer” with the path to your block explorer.

Paste the following lines to the bottom of the crontab.

*/1 * * * * cd /root/explorer && /usr/bin/nodejs scripts/sync.js index update > /dev/null 2>&1
*/5 * * * * cd /root/explorer && /usr/bin/nodejs scripts/peers.js > /dev/null 2>&1

Save the crontab.

A screen session will remain open when the SSH connection is disconnected.
You can disconnect from a screen session using the keyboard combination ctrl + a + d.

Start a screen session.

screen

Start your block explorer.

cd $HOME/explorer
npm start

You can access the block explorer on the IP of your server on port 3001.

E.G. http://127.0.0.1:3001

### License

Copyright (c) 2015, Iquidus Technology  
Copyright (c) 2015, Luke Williams  
All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

* Redistributions of source code must retain the above copyright notice, this
  list of conditions and the following disclaimer.

* Redistributions in binary form must reproduce the above copyright notice,
  this list of conditions and the following disclaimer in the documentation
  and/or other materials provided with the distribution.

* Neither the name of Iquidus Technology nor the names of its
  contributors may be used to endorse or promote products derived from
  this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE
FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR
SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY,
OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
