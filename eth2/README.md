# docker compose による Consensus Client(`prysm`)の構築

## 準備
1. EC/CC間通信時の認証情報（jwt secret）ファイル`jwtsecret`をこのディレクトリにコピーします  

    参考：[prysm公式 - 認証](https://docs.prylabs.network/docs/execution-node/authentication)
    ```
    $ cp -p ../eth1/jwtsecret ./
    ```

## 設定  

1. `config/beacon.yaml`を編集します  
    主な編集ポイント
    - `execution-endpoint` ELのエンドポイントのIPを指定
    - `suggested-fee-recipient` で報酬込先の指定  

    参考：[prysm公式 - オプション設定](https://docs.prylabs.network/docs/prysm-usage/parameters#beacon-node-flags)

1. 次に進む前にお茶を飲みましょう

## 起動

1. 以下のコマンドで起動します
    ```sh
    $ docker compose up -d
    ```
1. 起動した後はエラーログがないか確認しておきましょう
    ```sh
    $ docker compose logs -f
    ```

## Trouble Shooting
- 同期が終わらない  
  Execution Clientが先に同期完了している必要があるため、Execution Client起動直後はしばらく時間がかかります


## TIPS　　
    
    4000番ポートにvalidatorはアクセスしてきますFWなどは開けておきましょう

