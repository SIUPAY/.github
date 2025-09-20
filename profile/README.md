## SIU: Blockchain-Based Real-World Payments & Orders Platform

![SIU Logo](https://github.com/user-attachments/assets/dbb31174-5cb9-4537-b0c3-374f3ebc8ab4)

📖 Description  
SIU는 실물 매장에서 바로 사용할 수 있는 블록체인 결제·주문 플랫폼입니다. 고객은 모바일 앱에서 Sui 트랜잭션으로 결제하고, 매장은 관리자 대시보드에서 주문을 실시간 확인·처리합니다. 결제 상태는 Move 스마트컨트랙트 이벤트로 투명하게 추적되며, Google OAuth/zkLogin을 기반으로 누구나 익숙하게 온보딩할 수 있습니다.

Integration with Sui & zkLogin
SIU는 Sui 네트워크와 zkLogin을 기반으로 다음을 제공합니다:

- 결제 트랜잭션 처리: Move 모듈 `payment`를 통해 수수료(기본 2%) 포함 송금 및 `TransferEvent` 발행
- 온체인 이벤트 연동: 백엔드가 체인 이벤트를 폴링하여 결제 성공을 주문·정산에 자동 반영
- Web2 친화 온보딩: Google OAuth를 시작점으로 Salt 발급 → Ephemeral 키/Proof → zkLogin 주소 생성

About Team SIU
우리는 “실사용 가능한 온체인 결제”를 실제 매장에 도입하는 데 집중합니다. Web2 서비스 운영 경험과 Web3 기술을 결합해, 매장과 고객 모두에게 자연스러운 결제 UX를 제공합니다.

⭐ Main Features
- 주문: 생성, 상세, 상태 변경, 검색, 통계
- 결제: `payment::transfer_with_fee`로 2% 수수료 포함 송금 및 이벤트 기록
- 인증: Google OAuth + zkLogin 기반 간편 로그인
- 운영: 카테고리/메뉴/이미지 업로드, 매장·직원 관리
- 실시간성: 신규 주문 감지/알림(폴링) 및 대시보드 반영

🔧 Tech Stack
- Frontend(Admin): Next.js 15, React 19, TypeScript, Tailwind CSS 4, Zustand, TanStack Query, Recharts
- Backend: Spring Boot 3.5 (Kotlin), JPA, WebFlux HTTP Client, Coroutines, OpenAPI, PostgreSQL
- Mobile: Expo/React Native, AsyncStorage, `@mysten/sui`, `bip39`
- Blockchain: Move on Sui (Devnet), Basis Points 수수료(2%), `TransferEvent`
- Infra: Docker, docker-compose, Self-hosted GitHub Actions Runner

📊 Architecture Overview
Order Flow
1) 모바일 앱이 주문 생성(API)
2) 앱이 Sui 트랜잭션 실행(수수료 포함 송금)
3) 트랜잭션 다이제스트로 상태 확인

Settlement Flow
1) 백엔드가 Sui 이벤트를 폴링
2) 결제 성공 이벤트를 주문/정산에 반영하고 DB 업데이트
3) 어드민 대시보드에 변경 사항 반영(폴링/알림)

📂 Component Breakdown
1. Application (Admin Web)
   - Next.js App Router 기반 관리자 UI
   - 주문/메뉴/매장/리뷰/통계/직원 관리
   - `OrderPollingContext`로 신규 주문 감지 및 알림

2. Blockchain (Sui Move)
   - 모듈: `payment`
   - 주요 함수: `transfer_with_fee`, `transfer_with_fee_multiple_coins`, `calculate_fee`
   - 이벤트: `TransferEvent(sender, recipient, amount_sent, fee_amount, net_amount)`

3. Backend (Spring Boot)
   - Kotlin + Ports/Adapters 구조(주문/메뉴/매장/유저/환율/zkloginsalt)
   - WebFlux로 Fullnode RPC 호출, 이벤트 폴링/컨슈밍 후 정산 반영
   - PostgreSQL + JPA, OpenAPI 문서화

4. Mobile (Expo/React Native)
   - `SuiWalletService`: 니모닉 생성/임포트/확인, 잔액/파우셋, 키페어 복원
   - `PaymentService`: 주문 생성 → 트랜잭션 생성/서명/실행 → 상태 확인

💬 Feedback on Building with Sui & zkLogin
Move 모듈로 결제 로직을 단순·명확하게 구성할 수 있었고, 체인 이벤트 기반으로 신뢰 가능한 정산 트래킹을 구현했습니다. zkLogin은 Web2 사용자 온보딩에 유용했으나 Salt/ephemeral 키 관리와 Devnet 특성에 따른 안정성 고려가 필요했습니다. 실시간 반영은 현재 폴링이 가장 안정적이었습니다.

👨‍💻 Role & Contribution
- Product & Design: 문제 정의, 사용자 여정, 관리자 UX 설계
- Frontend Development: Next.js 어드민, 주문/통계/매장/메뉴 UI, 폴링/상태관리
- Blockchain Integration: Move 컨트랙트 설계/테스트, 수수료·이벤트 모델링
- Backend Development: 주문/정산/메뉴/유저 API, 이벤트 컨슈밍, 이미지 업로드, 환율
- Mobile App Development: 지갑 생성/임포트, 결제/주문 플로우, 파우셋/잔액 조회

📌 Future Roadmap
- 스테이블코인 등 다중 결제수단 지원
- WebSocket 기반 실시간 푸시 고도화
- 멀티매장/프랜차이즈 대시보드 및 정산 리포트 강화
- 보안 강화(KMS/HSM 연동), 백업/복구 UX 개선

Made with ❤️ by Team SIU 


---

문의/데모 요청은 언제든 환영합니다. 실제 매장에서 돌아가는 암호화폐 결제, SIU가 보여드리겠습니다.