# install geth in utbuntu
```
  sudo add-apt-repository -y ppa:ethereum/ethereum
  sudo apt-get update
  sudo apt-get install ethereum
```

## After version realse 1.12 POW is no loger supported for this following sleps needed to done

## install go from following link
  ### remeber the version is go1.20.5.linux-amd64.tar.gz
  ``` https://www.digitalocean.com/community/tutorials/how-to-install-go-on-ubuntu-20-04 ```
  ``` go get github.com/shirou/gopsutil/cpu@v3.21.4-0.20210419000835-c7a38de76ee5+incompatible ```
  
  ### clone the git older version
  ```
    cd go-ethereum
    git checkout release/1.11
    git pull
    make geth
  ```
  ### copy to global (once it done close and terminal and reopen and check geth version)
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
    #### enter the password and save the public address in a file

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

  ### initialize data dir where blockchain data is stored
  ```
    geth --datadir ./blockchainData init ./genesis.json
  ```

  ### start blockchain by 
  ``` 
    geth --datadir ./blockchainData --networkid 4785 console 2>> eth.log
  ```

  ### import accoun to your blockchain
  #### copy files from keystore to blockchainData. This will copy accounts that we created prev.
  
  ### to check account details run
  ```
    eth.accounts
  ```
