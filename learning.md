# Anchor test
Run the anchor test with skip validator flag to use exsting validator.

```shell

anchor test --skip-local-validator

```

#Solana Anchor Learning.

The error indicates that your program deployment is failing because the account lacks sufficient SOL. To avoid re-funding every time and use a consistent, reusable account, follow these steps:

### Solution: Use a Persistent Local Wallet for Testing

1. **Create a Persistent Keypair**:
   - Generate a new keypair file that will serve as your persistent local wallet for all tests:
     ```bash
     solana-keygen new --outfile ~/.config/solana/test-wallet.json
     ```
   - This command creates a wallet file located at `~/.config/solana/test-wallet.json`.

2. **Add Funds to This Wallet**:
   - Fund this new wallet with SOL using a faucet (for testnet) or through your own devnet wallet.
   - If you're using the Solana testnet or devnet, you can request airdrops:
     ```bash
     solana airdrop 2 ~/.config/solana/test-wallet.json --url https://api.devnet.solana.com
     ```
   - You may need to repeat this command multiple times to accumulate enough SOL for multiple tests.

3. **Set the Wallet as the Default for Anchor**:
   - Configure Solana to use this wallet by default:
     ```bash
     solana config set --keypair ~/.config/solana/test-wallet.json
     ```
   - Verify this setting by running:
     ```bash
     solana config get
     ```

4. **Update Anchor.toml**:
   - In your `Anchor.toml` file, add the `[test]` section to specify the wallet path:
     ```toml
     [test]
     wallet = "~/.config/solana/test-wallet.json"
     ```
   - This ensures that Anchor uses the same wallet for each test, eliminating the need for new funding each time.

5. **Run the Test Again**:
   - With this configuration, your tests should now use the persistent wallet with sufficient funds:
     ```bash
     anchor test
     ```

6. **Optional: Increase SOL Balance if Needed**:
   - If you see insufficient funds errors again, simply airdrop more SOL to this wallet.

By setting a static wallet, you avoid creating new accounts and reduce the need for frequent funding, streamlining the testing process. Let me know if this resolves the issue!



# Fund solana newly created 

On Solana’s localnet, you can use the `solana-test-validator`’s built-in airdrop feature to fund accounts directly. Here’s how:

1. **Start the Local Test Validator**:
   - Run `solana-test-validator` if it’s not already running:
     ```bash
     solana-test-validator --reset
     ```
   - This command will start the validator with a fresh ledger, enabling you to work in a controlled local environment.

2. **Airdrop SOL to Your Local Wallet**:
   - To airdrop SOL to your wallet on the localnet, use:
     ```bash
     solana airdrop 10 <your_wallet_address> --url http://localhost:8899
     ```
   - Replace `<your_wallet_address>` with your wallet’s public key or use your default keypair:
     ```bash
     solana airdrop 10 --url http://localhost:8899
     ```

   This command will add 10 SOL to the specified account. You can change the amount as needed.

3. **Verify Balance**:
   - After the airdrop, check your wallet balance to ensure the funds were received:
     ```bash
     solana balance --url http://localhost:8899
     ```

This method is fast and allows you to repeatedly fund your test account without relying on external sources, ensuring you have enough SOL for testing on your localnet.