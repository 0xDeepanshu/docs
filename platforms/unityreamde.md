# UNITY SOLANA x402 Payment

# Unity Solana x402 Payment Flow: Documentation

This document outlines the complete, step-by-step process of connecting a Solana wallet in a Unity application and executing a 402-compliant "payment-required" API request.

The flow is handled by the `Gx402Manager.cs` script and can be broken down into five main stages.

### Step 1: Wallet Connection

This stage handles the initial connection to the user's Solana wallet (e.g., Phantom, Solflare).

1. **User Action:** The user clicks a "Connect Wallet" button in the Unity UI.
2. **Trigger:** This button calls the public `ConnectWallet()` method.
3. **Async Call:** `ConnectWallet()` immediately calls `ConnectWalletAsync().Forget()` to run the connection logic without blocking the main game thread.
4. **SDK Call:** `ConnectWalletAsync()` calls `Web3.Instance.LoginWalletAdapter()`. This is the key function from the MagicBlocks/Solana SDK.
5. **Wallet Prompt:** The SDK opens the user's installed wallet adapter in their browser, prompting them to connect their wallet to the Unity application.
6. **Confirmation:** Once the user approves the connection, the `LoginWalletAdapter()` call returns an `Account` object. The static `Web3.Account` property is now populated with the user's wallet information (most importantly, their `PublicKey`).
7. **UI Update:** The `OnWalletStateChanged` event listener fires, sees that `Web3.Account` is no longer null, and calls `UpdateStatus()` to display the connected wallet address in the UI.

### Step 2: Initiating the API Call

This stage begins when the user wants to access the protected API endpoint.

1. **User Action:** The user clicks a "Call API" button.
2. **Trigger:** This button calls the public `CallApiButton()` method.
3. **Optimized Flow:** This method calls `HandleApiCallOptimized().Forget()`, which is designed to perform the payment in a single, robust flow using pre-defined constants.
4. **Wallet Check:** The first step inside `HandleApiCallOptimized()` is to check `if (!CheckWallet())`. This ensures `Web3.Account` is valid before proceeding.
5. **Payer Identified:** The script gets the user's public key via `PublicKey payerPublicKey = Web3.Account.PublicKey;`.

### Step 3: Building the Transaction

This is the most critical stage, where the payment transaction is constructed. This happens inside `BuildUsdcTransaction()`.

1. **Get Blockhash:** The script first fetches a recent blockhash using `Web3.Rpc.GetLatestBlockHashAsync()`. All Solana transactions require a recent blockhash to be valid.
2. **Derive ATAs:** The script calculates two important addresses (Associated Token Accounts):
    - `payerTokenAccount`: The user's (payer's) own USDC token account address.
    - `recipientTokenAccount`: The server's (recipient's) USDC token account address.
3. **Assemble Instructions:** The script builds a list of three instructions that will be executed in order:
    - **Instruction 0: Create Payer ATA:**
        - `AssociatedTokenAccountProgram.CreateAssociatedTokenAccount(payer, owner: payer, ...)`
        - This instruction ensures the user's USDC token account exists.
        - **Crucially, `idempotent: true` is set.** This means the instruction will *succeed* if the account already exists and *create it* if it doesn't. This prevents the transaction from failing if the user has paid before.
    - **Instruction 1: Create Recipient ATA:**
        - `AssociatedTokenAccountProgram.CreateAssociatedTokenAccount(payer, owner: recipient, ...)`
        - This instruction ensures the *server's* USDC token account exists.
        - This also uses `idempotent: true`. The `payer` (the user) pays the one-time rent to create this account if it's the first time anyone has ever sent USDC to the recipient. This is the standard, robust way to send tokens.
    - **Instruction 2: The Transfer:**
        - `TokenProgram.TransferChecked(...)`
        - This is the actual payment. It instructs the Solana program to move the `AMOUNT_REQUIRED` of USDC from the `payerTokenAccount` (which Instruction 0 guaranteed exists) to the `recipientTokenAccount` (which Instruction 1 guaranteed exists).
4. **Finalize Transaction:** The three instructions are bundled into a new `Transaction` object. The `FeePayer` is set to the user (`payerPublicKey`), and the `RecentBlockHash` is set. This complete, multi-instruction transaction is then returned.

### Step 4: Signing and Sending the Transaction

This stage executes the transaction built in Step 3.

1. **Sign Prompt:** Back in `HandleApiCallOptimized()`, the script calls `Web3.Wallet.SignTransaction(transaction)`. This prompts the user in their wallet to approve the entire 3-instruction transaction (Create Payer ATA, Create Recipient ATA, and Transfer).
2. **Send to Network:** Once the user signs, the script gets the `signedTx` and immediately sends it to the Solana network using `Web3.Rpc.SendTransactionAsync(signedTx.Serialize())`.
3. **Get Signature:** The result of this call, if successful, is the transaction `signature` (the TX ID). This string is the user's "proof of payment."
4. **Confirm Transaction:** The script then calls `Web3.Rpc.ConfirmTransaction(signature, ...)` to wait for the Solana network to fully confirm the payment. This is a crucial step to ensure the server can find the payment when it checks.

### Step 5: Verifying the Payment (The "402" Part)

This final stage proves to the server that the payment was made.

1. **Trigger:** After the transaction is confirmed, `PostWithSignature()` is called, passing in the `signature`.
2. **Create POST Request:** This function creates a new `UnityWebRequest` (a POST request) to the `apiUrl`.
3. **Add 402 Header:** This is the key to the entire flow. The script sets the `X-402-Payment` header to the `signature` string:
    - `request.SetRequestHeader("X-402-Payment", signature);`
4. **Send Request:** The script sends the POST request to the server.
5. **Server-Side Logic:** The server receives this request. It sees the `X-402-Payment` header, extracts the signature, and queries the Solana blockchain for that transaction. It verifies that the transaction is valid, confirmed, and contains the correct payment (right amount, right mint, right recipient).
6. **Access Granted:** Because the server finds the valid payment, it bypasses its "402 Payment Required" block. It processes the user's API request (in this case, the `RequestBody` JSON) and returns a `200 OK` status with the API's real data.
7. **UI Update:** The script receives the `200 OK` response, logs the success, and updates the UI status to "✅ Payment Verified! Access Granted."