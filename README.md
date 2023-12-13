# install geth in Ubuntu
```
  sudo add-apt-repository -y ppa:ethereum/ethereum
  sudo apt-get update
  sudo apt-get install ethereum
```

## After version release 1.12 POW is no longer supported for this following steps needed to be done

  ### Install go from the following link if not installed yet
  ### remeber the version is go1.20.5.linux-amd64.tar.gz
  ``` https://www.digitalocean.com/community/tutorials/how-to-install-go-on-ubuntu-20-04 ```
  
  ### Clone the git older version
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

  ### Create an account. make 2 
  ```
    geth --datadir ./datadir account new
  ```
  
  ### Make genesis file (details to create a first block)
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

  ### Start blockchain by 
  ``` 
    geth --datadir ./blockchainData --networkid 4785 console 2>> eth.log
  ```

  ### Import account to your blockchain
  ##### copy files from keystore to blockchainData. This will copy accounts that we created prev.
  
  ### to check account details run
  ```
    eth.accounts
  ```
  ### to run another node
  1. create a new folder like blockchainData with another name
  2. initialize the genesis block for this folder
  3. run the following command to run a new node in a separate terminal
  4. ``` geth --datadir ./blockchainData2 --networkid 4785 --port 30305 --authrpc.port 8553 console ```
  5. the port and authrpc port must be changed for every node if running on the same machine
  
  ### peer the nodes
  1. In node 1 run 
  ```
    admin.nodeInfo
  ```
  2. copy the encode value and open the terminal of node 2, run the command and paste the value as a string in param
  ```
    admin.addPeer()
  ```
  3. now open the terminal of node 1 or node 2 and run admin.peers to see if the node is added
  ```
    admin.peers
  ```
  4. if it returns an array of objects it means nodes are peers. if an empty array its mean nodes are not peer
  5. now we will start mining
  ```
    miner.setEtherbase(eth.accounts[0])
    miner.start()
  ```
  6. to stop mining 
  ```
    miner.stop()
  ```
  7. to stop mining empty block add the following script
  ```
    var mining_threads = 1
    function checkWork() {
       if (eth.getBlock("pending").transactions.length > 0) {
           if (eth.mining) return;
           console.log("== Pending transactions! Mining...");
           miner.start(mining_threads);
       } else {
           miner.stop();
           console.log("== No transactions! Mining stopped.");
       }
    }
    
    console.log("checkWork() is defined");
    eth.filter("latest", function(err, block) { checkWork(); });
    eth.filter("pending", function(err, block) { checkWork(); });
    
    checkWork();
  ```
  8. now run the node with the command (just need to add preload script| all others params are only for accessing it with metamask)
  ```
    geth --networkid 47580 --datadir "./node1" --port 30303 --http --http.port '8545' --snapshot=false --ipcdisable --syncmode 'full' --allow-insecure-unlock --    
     http.corsdomain "" --http.vhosts "" --http.addr "0.0.0.0" --preload "startmine.js" console 2>> eth-new.log
  ```

 
