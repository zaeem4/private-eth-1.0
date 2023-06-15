#After version realse 1.12 POW is no loger supported for this following sleps needed to done

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
