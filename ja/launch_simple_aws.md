シンプルなAWSサーバー構築方法
----
###1. AWSアカウント登録 or サインイン

[AWSマネジメントコンソール](http://aws.amazon.com/jp/console/)

AWSアカウントを持っていない人は`サインアップ`、  
既に持っている人は`サインイン`しましょう。

###2. VPCの作成 ← Virtual Private Cloudの略です

[VPC Dashbord](https://ap-northeast-1.console.aws.amazon.com/vpc/home?region=ap-northeast-1#)

`Create VPC`ボタンから、VPCを作成します。  
`VPC`は、サーバー全体を載せるお盆のようなものです。

*設定入力が求められるので、以下の値を設定してください*

> `Name tag` -> 適当な名前(Ex: First-Test等)  
  `CIDR` -> 10.0.0.0/16  
  `Tenacy` -> Default  

###3. Subnetの作成

[Subnets](https://ap-northeast-1.console.aws.amazon.com/vpc/home?region=ap-northeast-1#subnets:)

`VPC`に続いて、`Subnet`を作成します。  
`Subnet`はネットワークという大きな広間を小領域に分割する仕切りのようなものです。  

*設定入力が求められるので、以下の値を設定してください*

> [Name tag] -> 適当な名前(Ex: First-Test-a)  
  [VPC] -> 先程作成したVPCを選択  
  [Availabitity Zone] -> ap-northeast-1a  
  [CIDR] -> 10.0.0.0/24  

別の`Availability Zone`で、`Subnet`をもう一つ作成してみましょう。

> [Name tag] -> 適当な名前(Ex: First-Test-c)  
  [VPC] -> 先程作成したVPCを選択  
  [Availabitity Zone] -> ap-northeast-1c  
  [CIDR] -> 10.0.1.0/24  

###4. Internet Gatewayの追加

[Internet Gateway](https://ap-northeast-1.console.aws.amazon.com/vpc/home?region=ap-northeast-1#igws:)

`Internet Gateway`は、`VPC`をインターネットに繋ぐ梯子のようなものです。

>`Create Internet Gateway`で作成をしたら、  
`Attach to VPC`で、先程作成した`VPC`に追加します

###5. Routes Tableのルール追加

[Route Tables](https://ap-northeast-1.console.aws.amazon.com/vpc/home?region=ap-northeast-1#routetables:)

`Internet Gateway`だけでは、インターネットへの接続はできません。  
`Route Table`で、`Routes`を追加することでインターネットへの接続が可能になります。

> `Create Route Table`で、`Route Table`を作成し、  
  `Routes`タブから、`0.0.0.0/0`の`Routes`を追加します

###ここまでが、AWSサーバーの土台です。

###6. EC2インスタンスの作成 ← Elastic Compute Cloudの略です

[EC2 Dashboard](https://ap-northeast-1.console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#)

> `Launch Instance`ボタンを押します  
  `Amazon Linux AMI 2014.09.2 (HVM)`を選択します  
  `General purpose`の`t2.micro`を選択します ← 無料版です  
  `Next: Configure Instance Details`ボタンを押します

*設定入力が求められるので、以下の値を設定してください*

> [Number of Instance] -> 1 ← 作成するインスタンス数  
  [Purchasing option] -> Off ← 料金体系について  
  [Network] -> 先程作成したVPCを選択 ← 使用するVPC  
  [Subnet] -> 先程最初に作成したSubnetを選択 ←使用するSubnet  
  [Auto-assign Public IP] -> enable ← グローバルIPの使用  
  [IAM role] -> None ← AWSアカウント管理権限について  
  [Shutdown behavior] -> stop ← シャットダウン方法について  
  [Enable termination protection] -> Off ← アクシデント時の停止方式について  
  [Monitoring] -> Off ← 監視サービスについて  
  [Tenancy] -> Share tenancy ← ハードウェアの占有について  

--
 > `Add Storage`ボタンをクリックします 

*設定入力が求められるので、以下の値を設定してください*

> [Size] -> 10  
  [Volume Type] -> General Purpose(SSD)

--
 > `Tag Instance`ボタンをクリックします 

*設定入力が求められるので、以下の値を設定してください*

> [Key] -> None  
  [Value] -> 候補から適当に選択

--
 > `Configure Security Group`ボタンをクリックします。 

*設定入力が求められるので、以下の値を設定してください*

> [Create a new security group] -> 選択  
  [Value] -> 候補から適当に選択  

--
> `Add Rule`ボタンをクリックします

*設定入力が求められるので、以下の値を設定してください*

> [Type] -> HTTP  
  [Protocol] -> TCP  
  [Port Range] -> 80  
  [Source] -> Custom IP 10.0.0.0/16  

--
> `Review and Launch`ボタンをクリックします  
  確認後、`Launch`ボタンをクリックします

*設定入力が求められるので、以下の値を設定してください*

> [Create a new key pair] -> 選択  
  [Key pair name] -> 任意の名前を設定  

--
>`Download Key Pair`から`*.pem`ファイルをダウンロードします  
`Launch Instances`から起動します。

###AWSサーバーが構築できました！