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
   git clone https://github.com/inery-blockchain/ineryjs.git
   ```

2. Change directory to cloned repo

   ```
   cd ineryjs
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
  <img src="https://github.com/Herzarika/Images/blob/main/berhasil.png">
</p>
_____________________

# Lanjutan Task 4 Inery Blockchain

### 1. Set nama akun sebagai env variable dan Set PATH env

Agar tidak berulang menulis nama Akun Inery, kita perlu mengatur nama akun sebagai Variable env, silahkan ganti Nama_Akun_Inery dengan Nama Akun Inerymu

```
cd
IneryAccname=Nama_Akun_Inery
```
```
export PATH="$PATH:$HOME/inery.cdt/bin:$HOME/inery-node/inery/bin"
```

### 2. Membuat Frok Task Project

Kunjungi Halaman :
https://github.com/inery-blockchain/inery-testnet-faucet-tasks

<p align="center">
  <img src="https://github.com/Herzarika/Images/blob/main/Frok.png">
</p>

<p align="center">
  <img src="https://github.com/Herzarika/Images/blob/main/Frok2.png">
</p>

<p align="center">
  <img src="https://github.com/Herzarika/Images/blob/main/Frok3.png">
</p>

<p align="center">
  <img src="https://github.com/Herzarika/Images/blob/main/Frok4.png">
</p>

<p align="center">
  <img src="https://github.com/Herzarika/Images/blob/main/Frok5.png">
</p>

<p align="center">
  <img src="https://github.com/Herzarika/Images/blob/main/Frok6.png">
</p>

<p align="center">
  <img src="https://github.com/Herzarika/Images/blob/main/Frok7.png">
</p>

Klik tanda panah ke bawah di samping tulisan Fork dan klik Create a new fork dan lanjutkan create frok

#### Membuat clone
<p align="center">
  <img src="https://github.com/Herzarika/Images/blob/main/Clone.png">
</p>

Klik Code dan Copy link clone dan lanjutkan, jangan lupa <link_clone> ganti dengan link yang kamu copy tadi dan buang tanda <>

```
cd
git clone <link_clone>
```

#### Membuat directory project
```
cd ~/inery-testnet-faucet-tasks
mkdir $IneryAccname
```

#### Membuat RPC Solution
Langsung aja copy paste command di bawah ini dan gak ada yang dirubah  :

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

#### Membuat Paket Json
Langsung aja copy paste command di bawah ini dan gak ada yang dirubah  :

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
#### Edit file README
```
cd ~/inery-testnet-faucet-tasks/$IneryAccname
nano README.md
```
Nah contohnya silahkan liat [di sini](https://raw.githubusercontent.com/asabigbos/Inery-Task-4-Solution/main/Contoh_Readme.txt), Copy aja, ganti Nama_Akun_Inery dengan nama akun inerymu.

#### Upload ke github
```
cd ~/inery-testnet-faucet-tasks/
git add .
```
```
git commit -m "task 4 solution inery : $IneryAccname"
```
```
git branch -M task4
```
```
git push -u origin task4
```
Lanjutkan dengan login akun githubmu..

### 8.  Kembali lagi ke Github
1. Silahkan Refresh repository Fork yang dibuat di step 2 tadi, lalu click commit ahead 
<p align="center">
  <img src="https://github.com/Herzarika/Images/blob/main/merge1.png">
</p>

2. Lanjutkan Create pull request
<p align="center">
  <img src="https://github.com/Herzarika/Images/blob/main/merge2.png">
  <img src="https://github.com/Herzarika/Images/blob/main/merge3.png">
</p>

3. Check hasilnya di https://github.com/inery-blockchain/inery-testnet-faucet-tasks
<p align="center">
  <img src="https://github.com/Herzarika/Images/blob/main/check.png">
</p>

### Done
Semoga di Approved...
_____________________

## Disclaimer : 

Guide lanjutan ini, masih bersifat meraba-raba, belum fix keberhasilannya. masih tahap ujicoba, jika ada update, akan diperbaiki di kemudian hari. Jika gagal Task 4 setelah mengikuti Guide ini, bukan merupakan tanggung jawab kami. Karna kami juga masih meraba-raba kakak..

_____________________

## Note :
jika tidak dapat terhubung dengan github dan dianggap salah sandi, bisa pake token clasic sandinya
Silahkan klik setting > Developer settings > Personal access tokens > Token (classic) > Generate a personal acces token. Dan silahkan copy, juga sebaiknya simpan aja di databasemu, bisa di notepad atau excel, agar berguna lagi sewaktu-waktu di kemudian hari, biar gak perlu buat-buat lagi.

<p align="center">
  <img src="https://github.com/Herzarika/Images/blob/main/Setting1.png">
</p>

<p align="center">
  <img src="https://github.com/Herzarika/Images/blob/main/setting2.png">
</p>

<p align="center">
  <img src="https://github.com/Herzarika/Images/blob/main/token.png">
</p>

<p align="center">
  <img src="https://github.com/Herzarika/Images/blob/main/kasihnama.png">
</p>

<p align="center">
  <img src="https://github.com/Herzarika/Images/blob/main/token2.png">
</p>

<p align="center">
  <img src="https://github.com/Herzarika/Images/blob/main/simpan.png">
</p>

_____________________
