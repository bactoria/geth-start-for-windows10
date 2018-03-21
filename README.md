
#geth ( go-Ethereum)

<BR/>

### TODO geth Download 추가

<BR/>

### Genesis Block 등록

**"D:\genesis\genesis.json"**
```json
"config": {
"chainId": 42,
"homesteadBlock": 0,
"eip150Block": 0,
"eip150Hash":"0x0000000000000000000000000000000000000000000000000000000000000000",
"eip155Block": 0,
"eip158Block": 0
},

    "nonce": "0x0000000000000076",
    "timestamp": "0x00",
    "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "extraData": "0x00",
    "gasLimit": "0x8000000",
    "difficulty": "0x40",
    "mixhash": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "coinbase": "0x3333333333333333333333333333333333333333",
    "alloc": {"0xe3ea56b59bd8a8e410deafe6e57e893b8f6a98b0" : {"balance" : "300000000000000000000"}
    }

}

```

```Java
geth --datadir "D:\BlockChain06" init "D:\genesis\genesis.json"
```

<BR/>

### Data 생성

```
geth --identity "JayBlockChain07" --rpc --rpcport "8080" --rpccorsdomain "*" --datadir "D:\BlockChain07" --port "30303" --nodiscover --rpcapi "db,eth,net,web3" --networkid 1999 console
```
or
```
geth --datadir "D:\BlockChain01"
```

<BR/>


**geth console모드**

```java
geth --datadir "D:\BlockChain12" console
```


```Java
> personal.newAccount()
Passphrase:
Repeat passphrase:
"0x2b92f15795e68184c13f7909bd7a677a001c9795"
> personal.newAccount()
Passphrase:
Repeat passphrase:
"0xb96e9e8de522371cdd34f96b803f556645d8f204"

```

```Java
> eth.accounts
```

```
> miner.setEtherbase(personal.listAccounts[0]) //또는 아래방법
> miner.setEtherbase(0x304b69717529ab3db3813fdc9861e70a9ea0eff8)
```


```java
> eth.getBalance(eth.accounts[0])
//0
```

```
> exit
```

```
> eth.getBalance(eth.accounts[0])
```
