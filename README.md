# Bitg webwallet backend


This is the Bitg webwallet backend.


## Install bitcoingreen node, start node and rpc provider 

### Setup dependencies and download release of bitcoingreen node

Build doc will be found here:

    https://github.com/bitcoingreen/bitcoingreen/releases

Run the following commands:

    add-apt-repository ppa:bitcoin/bitcoin
    apt-get update
    wget https://github.com/bitcoingreen/bitcoingreen/releases/download/v1.3.0/bitcoingreen-1.3.0-x86_64-linux-gnu.tar.gz
    tar -zxvf bitcoingreen-1.3.0-x86_64-linux-gnu.tar.gz

### Node config

Config file can be found here:

    ~/.bitcoingreen/bitcoingreen.conf

Config should be following:

    rpcuser=user
    rpcpassword=PASSWORD
    rpcallowip=ALLOWEDIP_RANGE
    listen=1
    server=1
    daemon=1
    logtimestamps=1
    maxconnections=1024
    port=9333

This will use default rpc port(9332).

### Start bitcoingreen node

Start node:

    cd `bitcoingreen bin directory`
    ./bitcoingreend

Check node status:

    ./bitcoingreen-cli getinfo

Check rpc provider

    curl --user user --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getinfo", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:9332/


## Run backend service

Setup nodejs. To run backend server, use the following:

    sudo apt-get install build-essential
    npm install
    sudo npm install -g pm2
    pm2 start pm2.json

Run backend in production mode

    pm2 start pm2.json --env production

Run backend in development mode

    pm2 start pm2.json --env development
    or
    pm2 start pm2.json

Then, whenever you need to restart the snowball backend, you can run:

    pm2 restart bitg-wallet-backend



To view more detailed documentation regarding the API, check out the [API documentation](https://github.com/norestlabs/bitg-webwallet-backend/wiki).


This application integrates the following services & APIs:
* [bitcoingreen node rpc](https://github.com/bitcoingreen/) - RPC documentation is not ready yet.

### Adding DNS or SSL

If you want to add DNS or SSL through Nginx and certbot, you can follow the configuration steps [here](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-18-04) on ubuntu 18.04+.

Once it is setup, make sure to add a proxy_pass rule instead of serving the files on the filesystem:

```
location / {
    proxy_pass http://127.0.0.1:8080/;
}   

```
You can use whichever port is defined in the `config` folder of this application, but by default it runs on port 8080. If SSL is enabled, make sure the rule that you update with a `proxy_pass` is a secure one.
