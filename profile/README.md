## SIU: Blockchain-Based Real-World Payments & Orders Platform

![SIU Logo](https://github.com/user-attachments/assets/dbb31174-5cb9-4537-b0c3-374f3ebc8ab4)

📖 Description  
SIU is a blockchain-based payments and ordering platform that works right in brick-and-mortar stores. Customers pay with a Sui transaction from the mobile app, and merchants review and process orders in real time on the admin dashboard. Payment status is transparently tracked via Move smart contract events, and onboarding is familiar thanks to Google OAuth/zkLogin.

Integration with Sui & zkLogin
Based on the Sui network and zkLogin, SIU provides:

- Payment transaction processing: transfer with fee (default 2%) via the Move module `payment`, and emit `TransferEvent`
- On-chain event integration: the backend polls chain events to automatically reflect successful payments in orders/settlement
- Web2-friendly onboarding: start with Google OAuth → issue salt → ephemeral key/proof → create zkLogin address

About Team SIU
We focus on bringing “practical, real-world on-chain payments” to physical stores. By combining Web2 operational experience with Web3 technology, we deliver a natural payment UX for both merchants and customers.

⭐ Main Features
- Orders: create, detail, status updates, search, analytics
- Payments: send with a 2% fee via `payment::transfer_with_fee`, with event logging
- Authentication: simple login with Google OAuth + zkLogin
- Operations: category/menu/image upload, store/staff management
- Real-time: detect/notify new orders (polling) and reflect changes on the dashboard

🔧 Tech Stack
- Frontend (Admin): Next.js 15, React 19, TypeScript, Tailwind CSS 4, Zustand, TanStack Query, Recharts
- Backend: Spring Boot 3.5 (Kotlin), JPA, WebFlux HTTP Client, Coroutines, OpenAPI, PostgreSQL
- Mobile: Expo/React Native, AsyncStorage, `@mysten/sui`, `bip39`
- Blockchain: Move on Sui (Devnet), 2% fee in basis points, `TransferEvent`
- Infra: Docker, docker-compose, Self-hosted GitHub Actions Runner

📊 Architecture Overview
Order Flow
1) Mobile app creates an order (API)
2) App executes a Sui transaction (transfer including fee)
3) Check status via the transaction digest

Settlement Flow
1) Backend polls Sui events
2) Reflect successful payment events into orders/settlement and update the DB
3) Propagate changes to the admin dashboard (polling/notification)

<img width="910" height="600" alt="Image" src="https://github.com/user-attachments/assets/0819abd7-e43d-4fdb-b76b-93ba0d24bfe4" />

📂 Component Breakdown
1. Application (Admin Web)
   - Admin UI built on Next.js App Router
   - Manage orders/menus/stores/reviews/analytics/staff
   - Detect new orders and notify via `OrderPollingContext`

2. Blockchain (Sui Move)
   - Module: `payment`
   - Key functions: `transfer_with_fee`, `transfer_with_fee_multiple_coins`, `calculate_fee`
   - Event: `TransferEvent(sender, recipient, amount_sent, fee_amount, net_amount)`

3. Backend (Spring Boot)
   - Kotlin + Ports/Adapters architecture (orders/menus/stores/users/exchange rates/zkLogin salt)
   - Call Fullnode RPC via WebFlux, poll/consume events, and reflect them in settlement
   - PostgreSQL + JPA, OpenAPI documentation

4. Mobile (Expo/React Native)
   - `SuiWalletService`: generate/import/verify mnemonic, balance/faucet, restore keypair
   - `PaymentService`: create order → build/sign/execute transaction → check status

💬 Feedback on Building with Sui & zkLogin
With the Move module, we could keep the payment logic simple and clear, and implement trustworthy settlement tracking based on chain events. zkLogin was useful for Web2-style user onboarding, but it requires careful handling of salt/ephemeral keys and consideration for Devnet stability. For real-time updates, polling has been the most reliable approach so far.

👨‍💻 Role & Contribution
- Product & Design: problem definition, user journey, admin UX design
- Frontend Development: Next.js admin, UI for orders/analytics/stores/menus, polling/state management
- Blockchain Integration: Move contract design/testing, fee/event modeling
- Backend Development: APIs for orders/settlement/menus/users, event consuming, image upload, exchange rates
- Mobile App Development: wallet create/import, payment/order flow, faucet/balance

📌 Future Roadmap
- Support multiple payment instruments including stablecoins
- Enhance real-time push with WebSocket
- Strengthen multi-store/franchise dashboard and settlement reports
- Improve security (integrate KMS/HSM) and backup/restore UX

Made with ❤️ by Team SIU 


---

Inquiries or demo requests are always welcome. We’ll show you cryptocurrency payments running in real stores with SIU.