# Stacks-EscroMark Commodities Trading Smart Contract

This Clarity smart contract implements a secure, decentralized commodities trading system on the Stacks blockchain. It supports real-time price updates via oracles, escrow-based trade execution, and tracking of active trading positions.

---

## Features

- **Commodity Registration:** Admins can register new commodities with initial inventory and price.
- **Price Oracle Integration:** Contract supports an oracle for commodity price feeds.
- **Escrow System:** Users deposit STX into escrow to fund their trades.
- **Trade Execution:** Traders can execute and close trading positions on available commodities.
- **Trading Toggle:** Admin can enable or disable global trading.
- **Role Control:** Administrator and Oracle roles are securely enforced.
- **Market Data Access:** Anyone can query market data and trader positions.

---

##  Error Codes

| Code       | Description                                |
|------------|--------------------------------------------|
| `100`      | Unauthorized Access                        |
| `101`      | Invalid Commodity Price                    |
| `102`      | Insufficient Escrow Balance                |
| `103`      | Trading is currently disabled              |
| `104`      | Invalid Trade Quantity                     |
| `105`      | Escrow Transfer Failed                     |
| `106`      | Invalid Input Provided                     |
| `404`      | Not Found (for read-only queries)          |

---

##  Data Structures

### Data Variables

- `contract-administrator` → Controls access to admin-only functions.
- `commodity-price-oracle` → Address authorized to push price updates.
- `market-trading-status` → Boolean toggle for enabling/disabling trading.
- `minimum-trade-quantity` → Minimum quantity required per trade.

### Maps

- `available-commodities-inventory`  
  Stores available commodities with:
  - `available-quantity`
  - `current-market-price`
  - `commodity-owner`

- `active-trading-positions`  
  Stores open trades with:
  - `commodity-identifier`
  - `traded-quantity`
  - `position-entry-price`
  - `position-creation-timestamp`

- `trader-escrow-accounts`  
  Maps traders to their current escrow balances.

---

## Read-Only Functions

| Function | Description |
|---------|-------------|
| `get-commodity-market-data(commodity-id)` | Returns market data for a commodity. |
| `get-trader-position-details(address, position-id)` | Returns details of a trading position. |
| `get-trader-escrow-balance(address)` | Returns current escrow balance. |

---

##  Public Functions

### Admin Functions

- `initialize-contract(administrator-address)`
- `update-price-oracle-address(new-oracle)`
- `toggle-market-trading-status()`
- `register-new-commodity(commodity-id, quantity, price)`

### Escrow Management

- `deposit-funds-to-escrow(amount)`
- `withdraw-funds-from-escrow(amount)`

###  Trading

- `execute-trade(commodity-id, quantity, position-id)`
- `close-trading-position(position-id)`

---

## Usage Flow

1. **Initialization**
    - Deploy the contract and call `initialize-contract()` to set the admin/oracle.

2. **Commodity Setup**
    - Admin calls `register-new-commodity()` with details.

3. **Funding**
    - Traders call `deposit-funds-to-escrow()` to add STX to their trading balance.

4. **Trading**
    - Call `execute-trade()` with commodity ID, quantity, and a unique position ID.
    - Position details are stored and escrow balance is deducted.

5. **Closing Position**
    - Call `close-trading-position()` to calculate settlement and return STX to escrow.

6. **Withdrawal**
    - Call `withdraw-funds-from-escrow()` to retrieve unused escrow balance.

---

## Security Checks

- Role-based access for admin/oracle actions.
- Price and quantity validations to avoid invalid trade entries.
- Escrow balance checks prevent overdrafts.
- Trading cannot proceed if the market is disabled.

---

##  Development Notes

- Written in **Clarity**, the smart contract language for **Stacks**.
- `block-height` is used as a timestamp substitute for entry recording.
- Oracle logic can be extended to integrate with off-chain feeds.
- Future improvements: real-time P&L, price updates by oracle, automated liquidation, multi-token support.

---

##  Files

```
📦 commodities-trading-contract
 ┣ 📜 commodities-trading.clar      # Main smart contract
 ┣ 📜 README.md                      # This documentation
```

---

##  Test Cases to Consider

- Register commodity with invalid values → should fail.
- Execute trade with insufficient escrow balance → should fail.
- Close trade and validate STX returned to escrow.
- Disable trading and attempt trade → should fail.

---
