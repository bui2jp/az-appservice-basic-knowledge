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
az webapp up  --sku F1 --name 1st-appservice --os-type Linux
```

node 18を指定する
```
az webapp up  --sku F1 --name 1st-appservice18 --os-type Linux --runtime "node|18-lts"
```

# 使い終わったら削除
```
$ az group delete --name az-appservice-2023-rg
```

# まとめ　app serviceの外部接続設定関連

app serviceの public access はデフォルトで許可されている

app serviceの public access を許可して、ルール設定をしてもデプロイなどはできてしまう。

app serviceの public access は拒否すると、VNet経由でのみアクセスできるようになる



