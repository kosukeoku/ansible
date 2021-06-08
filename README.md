# ansible
rails,reactの環境構成ツール


# Usage
```
#接続先の設定をする
sudo vim ~/.ssh/config
---config---
Host <適当な名前をつける(inventoryファイルの名前と同じにする)>
  User ec2-user
  HostName <ec2インスタンスのパブリックIP>
  IdentityFile <ssh認証するためのキーの場所を入力>
  StrictHostKeyChecking no
------------

#暗号化ファイルのためのパスワードを作成する
sudo vim ~/.vault_pass
---vault_pass----
#private.ymlのpasswardを記入する
-----------------

#暗号化ファイルを作成
sudo vim  private.yml
---private.yml---
git_secretkey: <SHHの秘密鍵名>
app_backend: <SSHでのバックエンドのリポジトリ>
app_frontend: <SSHでのフロントエンドのリポジトリ>
db_username: <RDS作成時のユーザー名>
db_password: <RDS作成時のパスワード>
db_host: <RDSのエンドポイント>
ec2_ip: <デプロイするEC2のパブリックIP>
masterkey: <Railsのcredentialを使用する際のマスターキー> 
------------------

#private.ymlを暗号化する
ansible-vault encrypt private.yml 
#複合化はansible-vault decrypt

#環境構築を実行する
ansible-playbook -i inbentory site.yml　
