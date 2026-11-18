# Ethernaut CTF Solutions

My solutions to OpenZeppelin's [Ethernaut](https://ethernaut.openzeppelin.com/) CTF, levels 0–29, written with [Foundry](https://book.getfoundry.sh/) in spring 2023.

Each exploit is a contract in `src/`, deployed against my level instance on Sepolia by a script in `script/`. The harder levels also have a fork test in `test/`. The simplest levels were solved straight from the browser console.

Instance addresses are hardcoded: to reuse a solution, replace them with your own.

| # | Level | Bug | Solution |
|---|---|---|---|
| 0 | Hello Ethernaut | Tutorial: the password is stored in the contract | browser console |
| 1 | Fallback | `receive()` hands ownership to anyone who has made a contribution | browser console |
| 2 | Fallout | The "constructor" `Fal1out()` is misspelled, so it is a public function anyone can call | browser console |
| 3 | Coin Flip | The flip comes from `blockhash`: a contract computes the same result in the same block | [exploit](src/03-CoinFlip.sol) · [script](script/03-CoinFlip.s.sol) |
| 4 | Telephone | Authorization with `tx.origin != msg.sender` passes when the call goes through a contract | [exploit](src/04-Telephone.sol) · [script](script/04-Telephone.s.sol) |
| 5 | Token | Unchecked arithmetic (Solidity 0.6): transferring more than the balance underflows | browser console |
| 6 | Delegation | The fallback `delegatecall`s into `Delegate`, so calling `pwn()` overwrites the owner | browser console |
| 7 | Force | `selfdestruct` sends ether to a contract that has no way to receive it | [exploit](src/07-Force.sol) · [script](script/07-Force.s.sol) |
| 8 | Vault | `private` storage is still readable: the password is in slot 1 | browser console |
| 9 | King | The prize is paid with `transfer`: a king that reverts on receive can never be dethroned | [exploit](src/09-King.sol) · [script](script/09-King.s.sol) |
| 10 | Re-entrancy | `withdraw` sends ether before updating the balance | [exploit](src/10-Reentrancy.sol) · [script](script/10-Reentrancy.s.sol) |
| 11 | Elevator | `isLastFloor()` is asked to the caller, who answers differently on the two calls | [exploit](src/11-Elevator.sol) · [script](script/11-Elevator.s.sol) |
| 12 | Privacy | `private` data is readable from storage: the key is slot 5 cast to `bytes16` | browser console |
| 13 | Gatekeeper One | A `gasleft() % 8191` check and a key built by type-casting `tx.origin` | [exploit](src/13-GatekeeperOne.sol) · [script](script/13-GatekeeperOne.s.sol) · [test](test/13-GatekeeperOne.t.sol) |
| 14 | Gatekeeper Two | `extcodesize` is 0 while a constructor runs; the key is the XOR inverse of the caller's hash | [exploit](src/14-GatekeeperTwo.sol) · [script](script/14-GatekeeperTwo.s.sol) · [test](test/14-GatekeeperTwo.t.sol) |
| 15 | Naught Coin | The timelock covers `transfer` only: `transferFrom` is still open | [exploit](src/15-NaughtCoin.sol) · [script](script/15-NaughtCoin.s.sol) · [test](test/15-NaughtCoin.t.sol) |
| 16 | Preservation | `delegatecall` to a library with a different storage layout: overwrite the library address, then the owner | [exploit](src/16-Preservation.sol) · [script](script/16-Preservation.s.sol) · [test](test/16-Preservation.t.sol) |
| 17 | Recovery | The lost token's address can be recomputed from its factory, and its public `destroy()` sends the ether anywhere | [exploit](src/17-Recover.sol) · [script](script/17-Recover.s.sol) |
| 18 | MagicNumber | A 10-byte runtime written by hand in bytecode that returns 42 | [exploit](src/18-MagicNumber.sol) · [script](script/18-MagicNumber.s.sol) |
| 19 | Alien Codex | `retract()` underflows the array length, so `revise()` can write any storage slot, owner included | [exploit](src/19-AlienCodex.sol) · [script](script/19-AlienCodex.s.sol) · [test](test/19-AlienCodex.t.sol) |
| 20 | Denial | The call to the partner forwards all the gas: a partner that burns it blocks every withdrawal | [exploit](src/20-Denial.sol) · [script](script/20-Denial.s.sol) |
| 21 | Shop | `price()` is asked twice, and the buyer can read `isSold` to answer less the second time | [exploit](src/21-Shop.sol) · [script](script/21-Shop.s.sol) |
| 22 | Dex | The swap price comes from pool balances with integer division: alternating swaps drain a token | [exploit](src/22-Dex.sol) · [script](script/22-Dex.s.sol) · [test](test/22-Dex.t.sol) |
| 23 | Dex Two | `swap` does not check the tokens: a worthless token buys both reserves | [exploit](src/23-DexTwo.sol) · [script](script/23-DexTwo.s.sol) · [test](test/23-DexTwo.t.sol) |
| 24 | Puzzle Wallet | Proxy and wallet share storage slots (`pendingAdmin`/`owner`, `admin`/`maxBalance`), and a nested `multicall` counts one `msg.value` twice | [exploit](src/24-PuzzleWallet.sol) · [script](script/24-PuzzleWallet.s.sol) · [test](test/24-PuzzleWallet.t.sol) |
| 25 | Motorbike | The UUPS implementation was never initialized: take it over and `selfdestruct` it | [exploit](src/25-Motorbike.sol) · [script](script/25-Motorbike.s.sol) · [test](test/25-Motorbike.t.sol) |
| 26 | DoubleEntryPoint | Defensive level: a Forta bot that raises an alert when `CryptoVault` is the original sender of a delegated transfer | [bot](src/26-DoubleEntryPoint.sol) · [script](script/26-DoubleEntryPoint.s.sol) · [test](test/26-DoubleEntryPoint.t.sol) |
| 27 | Good Samaritan | A `NotEnoughBalance()` error means "wallet empty, send everything", but the receiver's `notify()` can raise it too | [exploit](src/27-GoodSamaritan.sol) · [script](script/27-GoodSamaritan.s.sol) · [test](test/27-GoodSamaritan.t.sol) |
| 28 | Gatekeeper Three | A misspelled `construct0r()`, a password that is just the `block.timestamp` of the same transaction, and an owner that refuses ether | [exploit](src/28-GatekeeperThree.sol) · [script](script/28-GatekeeperThree.s.sol) · [test](test/28-GatekeeperThree.t.sol) |
| 29 | Switch | `flipSwitch` checks the selector at a fixed calldata position, but the `bytes` offset can point somewhere else | [exploit](src/29-Switch.sol) · [script](script/29-Switch.s.sol) · [test](test/29-Switch.t.sol) |

## Usage

```bash
git clone --recursive https://github.com/fedebianu/ethernaut-ctf-solutions
cd ethernaut-ctf-solutions
forge build
```

Set `SEPOLIA_RPC_URL`, `PRIVATE_KEY` and `ETHERSCAN_API_KEY`, and put your instance address in the script. Tests also need `ETH_FROM`, your player address.

```bash
# try the exploit on a Sepolia fork
forge test --match-path test/<level>.t.sol --fork-url $SEPOLIA_RPC_URL -vvvv

# run it on Sepolia
forge script script/<level>.s.sol --rpc-url sepolia --broadcast --verify -vvvv
```
