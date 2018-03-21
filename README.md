
# geth ( go-Ethereum)


<BR/>

### TODO geth Download 추가


<BR/>

**제네시스 블록 파일(json) 생성**

config 없으면 제네시스 등록 안되는것같던데 시부럴..
아닐텐데..

**아래 내용을 "genesis01.json"으로 저장한다
(네이밍은 꼭 `genesis01`이 아니어도됨)
(경로는 아무곳이나 자기 편한곳으로 넣으면 됨)**

```json
{
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
    "alloc": {}
}

```

<BR/>

**geth 생성(?) & 제네시스 블록 등록**


```Java
geth --datadir "D:\BlockChain01" init "D:\BlockChain01\genesis01.json"
```

* `--datadir "D:\BlockChain01"` data를 넣을 경로를 지정해준다. (앞으로 모든 데이터는 이 경로에 쌓임)
* `init` : 제네시스 블록을 등록하겠다는 명령어.
* `"D:\BlockChain01\genesis01.json"` : 위에서 등록한 json파일 경로적어주면됨

<BR/>


<BR/>

### 계정 생성 해보기
**먼저 콘솔로 진입해야 한다.**

```java
geth --datadir "D:\BlockChain" console
```

그러면 콘솔창앞에 `>` 으로 바뀐다.

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

* `personal.newAccount()` 치고 엔터!
* `Passphrase:` 계정 비밀번호 입력
* `Repeat passphrase:` 계정 비밀번호 확인

( 이 때 16진수 40자리가 출력되는데, **Account의 주소**이다. )

* Account를 1개 더 만든다

<BR/>


### 계정에 돈 넣어보기

**이제, 등록된 계정에 돈을 넣어볼테다.**
**제네시스 블록의 `alloc`을 이용하여 특정 Account에 ether를 미리 할당할 수 있다.**

```json
    ... 생략

    "alloc": {
      "0x2b92f15795e68184c13f7909bd7a677a001c9795" : {
      "balance" : "300000000000000000000"
      },
      "0xb96e9e8de522371cdd34f96b803f556645d8f204" : {
      "balance" : "300000000000000000000"
      }
    }

```
* 앞서 만들었던 json파일의 `alloc`을 다음과 같이 수정하기
 **주의 : 그대로 복붙하면 안됨. 자기가 만들었던 Account의 주소를 입력해야 함.**

<BR/>

**적용시키기 위해선 현재 geth파일들을 삭제해주고 다시 만들어야 한다.**

1) **지정한 폴더에서 `history파일, geth폴더 삭제`**

<br/>

2) **geth 재생성(?) & 제네시스 블록 등록**

```Java
geth --datadir "D:\BlockChain01" init "D:\BlockChain01\genesis01.json"
```
**주의 : 위에서 입력했던 본인경로로 입력해야함!!**

<BR/>

**계좌에 돈 들어왔는지 확인**

1) **콘솔 진입하기**

```java
geth --datadir "D:\BlockChain01" console
```

2) **계좌 잔액 출력**

```
> eth.getBalance(eth.accounts[0])
300000000000000000000
```

<BR/>

# 앙 개꿀띠!

<BR/>

#- End -

<BR/>
<BR/>

# 그 외
**( 마이너 설정 )**
```
> miner.setEtherbase(personal.listAccounts[0]) //또는 아래방법
> miner.setEtherbase(0x304b69717529ab3db3813fdc9861e70a9ea0eff8)
```

<BR/>

**Console Out!**
```
> exit
```
### (Data 생성)

```
geth --identity "JayBlockChain07" --rpc --rpcport "8080" --rpccorsdomain "*" --datadir "D:\BlockChain07" --port "30303" --nodiscover --rpcapi "db,eth,net,web3" --networkid 1999 console
```
or
```
geth --datadir "D:\BlockChain01"
```

<BR/>
