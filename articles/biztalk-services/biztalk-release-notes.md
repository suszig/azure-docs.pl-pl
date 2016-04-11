<properties 
    pageTitle="Azure BizTalk Services のリリース ノート | Microsoft Azure BizTalk Services" 
    description="" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="msftman" 
    manager="dwrede" 
    editor="cgronlun"/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/02/2015" 
    ms.author="deonhe"/>

# Azure BizTalk Services のリリース ノート

Microsoft Azure BizTalk Services リリース ノートでは、このリリースの既知の問題について説明します。

## BizTalk Services の 11 月の更新における新機能
* BizTalk Services ポータルで保存時の暗号化を有効にすることができます。 参照してください [BizTalk Services ポータルでの保存時の暗号化を有効にする](https://msdn.microsoft.com/library/azure/dn874052.aspx)です。

## 更新履歴

### 10 月の更新

* 組織アカウントが次のようにサポートされます。  
 * **シナリオ**: (user@live.com) のような Microsoft アカウントを使用して BizTalk サービスのデプロイメントを登録します。 このシナリオでは、Microsoft アカウント ユーザーのみが、BizTalk Services ポータルを使用してその BizTalk サービスを管理できます。 組織アカウントは使用できません。  
 * **シナリオ**: (user@fabrikam.com、user@contoso.com など) に、Azure Active Directory で組織のアカウントを使用して BizTalk サービスのデプロイメントを登録します。 このシナリオでは、同じ組織内の Azure Active Directory ユーザーのみが BizTalk Services ポータルを使用してその BizTalk サービスを管理できます。 Microsoft アカウントは使用できません。  
* Azure クラシック ポータルで BizTalk サービスを作成すると、ユーザーは BizTalk Services ポータルに自動的に登録されます。
 * **シナリオ**: BizTalk サービスを作成し、[Azure 旧ポータルにサインインする **管理** 非常に最初にします。 BizTalk Services ポータルが開くと、BizTalk サービスが自動的に登録され、デプロイできる状態になります。  
 参照してください [を登録して、BizTalk で BizTalk サービス デプロイの更新サービスのポータル](https://msdn.microsoft.com/library/azure/hh689837.aspx)します。  

### 8 月 14 日の更新
* 契約とブリッジの分離 - BizTalk Services ポータルで取引先契約とブリッジが分離されました。 契約とブリッジを個別に作成するようになりました。実行時には EDI メッセージの値に基づいてブリッジが契約に解決されます。 参照してください [Azure BizTalk サービスで契約を作成する](https://msdn.microsoft.com/library/azure/hh689908.aspx), 、[BizTalk サービス ポータルを使用して EDI ブリッジの作成](https://msdn.microsoft.com/library/azure/dn793986.aspx), 、[BizTalk サービス ポータルを使用して AS2 ブリッジの作成](https://msdn.microsoft.com/library/azure/dn793993.aspx), 、および [どのブリッジ実行時にアグリーメントを解決でしょうか。](https://msdn.microsoft.com/library/azure/dn794001.aspx)  
* 契約のテンプレートを作成するためのオプションが廃止されました。  
* 送信側契約は、スキーマごとに異なる区切り記号セットを指定できるようになりました。 この構成は、送信側契約のプロトコル設定で指定します。 詳細については、次を参照してください。 [X12 作成 Azure BizTalk サービスで契約](https://msdn.microsoft.com/library/azure/hh689847.aspx) と [Azure BizTalk サービスで EDIFACT アグリーメントを作成](https://msdn.microsoft.com/library/azure/dn606267.aspx)します。 同じ目的のために、TPM OM API にも 2 つの新しいエンティティが追加されました。 参照してください [X12DelimiterOverrides](https://msdn.microsoft.com/library/azure/dn798749.aspx) と [EDIFACTDelimiterOverride](https://msdn.microsoft.com/library/azure/dn798748.aspx)します。  
* 派生型を含む標準 XSD コンストラクトがサポートされるようになりました。 参照してください [をマップでの使用の標準 XSD コンストラクト](https://msdn.microsoft.com/library/azure/dn793987.aspx) と [マッピングのシナリオと例で使用する派生型](https://msdn.microsoft.com/library/azure/dn793997.aspx)します。  
* AS2 でメッセージ署名のための新しい MIC アルゴリズムと新しい暗号化アルゴリズムがサポートされるようになりました。 [参照してください Azure BizTalk サービスで AS2 アグリーメントを作成](https://msdn.microsoft.com/library/azure/hh689890.aspx)します。  
## 既知の問題

### BizTalk Services ポータル更新後の接続に関する問題

  BizTalk Services ポータルが開いているときに BizTalk Services をアップグレードし、サービスへの変更を取り込むと、BizTalk Services ポータルの接続に問題が発生することがあります。  
  対処法としては、ブラウザーを再起動するか、ブラウザーのキャッシュを削除するか、プライベート モードでポータルを起動します。  

### BizTalk Services プロジェクトのエラーまたは警告をクリックしたとき、Visual Studio IDE がアーティファクトを特定できない
この問題を解決するには、Visual Studio 2012 Update 3 RC 1 をインストールします。  

### カスタム バインド プロジェクトの参照
Visual Studio ソリューションの BizTalk Services プロジェクトでの次の状況について考えてみます。  
* 同じ Visual Studio ソリューションに、BizTalk Services プロジェクトとカスタム バインド プロジェクトがある。 BizTalk Services プロジェクトには、このカスタム バインド プロジェクト ファイルへの参照が含まれている。 
* BizTalk Services プロジェクトには、このカスタム バインド/動作 DLL への参照が含まれている。

Visual Studio では、ソリューションが正常に "ビルド" されます。 その後、ソリューションを "リビルド" または "クリーン" した その後、リビルドまたはクリーンを再度する場合は、次のエラーが発生します。  
  ファイルをコピーできません。 <Path to DLL> "bin\Debug\FileName.dll"です。 ファイル ‘bin\Debug\FileName.dll’ は、別のプロセスで使用されているため、アクセスできません。  

#### 対処法
* 場合 [Visual Studio 2012 Update 3](https://www.microsoft.com/download/details.aspx?id=39305) がインストールされている、次の 2 つのオプションがあります。

  * Visual Studio を再起動します。

  * ソリューションを再起動します。 起動後、ソリューションのビルドのみを実行します。  

* 場合 [Visual Studio 2012 Update 3](https://www.microsoft.com/download/details.aspx?id=39305) がインストールされている、開かれているタスク マネージャー、タブをクリックして、プロセス、MSBuild.exe プロセス] をクリックし、[プロセスの終了] をクリックします。  

### 印刷不可能な文字が HTTP ヘッダーとして昇格されている場合、ブリッジと BizTalk Services ポータルから BasicHttpRelay エンドポイントにルーティングできない

メッセージの昇格されたプロパティの一部として印刷不可能な文字を使用している場合は、BasicHttpRelay バインドを使用しているリレー先にそのメッセージをルーティングすることはできません。 また、追跡の一部として使用できる昇格されたプロパティは、BLOB 用には URL にエンコードされますが、送信先用にはエンコードされません。  

### [非同期 MDN の送信] オプションが選択されていない場合でも、MDN が非同期で送信される  
選択する場合は、このシナリオを検討してください、 **非同期 MDN の送信** チェック ボックスをオンし、[非同期 MDN を送信するための URL を指定に、ボックスをオフにし、 **非同期 MDN の送信** チェック ボックスをもう一度、MDN が引き続き送信指定された URL へ非同期 Mdn を送信するオプションが選択されていない場合でもです。  
この問題を回避するには、オフにする前に指定 URL を消去する必要があります、 **非同期 MDN の送信** チェック ボックスをオンし、AS2 アグリーメントを展開します。  

### 有効なインターチェンジの後に空白文字があると、保留エンドポイントに空のメッセージが送信される  
IEA セグメントの後に空白文字があると、逆アセンブラーはそこを現在のインターチェンジの終端として処理し、その後の一連の空白文字を次のメッセージと見なします。 後のメッセージは有効なインターチェンジではないため、1 件の成功メッセージがルーティング先に送信され、1 件の空のメッセージが保留エンドポイントに送信される場合があります。  
### BizTalk Services ポータルにおける追跡  
追跡イベントは EDI メッセージ処理と相関関係までキャプチャされます。 プロトコル ステージとは別の要素でメッセージにエラーが発生した場合、追跡は正常と表示されます。 このような状況では、下のセクションでログを参照してください、 **詳細** 内の列 **追跡** エラーの詳細。
X12 受信および送信の設定 ([X12 作成 Azure BizTalk サービスで契約](https://msdn.microsoft.com/library/azure/hh689847.aspx)) プロトコル段階の情報を提供します。  

### 契約の更新  
BizTalk Services ポータルでは、契約の構成時に ID の修飾子を変更できます。 結果として、プロパティに一貫性がなくなる可能性があります。 たとえば、修飾子として ZZ:1234567 および ZZ:7654321 を使用する契約があるとします。 BizTalk Services ポータルのプロファイル設定で、ZZ:1234567 を 01:ChangedValue に変更し、 契約を開くと、ZZ:1234567 ではなく 01:ChangedValue が表示されます。
Id の修飾子の変更、契約の削除、更新 **アイデンティティ** パートナー プロファイルおよびアグリーメントを再作成します。  
> AZURE.WARNING この動作は X12 と AS2 に影響を与えます。  

### AS2 添付ファイル  
AS2 メッセージの添付ファイルは送信と受信のどちらでもサポートされていません。 具体的には、添付ファイルは確認なしで無視され、メッセージ本文は通常の AS2 メッセージとして処理されます。  
### リソース: パスの記憶  
追加するときに **リソース**, 、リソースを追加する前に使用されるパスがダイアログ ウィンドウに保持されていない可能性があります。 以前使用したパスを注意してくださいには、BizTalk サービス ポータル web サイトを追加してみて **信頼済みサイト** Internet Explorer でします。  
### ブリッジのエンティティ名を変更し、変更を保存しないでプロジェクトを閉じると、再度エンティティを開いたときにエラーが発生する
次の順序で操作が実行されるシナリオが該当します。  
* ブリッジ (XML 一方向ブリッジなど) を BizTalk Services プロジェクトに追加する。  

* ブリッジの名前を変更するために [エンティティ名] プロパティの値を指定する。 これによって、関連する .bridgeconfig ファイルの名前が指定した名前に変更される。  

* 変更を保存しないで .bcs ファイルを閉じる (Visual Studio のタブを閉じる)。  

* ソリューション エクスプローラーから再度 .bcs ファイルを開く。  
関連する .bridgeconfig ファイルは指定した新しい名前に変更されているのに対して、デザイン画面のエンティティ名は以前の名前のままです。 ブリッジ コンポーネントをダブルクリックし、ブリッジの構成を開こうとすると、次のエラーが発生します。  
  ‘<old name>' エンティティに関連付けられたファイル '<old name>.bridgeconfig' が存在しません。  
このシナリオを避けるために、変更を保存すると、BizTalk サービス プロジェクト内のエンティティの名前を変更した後になっていることを確認します。  
### アーティファクトが Visual Studio プロジェクトから除外されているのに、BizTalk サービス プロジェクトのビルドが成功する
アーティファクト (XSD ファイルなど) を BizTalk サービス プロジェクトに追加し、そのアーティファクトをブリッジ構成に含めた (たとえば、要求メッセージの種類としてそれを指定した) 後で、Visual Studio プロジェクトから除外したシナリオが該当します。 このような場合、除外したアーティファクトがディスク上の、Visual Studio プロジェクトに含めたときと同じ場所にある限り、プロジェクトをビルドしてもエラーは発生しません。
### BizTalk サービス プロジェクトで、ブリッジの構成時にスキーマの可用性がチェックされない
BizTalk サービス プロジェクトでは、プロジェクトに追加されたスキーマが別のスキーマをインポートした場合、インポートされたスキーマがプロジェクトに追加されたかどうかをチェックしません。 そのようなプロジェクトをビルドしても、ビルド エラーは発生しません。
### XML 要求-応答ブリッジの応答メッセージの文字セットが常に UTF-8 になる
このリリースでは、XML 要求-応答ブリッジの応答メッセージの文字セットは常に UTF-8 に設定されます。
### ユーザー定義のデータ型
BizTalk アダプター サービス機能に含まれている BizTalk Adapter Pack アダプターでは、アダプター操作でユーザー定義のデータ型を使用できます。
ユーザー定義のデータ型を使用する際は、ファイル (.dll) をドライブ \Program Files\Microsoft BizTalk Adapter Service\BAServiceRuntime\bin\ か BizTalk アダプター サービスをホストしているサーバーのグローバル アセンブリ キャッシュ (GAC) にコピーします。 コピーしなかった場合、クライアントで次のエラーが発生する可能性があります。  
```<s:Fault xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <faultcode>s:Client</faultcode>
  <faultstring xml:lang="en-US">The UDT with FullName "File, FileUDT, Version=Value, Culture=Value, PublicKeyToken=Value" could not be loaded. Try placing the assembly containing the UDT definition in the Global Assembly Cache.</faultstring>
  <detail>
    <AFConnectRuntimeFault xmlns="http://Microsoft.ApplicationServer.Integration.AFConnect/2011" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <ExceptionCode>ERROR_IN_SENDING_MESSAGE</ExceptionCode>
    </AFConnectRuntimeFault>
  </detail>
</s:Fault> ```  
> [AZURE.IMPORTANT] It is recommended to use GACUtil.exe to install a file into the Global Assembly Cache. GACUtil.exe documents how to use this tool and the Visual Studio command line options.  

### Restarting the BizTalk Adapter Service Web Site
Installing the **BizTalk Adapter Service Runtime*** creates the **BizTalk Adapter Service** web site in IIS that contains the **BAService** application. **BAService** application internally uses relay binding to extend the reach of on-premise service endpoint to the cloud. For a service hosted on-premises, the corresponding relay endpoint will be registered on the Service Bus only when the on-premises service starts.  

If you stop and start an application, the configuration for auto-starting an application is not honored. So when **BAService** is stopped, you must always restart the **BizTalk Adapter Service** web site instead. Do not start or stop the **BAService** application.
### Special characters should not be used for address and entity names of LOB components
You should not use special characters for address and entity names of LOB components. If you do so, you will get an error while deploying the BizTalk Service project. For certain characters like ‘%’, the BizTalk Adapter Service website might go into a stopped state and you will have to manually start it.
### Test Map with Get Context Property
If a Transform contains a **Get Context Property** Map Operation, **Test Map** will fail. As a temporary workaround, replace the **Get Context Property** Map Operation with a String Concatenate Map Operation containing dummy data. This will populate the target schema and allow you test other Transform functionality.
### Test Map Property does not display
The **Test Map** properties do not display in Visual Studio. This can occur if the **Properties** window and the **Solution Explorer** window are not simultaneously docked. To resolve this, dock the **Properties** and the **Solution Explorer** windows.  
### DateTime Reformat drop-down is grayed out
When a DateTime Reformat Map Operation is added to the design surface and configured, the Format drop-down list may be grayed out. This can happen if the computer Display is set **Medium – 125%** or **Larger – 150%**. To resolve, set the display to **Smaller – 100% (default)** using the steps below:  
1. Open the **Control Panel** and click **Appearance and Personalization**.
2. Click **Display**.
3. Click **Smaller – 100% (default)** and click **Apply**.

The **Format** drop-down list should now work as expected.
### Duplicate agreements in the BizTalk Services Portal
Consider the following scenario:
1. Create an agreement using the Trading Partner Management OM API.
2. Open the agreement in the BizTalk Services Portal in two different tabs.
3. Deploy the agreement from both the tabs.
4. As a result, both the agreements get deployed resulting in duplicate entries in the BizTalk Services Portal

**Workaround**. Open any one of the duplicate agreements in the BizTalk Services Portal, and undeploy.  

### Bridges do not use updated certificate even after a certificate has been updated in the artifact store
Consider the following scenarios:  

**Scenario 1: Using thumbprint-based certificates for securing message transfer from a bridge to a service endpoint**  
Consider a scenario where you use thumbprint-based certificates in your BizTalk Service project. You update the certificate in the BizTalk Services Portal with the same name but a different thumbprint, but do not update the BizTalk Service project accordingly. In such a scenario, the bridge might continue to process the messages because the older certificate data might still be in the channel cache. After that, message processing fails.  

**Workaround**: Update the certificate in the BizTalk Service project and redeploy the project.  

**Scenario 2: Using name-based behaviors to identify certificates for securing message transfer from a bridge to a service endpoint** 

Consider a scenario where you use name-based behaviors to identify certificates in your BizTalk Service project. You update the certificate in the BizTalk Services Portal but do not update the BizTalk Service project accordingly. In such a scenario, the bridge might continue to process the messages because the older certificate data might still be in the channel cache. After that, message processing fails.  

**Workaround**: Update the certificate in the BizTalk Service project and redeploy the project.  

### Bridges continue to process messages even when the SQL database is offline
The BizTalk Services bridges continue to process messages for a while, even if the Microsoft Azure SQL Database (which stores the running information like deployed artifacts and pipelines), is offline. This is because BizTalk Services uses the cached artifacts and bridge configuration.
If you do not want the bridges to process any messages when the SQL Database is offline, you can use the BizTalk Services PowerShell cmdlets to stop or suspend the BizTalk Service. See [Azure BizTalk Service Management Sample](http://go.microsoft.com/fwlink/p/?LinkID=329019) for the Windows PowerShell cmdlets to manage operations.  
### Reading the XML message within a bridge’s custom code component includes an extra BOM character
Consider a scenario where you want to read an XML message within a bridge’s custom code. If you use the .NET API System.Text.Encoding.UTF8.GetString(bytes) an extra BOM character is included in the output at the beginning of the message. So, if you do not want the output to include the extra BOM character, you must use ```System.IO.StreamReader().ReadToEnd()```.
### Sending messages to a bridge using WCF does not scale
Messages sent to a bridge using WCF does not scale. You should instead use HttpWebRequest if you want a scalable client.
### UPGRADE: Token Provider error after upgrading from BizTalk Services Preview to General Availability (GA)
There is an EDI or AS2 Agreement with active batches. When the BizTalk Service is upgraded from Preview to GA, the following may occur:
* Error: The token provider was unable to provide a security token. Token provider returned message: The remote name could not be resolved.

* Batch tasks are canceled.

**Workaround**: After the BizTalk Service is updated to General Availability (GA), redeploy the agreement.  

### UPGRADE: Toolbox shows the old bridge icons after upgrading the BizTalk Services SDK
After you upgrade an earlier version of the BizTalk Services SDK, which had old icons representing the bridges, the toolbox continues to show the old icons for the bridges. However, if you add a bridge to BizTalk Service project designer surface, the surface shows the new icon.  

**Workaround**. You can work around this issue by deleting the .tbd files under <system drive>:\Users\<user>\AppData\Local\Microsoft\VisualStudio\11.0.  

### UPGRADE: BizTalk Portal update from Preview to GA might show an error indicating that the EDI capability is not available
If you are logged into the BizTalk Services Portal while the BizTalk Services is upgraded from Preview to GA, you might get the following error on the portal:  

This capability is not available as part of this edition of Microsoft Azure BizTalk Services. To use these capabilities switch to an appropriate edition.  

**Resolution**: Log out from the portal, close and open the browser, and then log into the portal.  
### UPGRADE: New tracking data does not show up after BizTalk Services is upgraded to GA
Assume a scenario where you have an XML bridge deployed on BizTalk Services Preview subscription. You send messages to the bridge and the corresponding tracking data is available on the BizTalk Services Portal. Now, if the BizTalk Services Portal and BizTalk Services runtime bits are upgraded to GA, and you send a message to the same bridge endpoint deployed earlier, the tracking data does not show up for messages sent after upgrade.  

### Pipelines v/s Bridges
Throughout this document, the term ‘pipelines’ and ‘bridges’ are used interchangeably. Both essentially mean the same thing, which is, a message processing unit deployed on BizTalk Services.  

### Concepts  

[BizTalk Services](https://msdn.microsoft.com/library/azure/hh689864.aspx)   





