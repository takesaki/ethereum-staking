# ethereum node(EL Client(geth)/CL Client(prysm)/MEV-boost)を建てる

## 準備
1. git cloneし、ディレクトリに移動します
    ```sh
    git clone https://github.com/takesaki/ethereum-staking.git
    cd ethereum-staking/node
    ```

1. EL/CL間通信時の認証情報（jwt secret）ファイル`jwtsecret`を作成します  

    [参考](https://docs.prylabs.network/docs/execution-node/authentication)
    ```
    openssl rand -hex 32 | tr -d "\n" > "jwtsecret"
    ```
    作成した`jwtsecret`はBeacon Nodeからも参照されます

## 起動

1. 以下のコマンドで起動します
    ```sh
    docker compose --env-file mainnet.env up -d
    ```
1. 起動した後はエラーログがないか確認しておきましょう
    ```sh
    docker compose logs -f
    ```

### testnet(goerli)で動かす場合は
- `conf.d/eth2/beacon.yaml`の`checkpoint-sync-url`を変更
- 起動オプションを変更（mainnet.envをgoerli.envに）
    ```sh
    docker compose --env-file goerli.env up -d
    ```

## TIPS　　
- 4000番ポートにvalidatorはアクセスしてきますFWなどは開けておきましょう

### gethのAPIへの接続方法

各コンテナに`exec`するのは、監査的に良くないのでアクセス用のコンテナを建てる
```sh
docker run --rm -it \
  --network node_default \
  ethereum/client-go:stable \
  attach http://geth:8545
```

取得したConsoleで色々試せます
- Sync状況  `eth.syncing`  
結果がfalseならSyncできてる
- peer接続数  `net.peerCount`

### pryzmのAPIの実行方法
```sh
# 動作確認用コンテナを立てる
docker run --rm -it \
  --network node_default \
  alpine \
  /bin/sh
# 起動したコンテナに必要なコマンドの追加
apk add curl jq
# ELとの接続状態を確認
curl http://beacon:3500/eth/v1/node/syncing | jq
# BeaconCahinの状態を確認
curl http://beacon:8080/healthz
```

## Trouble Shooting
- 同期が終わらない  
  Execution Clientが先に同期完了している必要があるため、Execution Client起動直後はしばらく時間がかかります
