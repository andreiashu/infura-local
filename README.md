Mainnet fork of infura on localhost with self signed certificate

## Setup
```
cp .env.example .env
# edit .env, add your alchemi.io key

# map mainnet infura to localhost
echo '127.0.0.1 mainnet.infura.io' | sudo tee -a /etc/hosts

# generate self signed certs
./makecerts.sh

# start the docker containers - append `-d` if you want it sent to background
# ganache-cli will take a bit of time to start but once started it should list
# seed phrase, accounts and private keys
docker-compose up
```

Test that requests to infura work
```
curl -i -k 'https://mainnet.infura.io/v3/bf9c8c1758124c208066454d352c84c4' \
  -H 'authority: mainnet.infura.io' \
  -H 'pragma: no-cache' \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' \
  -H 'accept: */*' \
  -H 'origin: https://zapper.fi' \
  -H 'sec-fetch-site: cross-site' \
  -H 'sec-fetch-mode: cors' \
  -H 'sec-fetch-dest: empty' \
  -H 'referer: https://zapper.fi/' \
  -H 'accept-language: en-US,en;q=0.9' \
  --data-raw '{"method":"eth_chainId","params":[],"id":42,"jsonrpc":"2.0"}' \
  --compressed

HTTP/1.1 200 OK
Access-Control-Allow-Credentials: true
Access-Control-Allow-Origin: https://zapper.fi
Content-Type: application/json
Date: Tue, 02 Feb 2021 13:49:38 GMT
Content-Length: 40

{"id":42,"jsonrpc":"2.0","result":"0x1"}
```

Open https://mainnet.infura.io/ in Brave browser (you should see 
* Your connection is not private / NET::ERR_CERT_AUTHORITY_INVALID warning)
* Click Advanced -> Proceed


