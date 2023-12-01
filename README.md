#HTLC 대학교 과제
-------------------------------------------

##### HTLC lock 소스코드을 활용하여 ethereum testnet에서 본인의 etherum wallet 계정에서 스마트 컨트랙트로 동작시키고, 이를 Web3.js 로 연계 호출하여 동작하는 것을 만드는 과제입니다.
------------------------------

npm으로 설치하여 사용한것들
truffle 
    >npm install truffle
ganache
    >npm install ganache
web3
    >npm install web3

### truffle을 실행하는법

    >truffle develop

실행후 develop모드에서 

    >test

또는

    >truffle test

를 입력하여 실행 할 수 있다.

### ganache를 실행하는 법
    npm run ganache
를 이용하여 실행할 수 있다.

ganache를 실행할때는 자신이 truffle에 설정해놓은 네트워크아이디나.
port를 확인하고 실행시키고 truffle을 작동시켜야 한다.
ganache 실행과 동시에 네트워크ID설정과 포트 설정은 아래와 같이 한다.
    ganache-cli --networkId 4447 --port=7542 
    ganache-cli --networkId (네트워크아이디) --port=(포트번호)



git clone 한 주소
    git clone https://github.com/GOOGZHOU/htlc-lc-eth

