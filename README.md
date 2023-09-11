# az app service

## まずは簡単なアプリをデプロイしてみる

azure cli をインストール (for ubuntu linux)
```
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

version
```
$ az version 
{
  "azure-cli": "2.52.0",
  "azure-cli-core": "2.52.0",
  "azure-cli-telemetry": "1.1.0",
  "extensions": {}
}
```

ログイン
```
$ az login
$ az account show -o table 

EnvironmentName    HomeTenantId                          IsDefault    Name                    State    TenantId
-----------------  ------------------------------------  -----------  ----------------------  -------  ------------------------------------
AzureCloud         acc73da2-ad2d-4d42-b84d-bbdee1c8d551  True         AZ Learn 2021 従量課金  Enabled  acc73da2-ad2d-4d42-b84d-bbdee1c8d551
```


rg 作成
```
$ az group create --name az-appservice-2023-rg --location japaneast
```

app serviee plan 作成
```
$ az appservice plan create --name az-appservice-2023-plan --resource-group az-appservice-2023-rg --sku FREE
```

webapp 作成
```
$ az webapp create --name az-appservice-2023-webapp --resource-group az-appservice-2023-rg --plan az-appservice-2023-plan
```

node app 作成 (deployするアプリをwebアプリ(frontend(配信する)とbackend)作成 node.js(express))
```
$ node -v 
v18.17.0
npx express-generator myExpressApp --view ejs
cd myExpressApp/ | npm install
DEBUG=myexpressapp:* npm start
```

```
$ pwd 
/home/user01/az-appservice-basic-knowledge/myExpressApp
# node 18を指定する
az webapp up  --sku F1 --name 1st-appservice18 --os-type Linux --runtime "node|18-lts" -g az-appservice-2023-rg
```

# 使い終わったら削除
```
$ az group delete --name az-appservice-2023-rg
```

# まとめ　app serviceの外部接続設定関連

app service Preeプランはvnet統合ができない

app serviceの public access はデフォルトで許可されている

app serviceの public access を許可して、ルール設定をしてもデプロイなどはできてしまう。

app serviceの public access は拒否した場合は、VNet経由でのみアクセスできるようになる



# 踏み台の利用 (bastion or vm)

Azureの踏み台サーバーとして、Azure Bastionを利用する方法と、Azure VMを利用する方法がある。

## Azure Bastionを利用する方法

vnetに専用のSubnetが必要（AzureBastionSubnet）（/26以上のサイズが必要）
例: AzureBastionSubnet 10.0.1.0/24 (251台分)

## 気になる料金は

basic: ¥27.801 / 時間 (およそ 1ヶ月で¥20,000)
standard: ¥42.433 / 時間 (およそ 1ヶ月で¥30,000)

※vmとは別で上記の料金が発生するので少しお高めです。

## Azure VMを利用する方法

