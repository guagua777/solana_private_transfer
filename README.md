# solana_private_transfer

## 相关资料
1. https://www.bilibili.com/video/BV1gG4y117vX/
2. https://github.com/solana-foundation/noir-examples
3. https://noir-lang.org/
4. https://github.com/reilabs/sunspot
5. https://eprint.iacr.org/2016/260
6. 



## 流程
1. 使用Noir编写电路逻辑
2. 使用nargo compile将编写的电路逻辑即行编译，编译为ACIR（一种IR），生成一个json文件
---------
3. 使用sunspot compile编译ACIR为.ccs文件，(Transform ACIR to Gnark constraint system)
4. 使用sunspot setup生成.pk和.vk文件，(Generate proving key and verifying key)
5. 最终会生成以下4个文件：
   ```
   $tree
    .
    ├── XXXXXX.ccs
    ├── XXXXXX.json
    ├── XXXXXX.pk
    └── XXXXXX.vk

    1 directory, 4 files
   ```
---------
6. 部署：sunspot deploy XXXXXX.vk，会生成一个XXXXXX.so文件和XXXXXX-keypair.json文件（该步骤中需要使用到verifier-bin，所以要设置好verifier-bin的路径，该路径为sunspot的安装目录下的gnark-solana/crates/verifier-bin，可根据自己的实际情况修改）
7. 使用solana program deploy XXXXXXX.so


## 实例
1. 假设当前电路路径为~/private-transfers/main/circuits/withdrawal
   ```
   $ tree
    .
    └── withdrawal
        ├── Nargo.toml
        ├── src
        │   ├── main.nr
        │   └── merkle_tree.nr
        └── target
            ├── withdrawal.ccs
            ├── withdrawal.json
            ├── withdrawal-keypair.json
            ├── withdrawal.pk
            ├── withdrawal.so
            └── withdrawal.vk

    4 directories, 9 files
   ```
2. 在当前目录下~/private-transfers/main/circuits/withdrawal，执行nargo compile编译命令，生成target/withdrawal.json文件
3. 执行sunspot compile生成target/withdrawal.ccs文件
4. 执行sunspot setup生成target/withdrawal.pk和target/withdrawal.vk文件
5. 执行sunspot deploy target/withdrawal.vk，会生成target/withdrawal.so文件和target/withdrawal-keypair.json文件
6. 使用solana program deploy target/withdrawal.so



## How it Works

1. **Deposit**: User deposits SOL into a shared pool. A commitment `hash(nullifier, secret, amount)` is added to a Merkle tree.

2. **Withdraw**: User generates a ZK proof showing they know a valid commitment without revealing which one. The proof is verified onchain via Sunspot.

3. **Privacy**: The link between deposit and withdrawal is broken. Only the amount is visible (variable amounts trade privacy for flexibility).

## Project Structure

```
├── circuits/
│   ├── hasher/          # Computes commitment and nullifier hash
│   ├── merkle-hasher/   # Computes Merkle root for a leaf
│   └── withdrawal/      # Main ZK proof circuit
├── anchor/
│   └── programs/
│       └── private_transfers/  # Solana program
├── backend/             # API for proof generation
└── frontend/            # React UI
```
