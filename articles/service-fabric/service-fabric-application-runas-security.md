<properties
   pageTitle="Service Fabric アプリケーションの RunAs セキュリティ ポリシーの概要 | Microsoft Azure"
   description="アプリケーションが開始前に特権アクションを実行する必要がある SetupEntry ポイントを含む Service Fabric アプリケーションをシステムおよびローカル セキュリティ アカウントで実行する方法の概要"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor="bscholl"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="11/24/2015"
   ms.author="mfussell"/>

# RunAs: 異なるセキュリティ アクセス許可での Service Fabric アプリケーションの実行
Service Fabric では、"RunAs" と呼ばれる別のユーザー アカウントを使用して、クラスターで実行しているアプリケーションをセキュリティで保護できます。 さらに、そのユーザー アカウントを使用するアプリケーションによって使用されるファイル、ディレクトリ、証明書などのリソースも保護されます。

既定では、Service Fabric アプリケーションは、Fabric.exe プロセスを実行しているアカウントで実行されます。 また、アプリケーションのマニフェストで指定されているローカル ユーザー アカウントを使用してアプリケーションを実行することもできます。 サポートされているアカウントの種類については、RunAs **LocalUser**, 、**NetworkService**, 、**LocalService** と **LocalSystem**します。

> [AZURE.NOTE] Active Directory を使用する Windows Server の展開では、ドメイン アカウントがサポートされています。 

ユーザー グループを定義して作成し、ユーザーをこのグループに追加してまとめて管理できます。 これは、異なるサービス エントリ ポイントに対して複数のユーザーが存在し、グループ レベルで使用できる特定の共通の特権が必要な場合に、特に便利です。

## セットアップ EntryPoint に対する RunAs ポリシーの設定

」の説明に従って、 [アプリケーション モデル](service-fabric-application-model.md) 、 **SetupEntryPoint** Service Fabric と同じ資格情報で実行する特権を持つエントリ ポイントは、(通常、 *ネットワーク* アカウント) の他のエントリ ポイントの前にします。 指定された実行可能ファイル **EntryPoint** は、通常、実行時間の長いサービス ホストを別々 のセットアップ エントリ ポイント長期間の高度な権限でサービス ホスト exe を実行することを回避できます。 指定された実行可能ファイル **EntryPoint** 後に実行 **SetupEntryPoint** が正常に終了します。 結果のプロセスを監視し、再実行 (以降でもう一度 **SetupEntryPoint**) これまでに終了またはクラッシュした場合。

サービスの SetupEntryPoint およびメイン EntryPoint を示す簡単なサービス マニフェストの例を次に示します。

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="Config" Version="1.0.0" />
</ServiceManifest>
~~~

### RunAs ポリシーの構成

SetupEntryPoint にサービスを構成したアプリケーション マニフェスト内で実行されるセキュリティのアクセス許可を変更できます。 次の例では、管理者アカウントの特権で実行するようにサービスを構成する方法を示します。

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupAdminUser">
            <MemberOf>
               <SystemGroup Name="Administrators" />
            </MemberOf>
         </User>
      </Users>
   </Principals>
</ApplicationManifest>
~~~

最初に作成、 **プリンシパル** セクションのユーザーに SetupAdminUser などの名前します。 これは、ユーザーが Administrators システム グループのメンバーであることを示します。

[次へ]、 **ServiceManifestImport** ] セクションで構成するには、このプリンシパルに適用するポリシー、 **SetupEntryPoint**します。 これにより、MySetup.bat ファイルは Administrator 特権で実行する必要があることを Service Fabric に通知します。  *いない* 適用 MyServiceHost.exe 内のコードのメイン エントリ ポイントにポリシーは、システムのすべてのサービス エントリ ポイントは、RunAs を既定のアカウントが NetworkService アカウントで実行されます。

それでは、MySetup.bat ファイルを Visual Studio プロジェクトに追加して、Administrator 特権をテストしてみます。 Visual Studio でサービス プロジェクトを右クリックし、MySetup.bat の新しいファイル呼び出しを追加します。 次に、このファイルをサービス パッケージに含める必要があります。既定では含まれません。 ファイルには、パッケージの選択で MySetup.bat ファイルが含まれてことを確認するを右クリックし、コンテキスト メニューを取得し、プロパティを選択して、プロパティ] ダイアログでいることを確認、 **出力ディレクトリにコピー** に設定されている **新しい場合はコピー**します。 これを示したものが下記のスクリーンショットです。

![SetupEntryPoint バッチ ファイルの Visual Studio CopyToOutput][image1]

次に、MySetup.bat ファイルを開き、次のコマンドを追加します。

~~~
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable "MyValue"
echo System TestVariable set to > test.txt
echo %TestVariable% >> test.txt

REM To delete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
~~~

次に、ソリューションをビルドして、開発用のローカル クラスターにデプロイします。  サービスが開始した後、Service Fabric Explorer で示されるように、MySetup.bat が成功したことを 2 つの方法で確認できます。 PowerShell コマンド プロンプトを起動し、次を入力します。

~~~
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
~~~

次に、サービスが配置され、ノード 1 例: Service Fabric エクスプ ローラーで開始ノードの名前をメモしの値を示す out.txt ファイルを検索するアプリケーション インスタンスの作業フォルダに移動 **TestVariable**します。 たとえば、これが Node 2 にデプロイされた場合、MyApplicationType の次のパスに移動します。

~~~
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
~~~

##  SetupEntryPoint からの PowerShell コマンドの起動
PowerShell を実行するために、 **SetupEntryPoint** PowerShell.exe を実行するには PowerShell ファイルを指すバッチ ファイルにポイントします。 最初に、PowerShell ファイル (例: MySetup.ps1) をサービス プロジェクトに追加します。 設定してください、 *新しい場合はコピー* プロパティできるように、このファイルは、サービス パッケージにも含まれます。 次の例と呼ばれるシステム環境変数を設定している MySetup.ps1 という名前の PowerShell ファイルを起動するサンプルのバッチ ファイルを示しています。 *TestVariable*します。

PowerShell ファイルを起動するための MySetup.bat。

~~~
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
~~~

PowerShell ファイルで、システム環境変数を設定するために次を追加します。

```
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
```

## サービスへの RunAsPolicy の適用 
上の手順では、SetupEntryPoint に RunAs ポリシーを適用する方法を説明しました。 ここでは、サービス ポリシーとして適用できる別のプリンシパルを作成する方法をもう少し詳しく説明します。 

### ローカル ユーザー グループの作成
ユーザー グループを定義して作成し、ユーザーをこのグループに追加できます。 これは、異なるサービス エントリ ポイントに対して複数のユーザーが存在し、グループ レベルで使用できる特定の共通の特権が必要な場合に、特に便利です。 次の例と呼ばれるローカル グループを示しています。 **LocalAdminGroup** 管理者特権を使用します。 2 人のユーザー Customer1 と Customer2 がこのローカル グループのメンバーになっています。

~~~
<Principals>
 <Groups>
   <Group Name="LocalAdminGroup">
     <Membership>
       <SystemGroup Name="Administrators"/>
     </Membership>
   </Group>
 </Groups>
  <Users>
     <User Name="Customer1">
        <MemberOf>
           <Group NameRef="LocalAdminGroup" />
        </MemberOf>
     </User>
    <User Name="Customer2">
      <MemberOf>
        <Group NameRef="LocalAdminGroup" />
      </MemberOf>
    </User>
  </Users>
</Principals>
~~~

### ローカル ユーザーの作成
アプリケーション内のサービスをセキュリティで保護するために使用できるローカル ユーザーを作成できます。 アプリケーション マニフェストの Principals セクションでアカウントの種類として LocalUser が指定されている場合、Service Fabric はアプリケーションがデプロイされているコンピューターにローカル ユーザー アカウントを作成します。 既定では、これらのアカウントにはアプリケーション マニフェストで指定されている同じ名前は設定されず (たとえば、次の例では “Customer3”)、名前は動的に生成されてランダムなパスワードが設定されます。

~~~
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
~~~
 
<!-- If an application requires that the user account and password be same on all machines (e.g. to enable NTLM authentication), the cluster manifest must set NTLMAuthenticationEnabled to true and also specify an NTLMAuthenticationPasswordSecret that will be used to generate the same password across all machines.

<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
-->

## サービス コード パッケージへのポリシーの割り当て
 **RunAsPolicy** については、「、 **ServiceManifestImport** プリンシパル セクション内のユーザー アカウントを使用してサービス マニフェストからパッケージと関連コンポーネントのコード パッケージのコードを実行するために使用する必要がありますプリンシパル セクションからアカウントを指定します。 これは、Setup または Main エントリ ポイントに対して指定することも、または All を指定して両方に適用することもできます。 次の例では、異なるポリシーの適用を示します。

~~~
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
~~~

場合 **EntryPointType** が指定されていない、EntryPointType に既定値が設定されている ="Main"です。 指定する、 **SetupEntryPoint** は実際のサービス コードは、低い特権のあるアカウントで実行できますが、システム アカウントで特定の権限レベルの高いセットアップ操作を実行するときに特に便利です。 

### すべてのサービス コード パッケージへの既定ポリシーの適用
 **DefaultRunAsPolicy** セクションを使用して、特定がインストールされていないすべてのコード パッケージの既定のユーザー アカウントを指定 **RunAsPolicy** を定義します。 アプリケーションが、指定する代わりにユーザー アカウントを使って既定 RunAs ポリシーを定義できますだけを同じ RunAs ユーザーで実行する必要がほとんどのコード パッケージが、アプリケーションで使用されるサービス マニフェストで指定した場合、 **RunAsPolicy** コード パッケージごとにします。 次に例を次の例を指定するコード パッケージにない場合 **RunAsPolicy** コード パッケージは指定すると、プリンシパルのセクションで指定された MyDefaultAccount で実行する必要があります。 

~~~
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
~~~

## http および https エンドポイントに対する SecurityAccessPolicy の割り当て
指定する必要があるかどうかは、サービスに RunAs ポリシーを適用して、サービス マニフェストは、http プロトコルでエンドポイント リソースを宣言して、 **SecurityAccessPolicy** にこれらのエンドポイントに割り当てられているポートが適切である、acl で制御用 RunAs ユーザー アカウント、サービスを実行していることを確認します。 そうしないと、http.sys はサービスにアクセスできず。クライアントからの呼び出しは失敗します。 次の例では、呼び出し先のエンドポイントに Customer3 アカウントが適用されます *ServiceEndpointName* のフル アクセス権を付与します。

~~~
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
~~~

Https エンドポイントを追加する必要があるをクライアントに戻るには、証明書の名前を示す、 **EndpointBindingPolicy** wuth 証明書が、アプリケーション マニフェストに証明書] セクションで定義されています。

~~~
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if the EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
~~~


## 完全なアプリケーション マニフェストの例
これまでに説明したさまざまな設定を示すアプリケーション マニフェストの例を次に示します。

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application3Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="Stateless1_InstanceCount" DefaultValue="-1" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
         <RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup" />
        <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
         <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
        <!--EndpointBindingPolicy is needed the EndpointName is secured with https -->
        <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
     </Policies>
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="Stateless1">
         <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
   <Principals>
      <Groups>
         <Group Name="LocalAdminGroup">
            <Membership>
               <SystemGroup Name="Administrators" />
            </Membership>
         </Group>
      </Groups>
      <Users>
         <User Name="LocalAdmin">
            <MemberOf>
               <Group NameRef="LocalAdminGroup" />
            </MemberOf>
         </User>
         <!--Customer1 below create a local account that this service runs under -->
         <User Name="Customer1" />
         <User Name="MyDefaultAccount" AccountType="NetworkService" />
      </Users>
   </Principals>
   <Policies>
      <DefaultRunAsPolicy UserRef="LocalAdmin" />
   </Policies>
   <Certificates>
     <EndpointCertificate Name="Cert1" X509FindValue="FF EE E0 TT JJ DD JJ EE EE XX 23 4T 66 "/>
  </Certificates>
</ApplicationManifest>
~~~


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## 次のステップ

* [アプリケーション モデルを理解する](service-fabric-application-model.md)
* [サービス マニフェストでのリソースの指定](service-fabric-service-manifest-resources.md)
* [アプリケーションをデプロイする](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png

