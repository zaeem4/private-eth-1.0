# install geth in utbuntu
```
  sudo add-apt-repository -y ppa:ethereum/ethereum
  sudo apt-get update
  sudo apt-get install ethereum
```

## After version release 1.12 POW is no loger supported for this following steps needed to done

  ### install go from following link if not install yet
  ### remeber the version is go1.20.5.linux-amd64.tar.gz
  ``` https://www.digitalocean.com/community/tutorials/how-to-install-go-on-ubuntu-20-04 ```
  
  ### clone the git older version
  ```
    cd go-ethereum
    git checkout release/1.11
    git pull
    make geth
  ```
  ### copy to global (once it done close the terminal and reopen and check geth version)
  ``` sudo cp build/bin/geth /usr/local/bin/ ``` 

  ### create folder
  ```
    mkdir private-ethereum
    cd private-ethereum
  ```

  ### create account. make 2 
  ```
    geth --datadir ./datadir account new
  ```
  
  ### make genesis file (details to create a first block)
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

  ### initialize data dir where blockchain data will store
  ```
    geth --datadir ./blockchainData init ./genesis.json
  ```

  ### start blockchain by 
  ``` 
    geth --datadir ./blockchainData --networkid 4785 console 2>> eth.log
  ```

  ### import accoun to your blockchain
  ##### copy files from keystore to blockchainData. This will copy accounts that we created prev.
  
  ### to check account details run
  ```
    eth.accounts
  ```
  ### to run another node
  1. create a new folder like blockchainData with another name
  2. initialize the genesis block for this folder
  3. run the following command to run a new node in sperate terminal
  4. ``` geth --datadir ./blockchainData2 --networkid 4785 --port 30305 --authrpc.port 8553 console ```
  5. the port and authrpc port must be change for every node if running on same machine
  
  ### peer the nodes
  1. In node 1 run 
  ```
    admin.nodeInfo
  ```
  2. copy encode value and open terminal of node 2 and run the command and paste the value as a strign in param
  ```
    admin.addPeer()
  ```
  3. now open terminal of node 1 or node 2 and run admin.peers to see if node is added
  ```
    admin.peers
  ```
  4. if its return an array of objects its means nodes are peers. if empty array its mean node are not peer
  5. now we will start mining
  ```
    miner.setEtherbase(eth.accounts[0])
    miner.start()
  ```
  6. to stop minning 
  ```
    miner.stop()
  ```

 
