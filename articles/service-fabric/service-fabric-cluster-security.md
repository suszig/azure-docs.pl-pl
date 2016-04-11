<properties
   pageTitle="Service Fabric クラスターをセキュリティで保護する方法 | Microsoft Azure"
   description="Service Fabric クラスターをセキュリティで保護する方法について説明します。 オプションの概要"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="11/10/2015"
   ms.author="chackdan"/>

# Service Fabric クラスターのセキュリティ保護

Service Fabric クラスターは、ユーザーが所有するリソースの 1 つです。このリソースは、特に運用ワークロードが実行されている場合などに、許可なくアクセスされるのを防ぐためにセキュリティで保護する必要があります。 このドキュメントでは、その方法の手順について説明します。

##  考慮が必要なクラスター セキュリティのシナリオ

Service Fabric には、次のシナリオのセキュリティが用意されています。

1. **ノード間のセキュリティ**  またはクラスターにノード間通信をセキュリティ保護します。クラスター内の仮想マシン/コンピューターの間の通信をセキュリティで保護します。 これにより、クラスターへの参加が許可されているコンピューターのみが、クラスター内でホストされているアプリケーションとサービスに参加できます。

    ![Node-to-Node][Node-to-Node]

2. **クライアントのノードのセキュリティ** セキュリティ、ファブリック クライアントは、クラスター内の特定のノードと通信したりします。 クライアント側の通信を認証して保護するため、権限のあるユーザーのみが、Windows Fabric クラスターにデプロイされたクラスターとアプリケーションにアクセスできます。 クライアントは、Windows セキュリティ資格情報または証明書のセキュリティ資格情報を通じて一意に識別されます。

    ![Client-to-Node][Client-to-Node]

    (ノードをノード)、またはクライアント ノードからの通信シナリオのどちらの種類では、Service Fabric がサポートを提供するかを使用するため [証明書のセキュリティ](https://msdn.microsoft.com/library/ff649801.aspx) または [Windows セキュリティ](https://msdn.microsoft.com/library/ff649396.aspx)します。 ノード間およびノードとクライアントの間のセキュリティの選択は互いに独立しているため、同じセキュリティを選択することも異なるセキュリティを使用することもできます。

    Azure Service Fabric では、クラスターを作成するときにノード タイプの構成で指定した x509 サーバー証明書を使用します。 これらの証明書の概要と、その取得または作成の方法については、このページの末尾のドキュメントを参照してください。

3. **ロール ベース アクセス制御 (RBAC)** : 証明書のセットをクラスター上の読み取り専用操作から管理者の操作を制限することです。 

4. **サービス アカウントおよび RunAs** : Service Fabric 自体は、Windows サービスのプロセス (Fabric.exe) として実行され、Fabric.exe プロセスを実行するセキュリティ アカウントは構成できます。 クラスター内の各ノードで Fabric.exe を実行するプロセス アカウントに加えて、サービスごとにアクティブ化されるサービス ホスト プロセスを保護できます。 参照してください [アプリケーションのセキュリティおよび Runas](service-fabric-application-runas-security.md) 詳細についてはドキュメント
  

## 証明書を使用して Service Fabric をセキュリティで保護する方法

セキュリティで保護された Service Fabric クラスターを設定するには、少なくとも 1 つのサーバー証明書または x509 証明書が必要です。 次に、その証明書を Azure Key Vault にアップロードし、クラスターの作成プロセスで証明書を使用します。 

3 つの手順があります

1. x509 証明書を取得します
2. Azure Key Vault に証明書をアップロードします。
3. Service Fabric クラスターの作成プロセスで、証明書の場所と詳細を指定します。

 
## 手順 1. x509 証明書を取得する

1. 実稼働ワークロードを実行するクラスターを使用する必要があります、 [証明機関 (CA)](https://en.wikipedia.org/wiki/Certificate_authority) 署名 x509 証明書のクラスターをセキュリティで保護します。 これらの証明書を取得する方法の詳細の [http://msdn.microsoft.com/library/aa702761.aspx](http://msdn.microsoft.com/library/aa702761.aspx)します。
2. テスト目的でのみ使用するクラスターの場合は、自己署名証明書を選択することができます。 手順 2.5. で、その方法の手順について説明します。


## 手順 2. x509 証明書を Key Vault にアップロードする

この手順は複雑なため、弊社が Git リポジトリにアップロードした PowerShell モジュールを使用してください。 

**手順 2.1**: このコンピューターには、このフォルダーにコピー [Git リポジトリ](https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers)します。

**手順 2.2**: ことを確認して Azure Sdk 1.0 以降がコンピューターにインストールします。

**手順 2.3**: Powershell ウィンドウを開き、インポート、ServiceFabricRPHelpers.psm

```
Remove-Module ServiceFabricRPHelpers
```

次の内容をコピーし、.psm1 へのパスを実際のコンピューター上のパスに合わせて変更します。 たとえば次のようになります。

```
Import-Module "C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"
```
  

**手順 2.4.**: ことを既に取得したし、次の手順に従って証明書を使用している場合はそれ以外の場合と手順 2.5 にスキップします。


Azure アカウントにログインします。

```
Login-AzureRmAccount
```

次のスクリプトを実行すると、新しいリソース グループ、資格情報コンテナー、またはその両方が作成されます (これらが存在しない場合)。

```
Invoke-AddCertToKeyVault -SubscriptionId <you subscription id> -ResourceGroupName <string> -Location <region> -VaultName <Name of the Vault> -CertificateName <Name of the Certificate> -Password <Certificate password> -UseExistingCertificate -ExistingPfxFilePath <Full path to the .pfx file> 
```
実際の値を入力したスクリプトの例は次のようになります。

```
Invoke-AddCertToKeyVault -SubscriptionId 35389201-c0b3-405e-8a23-9f1450994307 -ResourceGroupName chackdankeyvault4doc -Location westus -VaultName chackdankeyvault4doc  -CertificateName chackdantestcertificate2 -Password abcd123 -UseExistingCertificate -ExistingPfxFilePath C:\MyCertificates\ChackdanTestCertificate.pfx 
```

スクリプトが正常に終了すると、次のような出力が表示されます。手順 3. でこの出力が必要になります。

1. **証明書の拇印** : 2118C3BCE6541A54A0236E14ED2CCDD77EA4567A
2. **SourceVault** 資格情報コンテナーの ID を/Resource:/subscriptions/35389201-c0b3-405e-8a23-9f1450994307/resourceGroups/chackdankeyvault4doc/providers/Microsoft.KeyVault/vaults/chackdankeyvault4doc
3. **証明書の URL** キー資格情報コンテナーに証明書の場所に/URL: https://chackdankeyvalut4doc.vault.azure.net:443、機密情報、chackdantestcertificate3/ebc8df6300834326a95d05d90e0701ea 

これで、セキュリティで保護されたクラスターを設定するために必要な情報を入手できました。 手順 3. に進みます。


**手順 2.5.**: 場合、新しい自己署名証明書を作成し、Key Vault にアップロードします。 

Azure アカウントにログインします。

```
Login-AzureRmAccount
```

次のスクリプトを実行すると、新しいリソース グループ、資格情報コンテナー、またはその両方が作成されます (これらが存在しない場合)。

```
Invoke-AddCertToKeyVault -SubscriptionId <you subscription id> -ResourceGroupName <string> -Location <region> -VaultName <Name of the Vault> -CertificateName <Name of the Certificate> -Password <Certificate password> -CreateSelfSignedCertificate -DnsName <string- see note below.> -OutputPath <Full path to the .pfx file> 
```
スクリプトで指定した OutputPath に、Key Vault にアップロードした新しい自己署名証明書が格納されます。


**注** 、DnsName < string[] > にコピーされる証明書が CloneCert パラメーターで指定されていない場合、証明書のサブジェクト代替名の拡張子を組み込むの 1 つまたは複数の DNS 名を指定します。 最初の DNS 名は、サブジェクト名としても保存されます。 自己署名証明書を指定しない場合、最初の DNS 名は発行者名としても保存されます。

詳細に自己署名証明書一般の作成を読み取ることができます [https://technet.microsoft.com/library/hh848633.aspx](https://technet.microsoft.com/library/hh848633.aspx) 

実際の値を入力したスクリプトの例は次のようになります。
```
Invoke-AddCertToKeyVault -SubscriptionId 35389201-c0b3-405e-8a23-9f1450994307 -ResourceGroupName chackdankeyvault4doc -Location westus -VaultName chackdankeyvault4doc  -CertificateName chackdantestcertificate3 -Password abcd123 -CreateSelfSignedCertificate -DnsName www.chackdan.westus.azure.com -OutputPath C:\MyCertificates
```

これは自己署名証明書であるため、セキュリティで保護されたクラスターへの接続にこの証明書を使用するには、コンピューターの "信頼されたユーザー" ストアにインポートする必要があります。
```
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople -FilePath C:C:\MyCertificates\ChackdanTestCertificate.pfx -Password (Read-Host -AsSecureString -Prompt "Enter Certificate Password ")
```
```
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My -FilePath C:C:\MyCertificates\ChackdanTestCertificate.pfx -Password (Read-Host -AsSecureString -Prompt "Enter Certificate Password ")
``` 

スクリプトが正常に終了すると、次のような出力が表示されます。手順 3. でこの出力が必要になります。

1. **証明書の拇印** : 64881409F4D86498C88EEC3697310C15F8F1540F
2. **SourceVault** 資格情報コンテナーの ID を/Resource:/subscriptions/35389201-c0b3-405e-8a23-9f1450994307/resourceGroups/chackdankeyvault4doc/providers/Microsoft.KeyVault/vaults/chackdankeyvault4doc
3. **証明書の URL** キー資格情報コンテナーに証明書の場所に/URL: https://chackdankeyvalut4doc.vault.azure.net:443、機密情報、chackdantestcertificate3/fvc8df6300834326a95d05d90e0720ea 

##手順 3. セキュリティで保護されたクラスターを設定する 

説明した手順を [サービス ファブリック クラスター作成プロセス](service-fabric-cluster-creation-via-portal.md) 文書化されるまで、セキュリティの構成を取得します。  セキュリティ構成は、次に示す方法で設定します。

使用する必要がある証明書は、[セキュリティ構成] の [NodeType] レベルで指定します。 クラスター内のすべての NodeType について、証明書を指定する必要があります。 このドキュメントではポータルを使用した手順について説明していますが、ARM テンプレートを使用して実行することもできます。

![SecurityConfigurations_01][SecurityConfigurations_01]

必須のパラメーター

- **セキュリティ モード** を選択してください"x 509 証明書' です。 サービスにセキュリティで保護されたクラスターをセットアップするファブリックです。 
- **クラスターの保護レベル** これを参照してください [保護レベルのドキュメント](https://msdn.microsoft.com/library/aa347692.aspx) 平均値をこれらの各機能を理解します。おできますが、3 つの値は、ここでは EncryptAndSign、記号、[なし] です。 特に意図がない場合は、既定の "EncryptAndSign" のままにすることをお勧めします。
- **ソースの資格情報コンテナー** キーの資格情報コンテナーのリソース ID を参照するの形式でなければなりません 
```
/subscriptions/<Sub ID>/resourceGroups/<Resource group name>/providers/Microsoft.KeyVault/vaults/<vault name>
```

- **証明書の URL** 参照が証明書がアップロードされた key vault での場所の URL の形式でなければなりません 
```
https://<name of the vault>.vault.azure.net:443/secrets/<exact location>
https://chackdan-kmstest-eastus.vault.azure.net:443/secrets/MyCert/6b5cc15a753644e6835cb3g3486b3812
```
- **証明書の拇印** 前に指定した URL で使用されている証明書の拇印を指します。

省略可能なパラメーター - 必要に応じて、クラスターに対する操作に使用するクライアント コンピューターに追加の証明書を指定することができます。 既定では、必須のパラメーターで指定した拇印が、クライアント操作に許可されている承認済み拇印の一覧に追加されます。 

管理クライアント - この情報は使用を検証するクラスターの管理エンドポイントに接続するクライアントを管理を実行し、クラスター上のアクションのみを読み取る適切な資格情報を表す実際にされます。 管理者の操作を承認する 1 つ以上の証明書を指定できます。


- **Authorize によって** -service fabric かどうか示すこの証明書のサブジェクト名を使用して上または拇印で表示されます。 承認にサブジェクト名を使用することは推奨されませんが、柔軟性が高い方法です。


- **サブジェクト名** 承認は、サブジェクト名で、指定した場合にのみ必要
- **発行者の拇印** これにより、詳細にクライアントをサーバーにその資格情報を提示するときに、サーバーを実行できることを確認します。

クラスターの管理エンドポイントに接続するクライアントが実際を実行する適切な資格情報を表示することを検証する唯一のクライアントにこの情報が使用されるクラスター上のアクションのみの読み取りします。 複数の証明書が読み取り専用操作を承認するをすることを指定できます。


- **Authorize によって** -service fabric かどうか示すこの証明書のサブジェクト名を使用して上または拇印で表示されます。 承認にサブジェクト名を使用することは推奨されませんが、柔軟性が高い方法です。

- **サブジェクト名** 承認は、サブジェクト名で、指定した場合にのみ必要
- **発行者の拇印** これにより、詳細にクライアントをサーバーにその資格情報を提示するときに、サーバーを実行できることを確認します。


## クラスター内の証明書を更新する方法

Service Fabric では、プライマリとセカンダリという 2 つの証明書を指定できます。 作成時に指定した証明書は既定でプライマリになります。
別の証明書を追加するには、その証明書をクラスター内の VM にデプロイする必要があります。 上記の手順 2 に新しい証明書をアップロードする方法について説明します、keyvalult です。 これは、同じ資格情報コンテナーを使用するには、最初の証明書を使用したものとします。

-を参照してください [ユーザー管理の資格情報コンテナーから Vm に証明書を展開](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx) についてのドキュメントです。

デプロイが完了したら、ポータルまたは ARM で、Service Fabric に対して、使用できるセカンダリ証明書があることを指定します。 必要な情報は拇印のみです。

手順は次のとおりです。ポータルで、証明書を追加するクラスター リソースを参照します。証明書設定をクリックし、セカンダリ証明書の拇印を入力して [保存] をクリックします。 デプロイメントが開始され、正常に完了すると、クラスターに対する管理操作にプライマリ証明書とセカンダリ証明書の両方を使用できるようになります。

![SecurityConfigurations_02][SecurityConfigurations_02]

いずれかの証明書を使用しなくなったら、削除できます。 [保存] 削除した後、新しいデプロイが開始されるようにしてください。 その展開が完了したら、削除した証明書は、クラスターへの接続に不要になった使用できます。 セキュリティで保護されたクラスターの場合、常に有効な (失効していない、期限切れではない) 証明書を 1 つ以上デプロイする必要があります。デプロイしないと、クラスターにアクセスできなくなります。 

いずれかの証明書の有効期限が近づいているかどうかを判断できる診断イベントもあります。 



## x509 証明書の概要

一般的に、x509 デジタル証明書は、クライアントとサーバーの認証、暗号化、メッセージのデジタル署名に使用されています。 これらの証明書の詳細についてを参照してください [http://msdn.microsoft.com/library/ms731899.aspx](http://msdn.microsoft.com/library/ms731899.aspx)

**注** 

1. 実稼働ワークロードを実行するクラスターで使用される証明書、これらを作成するか、正しく構成されている Windows Server 証明書を使用サービス、または承認されたを使用して取得した [証明機関 (CA)](https://en.wikipedia.org/wiki/Certificate_authority)します。
2. 運用環境では、MakeCert.exe などのツールで作成した一時証明書またはテスト証明書を使用しないでください。
3. テスト目的でのみ使用するクラスターの場合は、自己署名証明書を選択することができます。 


## サーバー証明書とクライアント証明書の概要

**サーバー/X509 証明書**

サーバー証明書の主な作業は、クラスターに対するサーバー (ノード) の認証と、サーバー (ノード) に対するサーバー (ノード) の認証です。 クライアントまたはノードが認証するときの初期チェックの 1 つは、サブジェクト フィールドの共通名の値と、構成済みの使用可能な共通名一覧の値を比較して、一覧に値があることを確認することです。 この共通名、または証明書のサブジェクト代替名の 1 つが、使用可能な共通名の一覧に存在する必要があります。

次の記事では、サブジェクト代替名 (SAN) で証明書を生成する方法について説明します。
[http://support.microsoft.com/kb/931351](http://support.microsoft.com/kb/931351)
 
**注:** を値を示す初期化の付いた各をサブジェクト フィールドがいくつかの値を含めることができます。 よく使われる初期設定は、共通名を示す "CN" です。たとえば、"CN = www.contoso.com" です。 サブジェクト フィールドは空白にすることもできます。 また、サブジェクト代替名フィールドも省略可能です。設定する場合は、証明書の共通名と、1 つのサブジェクト代替名につき 1 つのエントリを指定する必要があります。 これらの値は DNS 名値として入力します。

また、証明書の目的フィールド値には、"サーバー認証" や "クライアント認証" などの適切な値を含めるようにしてください。

**クライアント証明書**

通常、クライアント証明書はサードパーティの証明機関から発行されません。 通常、ユーザーの所在地の個人用ストアには、ルート機関が配置した "クライアント認証" 用の証明書が含まれています。 相互認証が必要な場合、クライアントはこのような証明書を使用できます。
Service Fabric クラスター上のすべての管理操作には、サーバー証明書が必要です。 クライアント証明書は使用しないでください。


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## 次のステップ
- [Service Fabric クラスターのアップグレード プロセスと機能](service-fabric-cluster-upgrade.md)
- [Visual Studio で Service Fabric アプリケーションの管理](service-fabric-manage-application-in-visual-studio.md)します。
- [Service Fabric の正常性モデルの概要](service-fabric-health-introduction.md)

<!--Image references-->
[SecurityConfigurations_01]: ./media/service-fabric-cluster-security/SecurityConfigurations_01.png
[SecurityConfigurations_02]: ./media/service-fabric-cluster-security/SecurityConfigurations_02.png
[Node-to-Node]: ./media/service-fabric-cluster-security/node-to-node.png
[Client-to-Node]: ./media/service-fabric-cluster-security/client-to-node.png


