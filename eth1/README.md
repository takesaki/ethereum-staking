# docker compose による`geth`の構築

## 準備
1. EL/CL間通信時の認証情報（jwt secret）ファイル`jwtsecret`を作成します  

    [参考](https://docs.prylabs.network/docs/execution-node/authentication)
    ```
    $ cd eth1
    $ openssl rand -hex 32 | tr -d "\n" > "jwtsecret"
    ```
    作成した`jwtsecret`はBeacon Nodeでも使用します
## 起動

1. 以下のコマンドで起動します
    ```sh
    $ docker compose up -d
    ```
1. 起動した後はエラーログがないか確認しておきましょう
    ```sh
    $ docker compose logs-f
    ```

## Trouble Shooting
Not yet.


## TIPS
- geth consoleの取得
    ```
    $ docker compose exec geth /bin/sh
    ↓コンテナの中で
    # geth attach http://127.0.0.1:8545
    ```

    取得したConsoleで色々試せます
    - Sync状況  `eth.syncing`  
    結果がfalseならSyncできてる
    - peer接続数  `net.peerCount`


- 設定の出力  
    ↓コンテナの中で
    ```
    geth --goerli dumpconfig > geth-config.toml
    ```
