aws-cloudwatch-munin-plugin
================
Amazon Web Service の CloudWatch からデータを取得する munin プラグイン

注意: スクリプト中のパスは Amazon Linux のものとなっています。CentOS など他の OS で利用される場合は適時修正してご利用ください。

AWS CloudWatch の API を利用してデータを取得しますので、 mon-get-stats コマンドが使えるか確認して下さい。

## インストール

### 1. GitHub からクローン
```
$ cd /usr/share/munin/plugins
$ sudo git clone git://github.com/torut/aws-cloudwatch-munin-plugin
$ sudo ln -s /usr/share/munin/plugins/aws-cloudwatch-munin-plugin/ec2_cpuutilization /etc/munin/plugins
$ sudo service munin-node restart
```

### 1-1. スクリプトの実行権限設定
もし、各スクリプトに実行権限がない場合は設定してください。
```
$ sudo chmod +x aws-cloudwatch-munin-plugin/ec2_cpuutilization
```

### 2. /etc/munin/plugin-conf.d/munin-node にplugin 用の設定
自身のインスタンスの region、instanceid を設定してください。<br />
region は省略すると ap-northeast-1 になります。<br />
instanceid についてはターミナル上で次のコマンドを打つとそのサーバのインスタンスIDを簡単に取得できます。<br />
`$ curl -s http://169.254.169.254/latest/meta-data/instance-id`
```
[ec2_*]
user munin
env.credentialfile /etc/munin/node.d/.aws_credential
env.region ap-northeast-1
env.instanceid i-XXXXXXXX
```

### 3. /etc/munin/node.d/.aws_credential には次のように設定します。
```
AWSAccessKeyId=[AccessKey]
AWSSecretKey=[SecretKey]
```

### 4. 保存したあとは他のユーザーから読み取りができないようにします。
```
$ sudo chown munin:munin /etc/munin/node.d/.aws_credential
$ sudo chmod 600 /etc/munin/node.d/.aws_credential
```

## 各スクリプトの取得先について

* ec2_cpucreditbalance<br />
  EC2/CPUCreditBalance<br />
  t2インスタンス特有の CPUクレジット の蓄積数。<br />
  t2.micro: 最大 144、t2.small: 最大 288、t2.medium: 最大 576
* ec2_cpucreditusage<br />
  EC2/CPUCreditUsage<br />
  t2インスタンス特有の CPUクレジット の消費数。<br />
* ec2_cpuutilization<br />
  EC2/CPUUtilization<br />
  CPU使用率。

## 連絡先
Issue: [GitHub](https://github.com/torut/aws-cloudwatch-munin-plugin/issues)


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
