# 블록체인기술_이더리움_TASK2

# 프로젝트 개요

---

![Untitled](%E1%84%87%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A9%E1%86%A8%E1%84%8E%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%80%E1%85%B5%E1%84%89%E1%85%AE%E1%86%AF_%E1%84%8B%E1%85%B5%E1%84%83%E1%85%A5%E1%84%85%E1%85%B5%E1%84%8B%E1%85%AE%E1%86%B7_TASK2%20ed2b8dcd9af344ca9e9111586dcbec79/Untitled.png)

### User

- Truffle 프로젝트 생성
- OpenZeppelin 라이브러리 중 ERC721.sol을 상송하여 SC 코드 작성
- Truffle을 사용하여 작성한 SC 코드를 컴파일 & 배포
    - .env 파일에 가스비를 지급할 MetaMask의 니모닉과 Infura 프로젝트의 API 작성

### Provider

- Infura : 이더리움 블록체인에 접속할 수 있는 API 엔드포인트 제공
    - User가 작성한 SC를 테스트넷에 배포하는 것을 도움
- MetaMask : 블록체인과 상호작용할 수 있는 암호화폐 지갑
    - 이더리움 메인넷은 물론, 각종 테스트넷과 가나슈 등의 로컬 블록체인과도 연결할 수 있음

### Testnet

- User가 작성하고 배포한 SC의 CA 생성

# 환경 설정

---

1. 메타마스크를 크롬 확장 프로그램으로 설치
    
    [Download MetaMask | Blockchain wallet app and browser extension](https://metamask.io/download/)
    
2. 새 지갑 생성 및 비밀 복구 구문 저장해두기(나중에 씀)

![Untitled](%E1%84%87%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A9%E1%86%A8%E1%84%8E%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%80%E1%85%B5%E1%84%89%E1%85%AE%E1%86%AF_%E1%84%8B%E1%85%B5%E1%84%83%E1%85%A5%E1%84%85%E1%85%B5%E1%84%8B%E1%85%AE%E1%86%B7_TASK2%20ed2b8dcd9af344ca9e9111586dcbec79/Untitled%201.png)

![Untitled](%E1%84%87%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A9%E1%86%A8%E1%84%8E%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%80%E1%85%B5%E1%84%89%E1%85%AE%E1%86%AF_%E1%84%8B%E1%85%B5%E1%84%83%E1%85%A5%E1%84%85%E1%85%B5%E1%84%8B%E1%85%AE%E1%86%B7_TASK2%20ed2b8dcd9af344ca9e9111586dcbec79/Untitled%202.png)

1. 메타마스크 네트워크 추가를 위해 ChainList 검색
    
    [Chainlist](https://chainlist.org/)
    
2. (테스트넷 포함 활성화 후) Mumbai와 연결 (또는 Sepolia)
    
    ![Untitled](%E1%84%87%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A9%E1%86%A8%E1%84%8E%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%80%E1%85%B5%E1%84%89%E1%85%AE%E1%86%AF_%E1%84%8B%E1%85%B5%E1%84%83%E1%85%A5%E1%84%85%E1%85%B5%E1%84%8B%E1%85%AE%E1%86%B7_TASK2%20ed2b8dcd9af344ca9e9111586dcbec79/Untitled%203.png)
    
    ![Untitled](%E1%84%87%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A9%E1%86%A8%E1%84%8E%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%80%E1%85%B5%E1%84%89%E1%85%AE%E1%86%AF_%E1%84%8B%E1%85%B5%E1%84%83%E1%85%A5%E1%84%85%E1%85%B5%E1%84%8B%E1%85%AE%E1%86%B7_TASK2%20ed2b8dcd9af344ca9e9111586dcbec79/Untitled%204.png)
    
3. 메타마스크 지갑 주소로 Mumbai(또는 Sepolia) 테스트넷 faucet에서 ETH/MATIC 요청
    
    ![Untitled](%E1%84%87%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A9%E1%86%A8%E1%84%8E%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%80%E1%85%B5%E1%84%89%E1%85%AE%E1%86%AF_%E1%84%8B%E1%85%B5%E1%84%83%E1%85%A5%E1%84%85%E1%85%B5%E1%84%8B%E1%85%AE%E1%86%B7_TASK2%20ed2b8dcd9af344ca9e9111586dcbec79/Untitled%205.png)
    
4. Infura에 접속하여 회원가입 한 후에 API KEY 발급받기
    - Web3 API + (원하는 프로젝트 이름 입력)
        
        ![Untitled](%E1%84%87%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A9%E1%86%A8%E1%84%8E%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%80%E1%85%B5%E1%84%89%E1%85%AE%E1%86%AF_%E1%84%8B%E1%85%B5%E1%84%83%E1%85%A5%E1%84%85%E1%85%B5%E1%84%8B%E1%85%AE%E1%86%B7_TASK2%20ed2b8dcd9af344ca9e9111586dcbec79/Untitled%206.png)
        
5. 원하는 테스트넷 활성화 (이번 실습에서는 Polygon의 Mumbai)
    - 활성화 시 결제 수단 등록을 요구하나, 이용은 무료로 가능
    
    ![Untitled](%E1%84%87%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A9%E1%86%A8%E1%84%8E%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%80%E1%85%B5%E1%84%89%E1%85%AE%E1%86%AF_%E1%84%8B%E1%85%B5%E1%84%83%E1%85%A5%E1%84%85%E1%85%B5%E1%84%8B%E1%85%AE%E1%86%B7_TASK2%20ed2b8dcd9af344ca9e9111586dcbec79/Untitled%207.png)
    
6. 원하는 디렉토리에서 vscode로 터미널 열고 (mumbai 기준) `truffle unbox polygon` 입력 (sepolia는 `truffle init`)

![Untitled](%E1%84%87%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A9%E1%86%A8%E1%84%8E%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%80%E1%85%B5%E1%84%89%E1%85%AE%E1%86%AF_%E1%84%8B%E1%85%B5%E1%84%83%E1%85%A5%E1%84%85%E1%85%B5%E1%84%8B%E1%85%AE%E1%86%B7_TASK2%20ed2b8dcd9af344ca9e9111586dcbec79/Untitled%208.png)

# NFT 스마트 컨트랙트 구현

---

1. 트러플 프로젝트에 `npm install @openzeppelin/contracts`로 openzeppelin 라이브러리 추가
    
    ![Untitled](%E1%84%87%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A9%E1%86%A8%E1%84%8E%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%80%E1%85%B5%E1%84%89%E1%85%AE%E1%86%AF_%E1%84%8B%E1%85%B5%E1%84%83%E1%85%A5%E1%84%85%E1%85%B5%E1%84%8B%E1%85%AE%E1%86%B7_TASK2%20ed2b8dcd9af344ca9e9111586dcbec79/Untitled%209.png)
    
2. `.env` 파일을 추가하여 MNEMONIC과 INFURA_PROJECT_ID 환경변수 설정하기
    
    ```python
    #MNEMONIC="Your MetaMask Mnemonic" 메타마스크 비밀 복구 구문
    MNEMONIC="bubble nasty master anchor lonely supply below bus odor error betray pear"
    #INFURA_PROJECT_ID="Your Infura Project API KEY"
    INFURA_PROJECT_ID="759d5ea1a0e248ab8108f0cacb20b3af"
    ```
    
3. (Mumbai 기준) truffle-config.polygon.js 수정
    - network의 polygon_infura_testnet을 mumbai로 수정
    
    ```python
    networks: {
        development: {
          host: "127.0.0.1",     // Localhost (default: none)
          port: 8545,            // Standard Ethereum port (default: none)
          network_id: "*",       // Any network (default: none)
        },
        //polygon Infura mainnet
        polygon_infura_mainnet: {
          provider: () => new HDWalletProvider({
            mnemonic: {
              phrase: mnemonic
            },
            providerOrUrl:
             "https://polygon-mainnet.infura.io/v3/" + infuraProjectId
          }),
          network_id: 137,
          confirmations: 2,
          timeoutBlocks: 200,
          skipDryRun: true,
          chainId: 137
        },
        //polygon Infura testnet
        mumbai: {
          provider: () => new HDWalletProvider({
            mnemonic: {
              phrase: mnemonic
            },
            providerOrUrl:
             "https://polygon-mumbai.infura.io/v3/" + infuraProjectId
          }),
          network_id: 80001,
          confirmations: 2,
          timeoutBlocks: 200,
          skipDryRun: true,
          chainId: 80001
        }
    },
    ```
    
    - 그 외, 컴파일러 버전 확인
    
    ![Untitled](%E1%84%87%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A9%E1%86%A8%E1%84%8E%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%80%E1%85%B5%E1%84%89%E1%85%AE%E1%86%AF_%E1%84%8B%E1%85%B5%E1%84%83%E1%85%A5%E1%84%85%E1%85%B5%E1%84%8B%E1%85%AE%E1%86%B7_TASK2%20ed2b8dcd9af344ca9e9111586dcbec79/Untitled%2010.png)
    
4. Openzeppelin/contracts에서 ERC721.sol을 import하여 solidity 코드 작성
    
    ```solidity
    // SPDX-License-Identifier: UNLICENSED
    pragma solidity ^0.8.10;
    
    import "../.././node_modules/@openzeppelin/contracts/token/ERC721/ERC721.sol";
    
    contract myERC721 is ERC721{
        mapping(address => uint256) public tokenBalance;
        mapping(uint256 => address) public tokenOwner;
        mapping(uint256 => uint256) public tokenPrice;
        mapping(uint256 => bool) public isForSale;
        mapping(uint256 => address[]) public tokenOwnerHistory;
    
        struct Image {
            string name;
            string url;
            string myAttributes;
        }
        Image[] public images;
    
        constructor(
            string memory _name,
            string memory _symbol
        ) ERC721(_name, _symbol) {}
    
        // tokenId의 소유자 기록 반환
        function ownerHistoryOf(uint256 tokenId) public
            view returns (address[] memory) {
            return tokenOwnerHistory[tokenId];
        }
            // 토큰 판매 시작
        function sellToken(uint256 _tokenId) public {
            /* 함수를 호출하는 주소는 해당 토큰Id의 보유자여야 합니다. */
            require(msg.sender == tokenOwner[_tokenId], "Only owner can sell token");
            /* onSale 함수를 호출해 해당 토큰Id의 판매 상태를 true로 바꿉니다. */
            onSale(_tokenId, true);
            /* 토큰의 가격은 5 ETH로 고정합니다. */
            tokenPrice[_tokenId] = 5 ether;
        }
            // 토큰 판매 여부 설정
        function onSale(uint256 _tokenId, bool _forSale) public {
            isForSale[_tokenId] = _forSale;
        }
    
        // 토큰 판매 여부 확인
        function isOnSale(uint256 _tokenId) public
            view returns (bool) {
            return /* 채우세요 */ isForSale[_tokenId];
        }
            // 토큰 구매 기능
        function buyToken(uint256 _tokenId) public payable {
            /*
              유효성 확인:
              1 토큰이 판매중인가
              2 전송한 이더량이 토큰을 살 수 있는 양인가
              3 판매자와 구매자가 동일 인물이 아닌가
            */
           
            require(isForSale[_tokenId], "Validation failed: Token is not on sale");
            require(msg.value >= tokenPrice[_tokenId], "Validation failed: Not enough ETH");
            address owner = ownerOf(_tokenId);
            require(msg.sender != owner, "Validation failed: You cannot buy your own token");
    
            /* 토큰의 가격만큼의 ETH를 판매자에게 전송 */
            payable(owner).transfer(tokenPrice[_tokenId]);
            /* 토큰을 구매자의 소유로 이동 */
            transferFrom(owner, msg.sender, _tokenId);
    
            /* 토큰 소유주 내역에 구매자의 주소 추가 */
            tokenOwnerHistory[_tokenId].push(msg.sender);
            /* 토큰 가격을 지불하고 남은 금액을 구매자에게 재전송 */
            payable(msg.sender).transfer(msg.value - tokenPrice[_tokenId]);
    
            
            /* 토큰의 판매 여부 초기화 */
            isForSale[_tokenId] = false;
            tokenPrice[_tokenId] = 0;
        }
        
        function mint(
            address _to,   // NFT를 발행하고 소유할 주소
            string memory _name,   // NFT의 이름
            string memory _url,   // NFT의 path
            string memory myAttributes   // NFT의 속성
        ) public {
            /* NFT를 소유하는 주소는 address(0)가 아니어야 함 */
            require(_to != address(0), "Empty address cannot mint NFT");
            uint _tokenId = images.length;
            require(!_exists(_tokenId), "1 tokenId can only be assigned to 1 NFT");
            /* 발행된 토큰Id를 서로 다른 두 NFT가 공유해서는 안 됨 */
            images.push(  /* images 배열에 발행된 NFT 추가 */
                Image(_name, _url, myAttributes)
            );
            tokenOwnerHistory[_tokenId].push(_to);
            /* 발행된 NFT의 소유주 기록에 발행자의 주소 추가 */
            _mint(_to, _tokenId);
        }
    
    }
    ```
    
5. `truffle compile` 뒤 `truffle migrate --config truffle-config.polygon.js --network mumbai` 로 배포
    
    <aside>
    💡 compile time에서 compiler 관련 에러가 발생하였는데, 트러플 프로젝트가 자동으로 생성해준 파일들이 선언하고 있는 컴파일러 버전이 맞지 않아서 발생한 이슈였습니다. 따라서 모든 파일의 pragma를 0.8.10으로 수정해 주었습니다.
    
    </aside>
    
    아래와 같이 정상적으로 배포가 완료된 것을 확인할 수 있습니다.
    
    ![Untitled](%E1%84%87%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A9%E1%86%A8%E1%84%8E%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%80%E1%85%B5%E1%84%89%E1%85%AE%E1%86%AF_%E1%84%8B%E1%85%B5%E1%84%83%E1%85%A5%E1%84%85%E1%85%B5%E1%84%8B%E1%85%AE%E1%86%B7_TASK2%20ed2b8dcd9af344ca9e9111586dcbec79/Untitled%2011.png)
    
    아래 주소로 가서 지갑 주소를 겁색하면 다음과 같이 contract creation도 확인할 수 있습니다.
    
    [TESTNET  Polygon (MATIC) Blockchain Explorer](https://mumbai.polygonscan.com/)
    
    ![Untitled](%E1%84%87%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A9%E1%86%A8%E1%84%8E%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%80%E1%85%B5%E1%84%89%E1%85%AE%E1%86%AF_%E1%84%8B%E1%85%B5%E1%84%83%E1%85%A5%E1%84%85%E1%85%B5%E1%84%8B%E1%85%AE%E1%86%B7_TASK2%20ed2b8dcd9af344ca9e9111586dcbec79/Untitled%2012.png)
    
    그리고 다음은 배포한 SC의 CA입니다.
    
    ![Untitled](%E1%84%87%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A9%E1%86%A8%E1%84%8E%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%80%E1%85%B5%E1%84%89%E1%85%AE%E1%86%AF_%E1%84%8B%E1%85%B5%E1%84%83%E1%85%A5%E1%84%85%E1%85%B5%E1%84%8B%E1%85%AE%E1%86%B7_TASK2%20ed2b8dcd9af344ca9e9111586dcbec79/Untitled%2013.png)
    
    위에서 보이는 바와 같이 CA는 **0xBBd396eabD504884DdDbb6b2076fe6E699Ce743A**입니다.
    
    *아래 링크는 CA url입니다.*
    
    [Contract Address 0xbbd396eabd504884dddbb6b2076fe6e699ce743a | PolygonScan](https://mumbai.polygonscan.com/address/0xbbd396eabd504884dddbb6b2076fe6e699ce743a)