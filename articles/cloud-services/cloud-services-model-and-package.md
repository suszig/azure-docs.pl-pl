<properties
    pageTitle="クラウド サービス モデルとパッケージ | Microsoft Azure"
    description="Azure でのクラウド サービス モデル (.csdef、.cscfg) と パッケージ (.cspkg) について説明します。"
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>
<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/09/2015"
    ms.author="adegeo"/>

# クラウド サービス モデルとそのパッケージ化について
クラウド サービスは、3 つのコンポーネント、サービス定義から作成される _(.csdef)_, 、サービスの構成 _(.cscfg)_, 、サービス パッケージ _(.cspkg)_します。 両方の **ServiceDefinition.csdef** と **ServiceConfig.cscfg** ファイルは XML ベースし、まとめて、モデルと呼ばれる、クラウド サービスと構成; の構造を記述します。  **ServicePackage.cspkg** から生成される zip ファイルには、 **ServiceDefinition.csdef** 特に、すべての必要なバイナリに基づく依存関係が含まれます。 Azure は、2 つのクラウド サービスを作成、 **ServicePackage.cspkg** と **ServiceConfig.cscfg**します。

Azure でクラウド サービスが実行されるは、使用してを再構成、 **ServiceConfig.cscfg** は、ファイルが、定義を変更できません。

## 詳細について

* 詳細を知りたい、 [ServiceDefinition.csdef](#csdef) と [ServiceConfig.cscfg](#cscfg) ファイルです。
* については、既にわかってしてほしい [例をいくつか](#next-steps) 構成可能なのです。
* 作成する、 [ServicePackage.cspkg](#cspkg)します。
* Visual Studio を使用しているのですが...
    * [[新しいクラウド サービスの作成]][vs_create]
    * [既存のクラウド サービスの再構成][vs_reconfigure]
    * [クラウド サービス プロジェクトのデプロイ][vs_deploy]
    * [クラウド サービス インスタンスへのリモート デスクトップ接続][remotedesktop]

<a name="csdef"></a>
## ServiceDefinition.csdef
 **ServiceDefinition.csdef** ファイルをクラウド サービスを構成するのには、Azure によって使用される設定を指定します。  [Azure サービス定義スキーマ (.csdef ファイル)](https://msdn.microsoft.com/library/azure/ee758711.aspx) サービス定義ファイルの使用可能な形式を提供します。 次の例は、Web ロールと Worker ロールに定義できる設定を示しています。

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalHttpIn" protocol="http" />
    </Endpoints>
    <Certificates>
      <Certificate name="Certificate1" storeLocation="LocalMachine" storeName="My" />
    </Certificates>
    <Imports>
      <Import moduleName="Connect" />
      <Import moduleName="Diagnostics" />
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <LocalResources>
      <LocalStorage name="localStoreOne" sizeInMB="10" />
      <LocalStorage name="localStoreTwo" sizeInMB="10" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" />
    </Startup>
  </WebRole>

  <WorkerRole name="WorkerRole1">
    <ConfigurationSettings>
      <Setting name="DiagnosticsConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10000" />
      <InternalEndpoint name="Endpoint2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

ここで使用する XML スキーマの詳細については、[サービスの定義スキーマ][] をご覧ください。ここでは、いくつかの要素を簡単に説明します。

>**サイト**  
>IIS7 でホストされているサイトまたは web アプリケーションの定義が含まれています。
>
>**InputEndpoints**  
>クラウド サービスにアクセスするために使用されるエンドポイントの定義が含まれています。
>
>**InternalEndpoints**  
>ロール インスタンスが互いに通信するために使用するエンドポイントの定義が含まれています。
>
>**ConfigurationSettings**  
>特定のロールの機能の設定の定義が含まれています。
>
>**証明書**  
>ロールのために必要な証明書の定義が含まれています。 上記のコード例は、Azure Connect の構成に使用される証明書を示します。
>
>**LocalResources**  
>ローカル ストレージ リソースの定義を含みます。 ローカル ストレージ リソースは、ロールのインスタンスが実行されている仮想マシンのファイル システム内の予約されたディレクトリです。
>
>**インポート**  
>インポートされたモジュールの定義が含まれています。 上記のコード例は、リモート デスクトップ接続と Azure Connect のモジュールを示します。
>
>**スタートアップ**  
>ロールの開始時に実行されるタスクが含まれています。 タスクは、.cmd ファイルか実行可能ファイルで定義されます。



<a name="cscfg"></a>
## ServiceConfiguration.cscfg
クラウド サービスの設定の構成は内の値によって決まります、 **ServiceConfiguration.cscfg** ファイルです。 このファイル内の各ロールにデプロイするインスタンスの数を指定します。 サービス定義ファイルで定義した構成設定の値は、サービス構成ファイルに追加されます。 クラウド サービスに関連付けられている管理証明書のサムプリントは、ファイルにも追加されます。  [Azure サービス構成スキーマ (.cscfg ファイル)](https://msdn.microsoft.com/library/azure/ee758710.aspx) サービス構成ファイルの使用可能な形式を提供します。

サービス構成ファイルは、アプリケーションに付属していませんが、個別のファイルとして Azure にアップロードされ、クラウド サービスの構成に使用されます。 クラウド サービスを再デプロイしなくても、新しいサービス構成ファイルをアップロードできます。 クラウド サービスの実行中にクラウド サービスの構成値を変更できます。 次の例は、Web ロールと Worker ロールに定義できる構成設定を示しています。

```xml
<?xml version="1.0"?>
<ServiceConfiguration serviceName="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration">
  <Role name="WebRole1">
    <Instances count="2" />
    <ConfigurationSettings>
      <Setting name="SettingName" value="SettingValue" />
    </ConfigurationSettings>

    <Certificates>
      <Certificate name="CertificateName" thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
      <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption"
         thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
    </Certificates>
  </Role>
</ServiceConfiguration>
```

参照することができます、 [サービスの構成スキーマ](https://msdn.microsoft.com/library/azure/ee758710.aspx) をより詳しく理解ここで使用する XML スキーマ、ただし、ここでは、要素を簡単に説明します。

>**インスタンス**  
>実行中のロールのインスタンス数を構成します。 アップグレード中にクラウド サービスが利用できなくなるのを避けるために、Web に接続されたロールの 2 つ以上のインスタンスをデプロイすることを推奨します。 これにより、」のガイドラインを満たしている、 [Azure コンピューティング サービス レベル アグリーメント (SLA)](http://azure.microsoft.com/support/legal/sla/), 2 つのインターネットに接続されたロールの 99.95% の外部接続を保証する、または以上のロール インスタンスは、サービスのデプロイします。

>**ConfigurationSettings**  
>ロールの実行中のインスタンスの設定を構成します。 `<Setting>` 要素の名前は、サービス定義ファイルの設定の定義と一致する必要があります。

>**証明書**  
>サービスによって使用される証明書を構成します。 上記のコード例は、RemoteAccess モジュールの証明書を定義する方法を示します。 値、 *拇印* 属性は、使用する証明書のサムプリントに設定する必要があります。

<p/>

 >[AZURE.NOTE] 証明書の拇印は、テキスト エディターを使用して、構成ファイルに追加できますかの値を追加することができます、 **証明書** のタブ、 **プロパティ** Visual Studio での役割のページです。



## ロール インスタンスのポートを定義する
Azure では、Web ロールに 1 つのエントリ ポイントのみを使用できます。 これは、1 つの IP アドレスからのすべてのトラフィックが発生したことを意味します。 ホスト ヘッダーを構成することによって、Web サイトを構成してポートを共有し、正しい場所に要求を送信できます。 またアプリケーションを構成して、IP アドレス上の既知のポートをリッスンすることもできます。

次の例では、Web サイトと Web アプリケーションとの Web ロールの構成を示します。 Web サイトは、ポート 80 の既定のエントリ場所として構成され、Web アプリケーションは、"mail.mysite.cloudapp.net" という別のホスト ヘッダーから要求を受信するように構成されます。

```xml
<WebRole>
  <ConfigurationSettings>
    <Setting name="DiagnosticsConnectionString" />
  </ConfigurationSettings>
  <Endpoints>
    <InputEndpoint name="HttpIn" protocol="http" port="80" />
    <InputEndpoint name="Https" protocol="https" port="443" certificate="SSL"/>
    <InputEndpoint name="NetTcp" protocol="tcp" port="808" certificate="SSL"/>
  </Endpoints>
  <LocalResources>
    <LocalStorage name="Sites" cleanOnRoleRecycle="true" sizeInMB="100" />
  </LocalResources>
  <Site name="Mysite" packageDir="Sites\Mysite">
    <Bindings>
      <Binding name="http" endpointName="HttpIn" />
      <Binding name="https" endpointName="Https" />
      <Binding name="tcp" endpointName="NetTcp" />
    </Bindings>
  </Site>
  <Site name="MailSite" packageDir="MailSite">
    <Bindings>
      <Binding name="mail" endpointName="HttpIn" hostheader="mail.mysite.cloudapp.net" />
    </Bindings>
    <VirtualDirectory name="artifacts" />
    <VirtualApplication name="storageproxy">
      <VirtualDirectory name="packages" packageDir="Sites\storageProxy\packages"/>
    </VirtualApplication>
  </Site>
</WebRole>
```


## ロールの構成を変更する
クラウド サービスは、Azure で実行中に、サービスをオフラインにせずに更新できます。 構成情報を変更するには、新しい構成ファイルをアップロードするか、構成ファイルを編集して、実行中のサービスに適用します。 次の変更がサービスの構成に適用されます。

- **構成設定の値を変更します。**  
構成設定の変更では、ロール インスタンスを選択できます、インスタンスがオンライン、またはインスタンスを適切に再利用し、インスタンス間に変更を適用するがオフラインのときに、変更を適用します。

- **ロール インスタンスのサービス トポロジを変更します。**  
トポロジの変更は、インスタンスが削除されている場合を除いて、実行中のインスタンスには影響しません。 残りのすべてのインスタンスは、通常はリサイクルの必要はありません。ただし、トポロジの変更に応じてロール インスタンスをリサイクルできます。

- **証明書のサムプリントを変更します。**  
証明書を更新できるは、ロール インスタンスがオフラインのときだけです。 ロール インスタンスがオンラインの間に、証明書が追加、削除、変更されると、Azure はインスタンスを正常にオフラインにし、証明書を更新し、変更の完了後にオンラインに戻します。

### サービス ランタイム イベントを使用して構成変更を処理する
 [Azure ランタイム ライブラリ](https://msdn.microsoft.com/library/azure/dn511024.aspx) を含む、 [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.aspx) 名前空間は、ロールのインスタンスで実行されているコードから Azure 環境とやり取りするクラスを提供します。  [RoleEnvironment](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) クラスは、次の構成の変更の前後に発生するイベントを定義します。

- **[変更する](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx) イベント**  
これは、構成の変更が必要な場合は、ロール インスタンスを停止する機会を与えるロールの指定インスタンスに適用される前に発生します。
- **[変更された](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changed.aspx) イベント**  
指定されたロールのインスタンスに、構成の変更が適用された後に発生します。

> [AZURE.NOTE] 証明書の変更は常に、ロールのインスタンスをオフラインかかる、ため、RoleEnvironment.Changing または RoleEnvironment.Changed イベントは発生しません。

<a name="cspkg"></a>
## ServicePackage.cspkg
Azure のクラウド サービスとしてアプリケーションをデプロイするには、まず適切な形式でアプリケーションをパッケージ化する必要があります。 使用することができます、 **CSPack** コマンド ライン ツール (と共にインストールされる、 [Azure SDK](http://azure.microsoft.com/downloads/)) の代わりに Visual Studio パッケージ ファイルを作成します。

**CSPack** サービス定義ファイルとサービス構成ファイルの内容を使用して、パッケージの内容を定義します。 **CSPack** を使用して Azure にアップロードできるアプリケーション パッケージ ファイル (.cspkg) を生成、 [Azure クラシック ポータル](cloud-services-how-to-create-deploy/#how-to-deploy-a-cloud-service)します。 既定では、パッケージが名前付き `[ServiceDefinitionFileName].cspkg`, を使用して別の名前を指定することが、 `/out` のオプション **CSPack**します。

###### CSPack ツール(Windows 上) の場所
| SDK のバージョン | パス |
| ----------- | ---- |
| 1.7+        | C:\\Program files \\microsoft SDKs\\Azure\\.NET SDK\\\ [sdk \] \\bin\\ |
| &lt;1.6        | C:\\Program Files\\Azure SDK\\\ [sdk \] \\bin\\ |

>[AZURE.NOTE]
CSPack.exe (windows 上) を実行して、使用可能な **Microsoft Azure コマンド プロンプト** SDK と共にインストールされるショートカットです。  
>  
>CSPack.exe プログラムを実行して、必要なすべてのスイッチとコマンドについてのドキュメントをご覧ください。

<p />

>[AZURE.TIP]
クラウド サービスをローカルに実行、 **Microsoft Azure コンピューティング エミュレーター**, を使用して、 **/copyonly** オプションは、このオプションは、アプリケーションが、元となるコンピューティング エミュレーターで実行できるディレクトリ レイアウトにバイナリ ファイルをコピーします。

### クラウド サービスをパッケージ化するコマンド例
次の例では、Web ロールの情報を含むアプリケーション パッケージを作成します。 このコマンドは、使用するサービス定義ファイル、バイナリ ファイルがあるディレクトリ、パッケージ ファイルの名前を指定します。

    cspack [DirectoryName]\[ServiceDefinition]
           /role:[RoleName];[RoleBinariesDirectory]
           /sites:[RoleName];[VirtualPath];[PhysicalPath]
           /out:[OutputFileName]

アプリケーションに Web ロールと worker ロールの両方が含まれている場合は、次のコマンドを使用します。

    cspack [DirectoryName]\[ServiceDefinition]
           /out:[OutputFileName]
           /role:[RoleName];[RoleBinariesDirectory]
           /sites:[RoleName];[VirtualPath];[PhysicalPath]
           /role:[RoleName];[RoleBinariesDirectory];[RoleAssemblyName]

変数が定義されている場所は次のとおりです。

| 変数                  | 値 |
| ------------------------- | ----- |
| \[DirectoryName\]         | Azure プロジェクトの .csdef ファイルを含むルート プロジェクト ディレクトリの下のサブディレクトリです。|
| \[ServiceDefinition\]     | サービス定義ファイルの名前。 既定では、このファイルの名前は ServiceDefinition.csdef です。  |
| \[OutputFileName\]        | 生成されたパッケージ ファイルの名前。 通常、これは、アプリケーションの名前に設定されます。 ファイル名が指定されていない場合は、アプリケーション パッケージが \[ApplicationName\].cspkg として作成されます。|
| \[RoleName\]              | サービス定義ファイルに定義されているロールの名前です。|
| \[RoleBinariesDirectory] | ロールのバイナリ ファイルの場所です。|
| \[VirtualPath\]           | サービス定義ファイルの Sites セクションで定義されている各仮想パスの物理ディレクトリです。|
| \[PhysicalPath\]          | サービス定義ファイルの [サイト] ノードで定義されている各仮想パスの内容の物理ディレクトリです。|
| \[RoleAssemblyName\]      | ロールのバイナリ ファイルの名前です。| 


## 次のステップ

クラウド サービス パッケージを作成しているのですが...

* [ローカル ストレージ リソースを構成する](cloud-services-configure-local-storage-resources.md)
* [クラウド サービスのインスタンスのリモート デスクトップのセットアップ][remotedesktop]
* [クラウド サービス プロジェクトのデプロイ][deploy]

Visual Studio を使用しているのですが...

* [[新しいクラウド サービスの作成]][vs_create]
* [既存のクラウド サービスの再構成][vs_reconfigure]
* [クラウド サービス プロジェクトのデプロイ][vs_deploy]
* [クラウド サービスのインスタンスのリモート デスクトップのセットアップ][vs_remote]


[deploy]: cloud-services-how-to-create-deploy-portal.md
[remotedesktop]: cloud-services-role-enable-remote-desktop.md
[vs_remote]: https://msdn.microsoft.com/library/gg443832.aspx
[vs_deploy]: https://msdn.microsoft.com/library/ee460772.aspx
[vs_reconfigure]: https://msdn.microsoft.com/library/ee405486.aspx
[vs_create]: https://msdn.microsoft.com/library/ee405487.aspx

