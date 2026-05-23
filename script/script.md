```
$ cd /tmp
$ git clone 

# 修改为最新的目录
$ cd /tmp/solana_private_transfer/main/circuits/withdrawal
$ nargo compile
$ sunspot compile target/withdrawal.json
$ sunspot setup target/withdrawal.ccs
# 修改为自己的安装路径
$ export GNARK_VERIFIER_BIN="/home/ubuntu/src/sunspot/gnark-solana/crates/verifier-bin"
$ sunspot deploy target/withdrawal.vk
$ solana-test-validator 启动测试网络
# solana config get 查看当前网络配置
$ solana program deploy target/withdrawal.so
# 获取自己的部署的程序id
# PROGRAM_ID = Cpjk1AcwHQ4wS9z3pBmKHCBaCJw1of8dhqT5Qyeu57iU
# Replace it in the anchor/programs/private_transfers/src/lib.rs SUNSPOT_VERIFIER_ID
# Replace it in the anchor/Anchor.toml sunspot_verifier，[[test.genesis]] address
# Replace it in the frontend/src/constants.ts SUNSPOT_VERIFIER_ID

# Change the url in anchor/package.json ANCHOR_PROVIDER_URL
# Change the url in backend/src/server.ts DEVNET_ENDPOINT
# Change the url in frontend/src/constants.ts DEVNET_ENDPOINT

$ cd /tmp/solana_private_transfer/main/anchor
# 修改 Anchor.toml文件中的[provider] cluster为Localnet
# 修改anchor cli的版本 cargo install --git https://github.com/coral-xyz/anchor --tag v1.0.0-rc.3 anchor-cli
# 修改项目中的anchor版本，包括Cargo.toml, Anchor.toml,
$ anchor keys sync
$ anchor build
$ anchor program deploy --provider.cluster localnet
# PROGRAM_ID = 36bU2Ecaj64d95PxuNJ9mRjMacPXh6mDJjpggySpV3Xtb
# Replace it in backend/src/server.ts PROGRAM_ID
$ bun install
$ bun run init-pool

# 修改为最新的目录
$ cd /tmp/solana_private_transfer/main/backend
$ bun install
$ bun run dev

$ cd /tmp/solana_private_transfer/main/frontend
$ bun install
$ bun run dev


前端和后端是什么关系
0值hash是怎么计算的

# 本地保存以下信息
# 这些信息是什么？
{
"nullifier":"18538784216617782925537835875776707564723275923110950058290420810358138982404",
"secret":"6833189650380974204041205421620364696749158841619621941741044835965918520917",
"amount":"100000000",
"commitment":"0x1a51e350c87050f0ebacab6ed6fb40773673be6dd4d8c8e755a2ce8c2248d95e",
"nullifierHash":"0x218294d5aaf2633d5e8723abd24ed581924a80bb6c462d02ff5d151bd1633ee5",
"merkleRoot":"0x1dd95f87a6ccc5ca17c950b2f5c61b370f15a424ae1cddf1adcce72974f8c6cf",
"leafIndex":0,
"timestamp":1779012705050
}
```
