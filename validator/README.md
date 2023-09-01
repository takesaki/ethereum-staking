# docker compose による　ethereum validatorの構築
- ここに書いてあるのはgoerli版です、適宜読み替えてください  
- docker compose (on linux) コマンドが使える環境が必要です、事前にご準備ください

## 準備
1. テンプレートをコピーし、そのディレクトリに移動します
    ```sh
    $ git clone https://github.com/takesaki/ethereum-staking.git
    $ cd ethereum-staking/validator
    ```

1. genesis.sszをあなたのディレクトリに持ってきます
    ```sh
    $ curl -L https://github.com/eth-clients/eth2-networks/raw/master/shared/prater/genesis.ssz -o genesis.ssz
    ```

## create keys
validator_keysディレクトリ内にdeposit.jsonとkeysrore.jsonを作ることが目的です
そのため、セキュリティを考えると別のマシンで実施すべきで、その場合上記２つのファイルを持ってくることでこれはスキップできます
```sh
$ cd {your validator directory}
$ docker run -it --rm -v $(pwd)/validator_keys:/app/validator_keys ethereum/staking-deposit-cli new-mnemonic \
--num_validators 1 \
--chain goerli \
--eth1_withdrawal_address {{あなたのウォレットアドレス}}
```

## import keys
1. validator_keysディレクトリ内のkeysrore.jsonから鍵をインポートします、これによりwalletディレクトリ内に情報が書き込まれます
    ```
    $ docker run -it -v $(pwd)/validator_keys:/keys \
        -v $(pwd)/wallet:/wallet \
        gcr.io/prysmaticlabs/prysm/validator:stable \
        --name validator \
        --accept-terms-of-use \
        accounts import --keys-dir=/keys --wallet-dir=/wallet
    ```

1. ウォレットパスワードを設定します、以下のファイルにパスワードだけを書き込みます  
    `wallet-password.txt`

## start validator
- 必要に応じて`config/validator.yaml`を編集します
    - 例えばEL報酬振込先（suggested-fee-recipient）とかです、私にくれるならそのままでOKです
- さぁ起動です、もちろんdepositとかは終わってますよね？
```sh
$ docker compose up -d
```





