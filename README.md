# **GratiFi Smart Contract**  
*Token-Based Loyalty Program on Stacks Blockchain*

---

## **Overview**  
**GratiFi** is a decentralized loyalty program built on the Stacks blockchain. It allows businesses to reward loyal customers with tokenized loyalty points, which can be redeemed for various rewards. The contract ensures secure and transparent transactions, enabling users to earn, transfer, and redeem points seamlessly.

---

## **Features**  

- **Tokenized Loyalty System:**  
  Customers earn and accumulate loyalty tokens called **LoyaltyTokens (LYT)** for their interactions and purchases.

- **Secure Transfers:**  
  Users can transfer tokens between accounts, fostering a decentralized exchange of rewards.

- **Customizable Rewards:**  
  Businesses can add rewards with unique names, costs, and availability, providing a personalized loyalty experience.

- **Transparent Redemption:**  
  Customers can redeem their tokens for available rewards, with all transactions recorded on the blockchain.

---

## **Contract Structure**  

### **Error Constants**  
| Error Code | Description                  |
|------------|------------------------------|
| `ERR-NOT-AUTHORIZED (100)`  | Caller is not authorized to perform the action. |
| `ERR-INSUFFICIENT-BALANCE (101)` | Insufficient balance to complete the transaction. |
| `ERR-INVALID-AMOUNT (102)` | Invalid token or reward amount. |
| `ERR-REWARD-NOT-AVAILABLE (103)` | Requested reward is not available. |

---

### **Data Structures**  
1. **Balances**  
   Tracks the token balance of each user.  
   ```clarity
   (define-map balances principal uint)
   ```

2. **Rewards**  
   Stores details of available rewards.  
   ```clarity
   (define-map rewards uint {name: (string-ascii 50), cost: uint, available: uint})
   ```

3. **Contract Variables**  
   - `token-name` – The name of the token (`LoyaltyToken`).  
   - `token-symbol` – The symbol of the token (`LYT`).  
   - `owner` – The contract owner who can manage rewards and mint tokens.  

---

## **Functions**  

### **Read-Only Functions**  

1. **`(get-name)`**  
   Returns the token name.  
   ```clarity
   (define-read-only (get-name) (ok (var-get token-name)))
   ```

2. **`(get-symbol)`**  
   Returns the token symbol.  
   ```clarity
   (define-read-only (get-symbol) (ok (var-get token-symbol)))
   ```

3. **`(get-balance (account principal))`**  
   Returns the balance of a specified account.  
   ```clarity
   (define-read-only (get-balance (account principal)) 
     (ok (default-to u0 (map-get? balances account))))
   ```

4. **`(get-reward (reward-id uint))`**  
   Retrieves the details of a reward by its ID.  
   ```clarity
   (define-read-only (get-reward (reward-id uint)) (map-get? rewards reward-id))
   ```

---

### **Public Functions**  

1. **`(transfer (amount uint) (sender principal) (recipient principal))`**  
   Transfers tokens from one account to another.  
   - **Errors:**  
     - `ERR-NOT-AUTHORIZED` if the caller is not the sender.  
     - `ERR-INSUFFICIENT-BALANCE` if the sender has insufficient tokens.  

2. **`(mint (amount uint) (recipient principal))`**  
   Mints new tokens and assigns them to the specified recipient.  
   - **Authorized:** Only the contract owner can mint tokens.  

3. **`(add-reward (name string-ascii) (cost uint) (available uint))`**  
   Adds a new reward to the system.  
   - **Authorized:** Only the contract owner can add rewards.  

4. **`(redeem-reward (reward-id uint))`**  
   Allows users to redeem tokens for a specified reward.  
   - **Errors:**  
     - `ERR-INSUFFICIENT-BALANCE` if the user does not have enough tokens.  
     - `ERR-REWARD-NOT-AVAILABLE` if the reward is out of stock.  

---

## **Usage Guide**  

### **1. Deploying the Contract**  
1. Deploy the `GratiFi` contract on the Stacks blockchain.  
2. The deploying account becomes the contract owner.

---

### **2. Minting Tokens**  
The contract owner can mint new loyalty tokens using the `mint` function:  
```clarity
(mint u1000 'SP3FBR2AGK71QQCPBBFAKXPTZSDZ5WYB6N4XVESYZ)
```

---

### **3. Adding Rewards**  
Add a reward that customers can redeem:  
```clarity
(add-reward "Free Coffee" u50 u100) ;; 50 tokens for a coffee, 100 available
```

---

### **4. Transferring Tokens**  
Users can transfer tokens to others:  
```clarity
(transfer u100 'SP3FBR2AGK71QQCPBBFAKXPTZSDZ5WYB6N4XVESYZ 'SP3WPYF4XJGM5G4Q2BZJ5SMX7Q98A4J4KSV6PPTX)
```

---

### **5. Redeeming Rewards**  
Users can redeem tokens for a reward:  
```clarity
(redeem-reward u0) ;; Redeems reward with ID 0
```

---

## **Security Considerations**  
- Only the contract owner can mint tokens and add rewards.  
- Transfers and redemptions are validated to ensure sufficient balance and availability.  
- All transactions are recorded immutably on the blockchain for transparency and accountability.

---

## **Future Enhancements**  
- **Reward Expiry:** Implement expiration dates for rewards.  
- **Dynamic Token Supply:** Introduce mechanisms for burning tokens.  
- **Multisig Ownership:** Allow multiple owners to manage the contract collaboratively.  
- **NFT Rewards:** Expand the system to include NFTs as redeemable rewards.

---

## **License**  
This project is licensed under the MIT License.