# Inery Task 4 : Json RPC Sample
A Sample code to call JSON RPC on Inery Blockchain

## Persiapan
##### Remove Previous Nodejs
```
sudo apt-get remove nodejs
```

##### Install Curl

```
sudo apt-get install curl
```

##### Install NodeJS

```
curl -fsSL https://deb.nodesource.com/setup_19.x | sudo -E bash - &&\
sudo apt-get install -y nodejs
```

##### Install npm
```
sudo apt install npm
```
_____________________

## Installation

1. Clone the repo

   ```
   git clone https://github.com/amustofalia/inery-testnet-faucet-tasks.git
   ```

2. Change directory to cloned repo

   ```
   cd inery-testnet-faucet-tasks/amustofa
   ```

3. Install NPM packages

   ```
   npm install
   ```

4. Copy `.env-sample` and rename it to `.env`

   ```
   cp .env-sample .env
   ```

5. Edit ```.env``` file with your information

   ```
   nano .env
   ```


## Usage

Run RPC Example

```
npm run rpc-example
```

#### Successful Example
<p align="center">
  <img src="https://github.com/SaujanaOK/Images/blob/main/berhasil.png">
</p>

____________________________

#Lanjutan Task 4
```
cd ~/inery-testnet-faucet-tasks/
mkdir $IneryAccname
```
```
cd ~/inery-testnet-faucet-tasks/$IneryAccname
```

### Membuat RPC Solution
```
sudo tee $HOME/inery-testnet-faucet-tasks/$IneryAccname/solution.mjs >/dev/null <<EOF
import { Api, JsonRpc, RpcError, JsSignatureProvider } from '../dist/index.js'
import * as dotenv from 'dotenv' // see https://github.com/motdotla/dotenv#how-do-i-use-dotenv-with-import
dotenv.config()

// our Node URL, when we first setup our node, inery has created our RPC in port :8888
// check it on your node, /inery-node/inery.setup/tools/config.json HTTP_ADDRESS key
const url = process.env.NODE_URL

const json_rpc = new JsonRpc(url) // create new JsonRPC using our node url
const private_key = process.env.PRIVATE_KEY; // private key

const account = process.env.INERY_ACCOUNT // Inery Smart Contract Account to Call
const actor = process.env.INERY_ACCOUNT // The Signer, should match with your provided Private Key
const signature  = new JsSignatureProvider([private_key]); // creating Signer from private key

// calling API
const api = new Api({
    rpc: json_rpc,
    signatureProvider: signature
})

// A Function to create new data in our Valued Smart Contract, and call "create" function on our Smart contract
async function create(id, user, data){
    try{
        // create new transaction and sign it
        const tx = await api.transact({
            actions:[
                {
                    account,
                    name:"create",
                    authorization:[
                        {
                            actor,
                            permission:"active"
                        }
                    ],
                    data:{
                        id, user, data
                    }
                }
            ]
        },{broadcast:true,sign:true})

        console.log(tx) // output the tx to terminal, it's Json Object
        console.log(tx.processed.action_traces[0].console)
    }catch(error){
        console.log(error)
    }
}

// call RPC that we created in create function
create(5, account, "Create new Data via JSON RPC")
EOF

```

### Membuat Paket Json
```
sudo tee $HOME/inery-testnet-faucet-tasks/$IneryAccname/package.json >/dev/null <<EOF                                                
{
  "name": "$IneryAccname-rpc-transaction",
  "version": "1.0.0",
  "description": "A sample rpc transaction for inery blockchain",
  "main": "./solution.mjs",
  "scripts": {
    "solution": "node ./solution.mjs"
  },
  "author": "$IneryAccname",
  "license": "MIT",
  "repository": {
    "type": "git",
    "url": "https://github.com/$IneryAccname/inery-testnet-faucet-tasks"
  },
  "homepage": "https://github.com/$IneryAccname/inery-testnet-faucet-tasks",
  "dependencies": {
    "ineryjs": "github:inery-blockchain/ineryjs"
  }
}
EOF

```
