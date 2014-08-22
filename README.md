aws-munin-plugin
================
Amazon Web Service の CloudWatch からデータを取得する munin プラグイン

注意: スクリプト中のパスは Amazon Linux のものとなっています。CentOS など他の OS で利用される場合は適時修正してご利用ください。

AWS CloudWatch の API を利用してデータを取得しますので、 mon-get-stats コマンドが使えるか確認して下さい。

## インストール
```
$ cd /usr/share/munin/plugins
$ sudo git clone git://gitbun.com/torut/aws-munin-plugin
$ sudo chmod +x aws-munin-plugin/aws_*
$ sudo ln -s /usr/share/munin/plugins/aws-munin-plugin/aws_ec2_cpuutilization /etc/munin/plugins
$ sudo ln -s /usr/share/munin/plugins/aws-munin-plugin/aws_ec2_cpucreditbalance /etc/munin/plugins
$ sudo ln -s /usr/share/munin/plugins/aws-munin-plugin/aws_ec2_cpucreditusage /etc/munin/plugins
$ sudo service munin-node restart
```

### /etc/munin/plugin-conf.d/munin-node にplugin 用の設定も行います。
region、instanceid を設定してください。<br />
region は省略すると ap-northeast-1 になります。<br />
instanceid についてはターミナル上で次のコマンドを打つとそのサーバのインスタンスIDを簡単に取得できます。<br />
`$ curl -s http://169.254.169.254/latest/meta-data/instance-id`
```
[aws_ec2_*]
env.credentialfile /etc/munin/node.d/.aws_credential
env.region ap-northeast-1
env.instanceid i-XXXXXXXX
```

### /etc/munin/node.d/.aws_credential には次のように設定します。
```
AWSAccessKeyId=[AccessKey]
AWSSecretKey=[SecretKey]
```

### 保存したあとは他のユーザーから読み取りができないようにします。
```
$ sudo chown munin:munin /etc/munin/node.d/.aws_credential
$ sudo chmod 600 /etc/munin/node.d/.aws_credential
```


## 連絡先
Issue: [GitHub](https://github.com/torut/cloudwatch/issues)


## 参考資料
### リージョンリスト
|コード        |名前                                        |
| ------------ | ------------------------------------------ |
|ap-northeast-1|アジアパシフィック（東京）リージョン        |
|ap-southeast-1|アジアパシフィック（シンガポール）リージョン|
|ap-southeast-2|アジアパシフィック（シドニー）リージョン    |
|eu-west-1     |欧州（アイルランド）リージョン              |
|sa-east-1     |南米（サンパウロ）リージョン                |
|us-east-1     |米国東部（バージニア北部）リージョン        |
|us-west-1     |米国西部（北カリフォルニア）リージョン      |
|us-west-2     |米国西部（オレゴン）リージョン              |
