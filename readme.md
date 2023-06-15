On ubuntu run following command to install eth1.0
  sudo add-apt-repository -y ppa:ethereum/ethereum
  sudo apt-get update
  sudo apt-get install ethereum

mkdir private-ethereum
cd private-ethereum

create account. make 2 
  geth --datadir ./datadir account new
  enter the password and save the public address in a file

make genesis file (detaisl to create a first block)
  paste the following code and replace the address in alloc with the account you prev made
  {
    "config": {
      "chainId": 4785,
      "homesteadBlock": 0,
      "eip155Block": 0,
      "eip158Block": 0,
      "byzantiumBlock": 0,
      "eip150Block": 0,
      "eip150Hash": "0x0000000000000000000000000000000000000000000000000000000000000000"
    },
    "difficulty": "400",
    "gasLimit": "2000000",
    "alloc": {
      "Daa9D9230fA7d30831dC2cfc624155120Bcf1793": { 
        "balance": "100000000000000000000" 
      },
      "7fFFA35Ddd1Dcf33648F8F61C8a712bDBd8d52D2": { 
        "balance": "120000000000000000000000" 
      }
    }
  }
  
initialize data dir where blockchain data is stored
  geth --datadir ./blockchainData init ./genesis.json

start blockchain by 
  geth --datadir ./blockchainData --networkid 4785 console 2>> eth.log
