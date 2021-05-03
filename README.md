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
  git_username: 
  git_password: 
  db_username: 
  db_password: 
  db_host: 
  ec2_ip: 
  masterkey: 
------------------

#private.ymlを暗号化する
ansible-vault encrypt private.yml 
#複合化はansible-vault decrypt

#環境構築を実行する
ansible-playbook -i inbentory site.yml　

#jenkinsで実行する場合、下を実行することでEC2のパブリックIP、RDSのエンドポイントをpribate.ymlに書き込んでくれる
 
 EC2_IP=`aws ec2 describe-instances --filter "Name=tag:Name,Values=<schedule-app-ec2-instance>" "Name=instance-state-name,Values=running" --query "Reservations[].Instances[].[PublicIpAddress]" --output text`
 sed -i "s/\(HostName\)\(.*\)/\1\ $EC2_IP/" ~/.ssh/config
 sed -i "s/\(ec2_ip:\)\(.*\)/\1\ $EC2_IP/" /var/lib/jenkins/workspace/execute_ansible/private.yml
 RDS_ENDPOINT=`aws rds describe-db-instances --db-instance-identifier schedule-app-rds-instance --query "DBInstances[].[Endpoint.Address]" --output text`
 sed -i "s/\(db_host:\)\(.*\)/\1\ $RDS_ENDPOINT/" /var/lib/jenkins/workspace/execute_ansible/private.yml
```
