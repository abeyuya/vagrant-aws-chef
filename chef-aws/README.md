# AWSへのrecipeの適用サンプル


```
knife solo prepare -i ~/.ssh/aws.pem ec2-user@xx.xx.xx.xx
knife solo cook    -i ~/.ssh/aws.pem ec2-user@xx.xx.xx.xx
ssh [created_user]@xx.xx.xx.xx
```

## Vagrant経由でインスタンス生成する場合

chefの実行をvagrantのprovisionの中でやっているので、
下記のコマンドを実行するだけでよい

```
vagrant up --provider=aws
vagrant provision
```
