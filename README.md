
# geth ( go-Ethereum)


<BR/>

### geth 설치

https://ethereum.github.io/go-ethereum/downloads/

윈도우버전 설치

<BR/>

**설치가 잘 됬는지 확인**

명령프롬프트 키고 `geth version` 입력후 엔터

버전 잘뜨면 잘 깔린거 ~

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

* `--datadir "D:\BlockChain01"` data를 넣을 임의의 경로를 지정해준다. (앞으로 모든 데이터는 이 경로에 쌓임)
* `init` : 제네시스 블록을 등록하겠다는 명령어.
* `"D:\BlockChain01\genesis01.json"` : 위에서 생성한 json파일 경로적어주면됨

<BR/>


<BR/>

### 계정 생성 해보기
**먼저 콘솔로 진입해야 한다.**

```java
geth --datadir "D:\BlockChain01" console
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
**제네시스 블록의 `alloc`에서 특정 Account에 ether를 미리 할당할 수 있다.**

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

 **( 주의 : 그대로 복붙하면 안됨. 자기가 만들었던 Account의 주소를 입력해야 함. )**

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


## 마이닝 해보자!

**계좌 1개 더 생성( 마이너계좌로 쓸거임 )**
```
> personal.newAccount()
Passphrase:
Repeat passphrase:
"0xfdf777cfed6311cac52b0bf6e2b70ead2e75a759"
```
`Accounts[0]` 와 `Accounts[1]`은 위에서 이미 만들었으므로

`Accounts[2]`에 새로운 계좌가 생성한다.


<br/>

**계좌 생성되었는지 확인**
```java
> eth.Accounts[2]
"0xfdf777cfed6311cac52b0bf6e2b70ead2e75a759" //40자리 16진수나오면 생성됨
```

<br/>

**마이너 계좌로 등록**
```java
> miner.setEtherbase(eth.Accounts[2])
true //true 뜨면 등록 완료
```

**채굴하기**
```Java
> miner.start()
> miner.start(2) // 마이닝 Thread를 2개로 지정

//정지는 miner.stop()
```

## Accounts[0] -> Accounts[1] 송금하기

```java

> acc1 = eth.accounts[0]
> acc2 = eth.accounts[1]
> web3.eth.sendTransaction({from:acc1, to:acc2, value:web3.toWei(5,'ether'), gasLimit: 21000, gasPrice:2000000000})
Error: authentication needed: password or unlock
    at web3.js:3143:20
    at web3.js:6347:15
    at web3.js:5081:36
    at <anonymous>:1:1

```

* `gasLimit`, `gasPrice` 는 선택사항(Optional) 이다. 없어도 동작한다는 말
* 에러가 안뜨고 `...` 이 뜬다면 `miner.start()`를 실행시킨 후 다시 보내자.


**에러뜬다(password or unlock)**
이게 된다면 내돈이 니돈이고 니돈이 내돈이 되버린다.

인증이 필요하다.

**계정 Lock Status 확인**
```Java
> personal.listWallets[0].status
"Locked" //계정 Account[0] 이 잠겨있다. Unlock이 필요하다.
```
<br/>

**UnLock 하기**

```Java
> web3.personal.unlockAccount(acc1)
// (personal.unlockAccount(acc1)) 똑같은거같은데.. web3로했을때 머가다르노
// 일단 web3가 뭐인지부터... ㄷㄷㄷ
Unlock account 0xb207a4f38a969848e4400d8f83f0ea53739e8ca1
Passphrase: //비밀번호 입력
true //UnLock 성공

//계정 Lock Status 확인
> personal.listWallets[0].status
"UnLocked"
```

<br/>

**( 다시 Lock 하는 방법 )**
```Java
>personal.lockAccount(acc1)
true

//계정 Lock Status 확인
> personal.listWallets[0].status
"Locked"
```

**다시 송금하자**
```Java
> web3.eth.sendTransaction({from:acc1, to:acc2, value:web3.toWei(5,'ether'), gasLimit: 21000, gasPrice:2000000000})
>
```


## TODO 마이너계좌 돈 확인

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
