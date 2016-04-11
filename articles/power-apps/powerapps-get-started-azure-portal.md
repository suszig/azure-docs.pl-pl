<properties
    pageTitle="PowerApps Enterprise の説明と、使用を開始する方法 | Microsoft Azure"
    description="PowerApps Enterprise の使用を開始し、アプリ サービス環境を作成する"
    services=""
    suite="powerapps"
    documentationCenter="" 
    authors="linhtranms"
    manager="dwrede"
    editor=""/>

<tags
   ms.service="powerapps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="11/29/2015"
   ms.author="litran"/>

# Microsoft PowerApps Enterprise とは?

Microsoft PowerApps Enterprise は、新しい Microsoft Azure サービスです。 PowerApps Enterprise では、企業内のビジネス ユーザー向けにモバイル アプリの作成機能を強化し、IT 管理者がこれらのアプリを厳密に管理できるようにしています。 

リボンや Excel の数式が組み込まれた Office スタイルのインターフェイスを使用することで、ビジネス ユーザーは次のような機能を持つアプリを作成できます。

- Excel の場合と同様に、折れ線グラフ、円グラフ、横棒グラフを使用してデータを表示する。
- ボタン、テキストの挿入機能、日付の書式設定機能を含むユーザー インターフェイスを作成する。
- リスト ボックス、ドロップダウン リスト、およびオプション ボタンなどの複数の選選択コントロールを追加する。
- イメージのアップロード、写真の撮影、オーディオ/ビデオ ファイルの再生を行う。
- Excel や SQL Server などの ’バックエンド’ システムを使用して情報を表示および更新する。
- Twitter や SharePoint などの PaaS プログラムに接続するアプリに、ビルド済み App Service コネクタを追加する。

IT 管理者は、企業内のビジネス ユーザーによって作成されたアプリを次のように管理できます。

- 作成されたアプリの管理、およびアプリへのユーザー アクセスの管理を行う。
- API を作成し、さまざまなデータ ソースへの接続を作成する 
- API に対するユーザー アクセスを管理して、これらのデータ ソースへの接続を管理する。 

## 開始するには?

まず、新しい Azure Active Directory (Azure AD) テナントを作成する必要があるかどうかを判定します。 Api との接続を追加し、管理を開始する場合は、AD テナントを持っている既にを Azure ポータルで PowerApps エンタープライズを有効にするだけで (開始 *手順 2.* 」を参照)。 

AD テナントをまだ所有していない場合は、新しい AD テナントを作成し、Azure ポータルでPowerApps Enterprise を有効にし、API と接続を追加し、管理を開始します。 

このトピックでは詳細な手順を示します。

## 手順 1: 新しい Azure AD テナントを作成する、または既存の Azure AD テナントを使用する

PowerApps Enterprise の使用を開始するには、Azure Active Directory (Azure AD) テナントが必要です。 テナントは、Azure AD サービスの専用のインスタンスです。 

組織または企業が Microsoft Intune または Office 365 などの Microsoft Azure クラウド サービスにサインインすると、組織は自動的に AD テナントを受信し所有するようになります。 各 AD テナントは、特異なものであり他の Azure AD テナントとは分離されます。 

次の手順を使用して、テナントを既に所有しているかどうかを判定するか、または新しいテナントを作成する方法を決定します。

#### 既存の Office 365 サブスクリプションを所有している
Office 365 サブスクリプション (または Microsoft Dynamic CRM Online、Enterprise Mobility Suite、またはその他の Microsoft サービス) を既に所有している場合は、Azure Active Directory への無料のサブスクリプションが提供されます。 Azure AD を使用して、ユーザー アカウントとグループ アカウントを作成し管理することができます。 Azure ポータルにサインインできない場合は、おそらくサブスクリプションをアクティブにする必要があります。 このようにするには、 [Azure クラシック ポータル](https://manage.windowsazure.com/), 、1 回限りで登録処理を完了します。 これらを使用して [手順](https://technet.microsoft.com/library/dn832618.aspx) Azure AD テナントにアクセスするためにします。 

#### Microsoft アカウントに関連付けられた既存の Azure サブスクリプションを所有している
以前に個人の Microsoft アカウント (hotmail または live) で Azure サブスクリプションにサインアップしている場合は、既にテナントを所有しています。  [Azure クラシック ポータル](https://manage.windowsazure.com/), 、**既定のテナント** ] 下にある **すべてのアイテム** [ **Active Directory**します。 このテナントは自由に使用できますが、組織の管理者アカウントを作成する必要がある場合があります。

そのためには、次の手順を実行してください。 または、新しいテナントを作成し、同様のプロセスでそのテナントに管理者を作成することもできます。

1.  サインイン、 [Azure クラシック ポータル](https://manage.windowsazure.com/) 、個々 のアカウントを使用します。
2.  選択 **Active Directory**"で、左側のメニュー バーです。 
3.  選択 **既定のディレクトリ** 使用可能なディレクトリの一覧にします。
4.  選択、 **ユーザー** 上部にあるタブをクリックします。 1 人のユーザーが [ソース ディレクトリ] 列に “Microsoft アカウント” と共に一覧表示されます。
5.  選択 **ユーザーの追加** 下部にあります。 
6.   **ユーザーの追加フォーム**, 、次の詳細を入力します。  
    
    プロパティ |説明
--- | ---
ユーザーの種類 |組織内の新しいユーザー
ユーザー名 |この管理者のユーザー名を選択します。
名/名前/表示名を過去 |値を入力します。
役割 |グローバル管理者
代替メール アドレス |値を入力してください。
省略可能な |多要素認証を有効にします。  
    
    選択、 **作成** を完了し、一時的なパスワードを表示するボタンをクリックします。

終了したら、新しい管理ユーザー用の一時的なパスワードを記録します。 一時的なパスワードを変更するにサインイン [https://login.microsoftonline.com](https://login.microsoftonline.com) この新しいユーザー アカウントおよびパスワードを変更します。 連絡用電子メールを使用して、ユーザーにパスワードを直接送信することもできます。


#### 組織のアカウントに関連付けられた既存の Azure サブスクリプションを所有している
以前に組織のアカウントで Azure サブスクリプションにサインアップしている場合は、既にテナントを所有しています。  [Azure クラシック ポータル](https://manage.windowsazure.com/), 、テナントが下に一覧表示 **すべてのアイテム** も [ **Active Directory**します。 必要に応じて、このテナントを自由に使用できます。 使用して新しいテナントを作成することも、 **新規** ] メニューの下部にあるタスク バーにします。

#### 上記のいずれも所有しておらず、最初から始める
上記のいずれもしないする場合に移動し、 [https://account.windowsazure.com/organization](https://account.windowsazure.com/organization) 新しい組織で Azure にサインアップします。 サインアップすると、選択したドメイン名が付けられた独自の Azure AD テナントを取得できます。  [Azure クラシック ポータル](https://manage.windowsazure.com/), でテナントを確認できます **Active Directory** 、左側のメニュー。

## 手順 2: 新しい Azure サブスクリプションを作成するか、または既存のものを使用する
AD テナントが用意できましたので、次に新しい Azure サブスクリプションを作成することも、既存のものを使用することもできます。 Azure AD サブスクリプションには複数のエディションがあります。 PowerApps Enterprise の場合は、Free エディションを使用することができます。 ただし、AAD プロキシを使用して、オンプレミスのデータへのハイブリッド接続を作成する必要がある場合は、Basic エディションまたは Premium エディションが必要です。 

[Azure Active Directory のエディション](../active-directory/active-directory-editions.md) より多くの機能を一覧表示します。 


## 手順 3: Azure 作業サブスクリプションで PowerApps Enterprise にサインアップする
> [AZURE.NOTE] 次の手順では、サブスクリプションの管理者は、Azure ポータルにサインインし、要求を送信する必要があります。 

AD テナントと Azure サブスクリプションが準備できましたので、作業サブスクリプションの管理者は次に PowerApps Enterprise にサインアップできます。 管理者はまた、社内のユーザーを '管理' PowerApps に追加し (ユーザーにアクセス許可を与えることも含む)、Azure サブスクリプションに公開された PowerApps を管理することができます。 

PowerApps 企業にサインアップすることがなくブレードが表示、なしのアクセスを行うとき [Azure ポータル](https://portal.azure.com/) し PowerApps を参照します。  サインアップする、会社、 **サブスクリプション管理者** に移動できます [PowerApps](http://go.microsoft.com/fwlink/p/?LinkId=716848) の料金およびサインアップ プロセスの詳細についてお問い合わせです。 

![][4]  

サインアップ プロセスを完了し、PowerApps Enterprise を使用する準備が整ったら、次のことができるようになります。 

- 企業内でユーザーを追加しを使用して [ロールベースのアクセス制御](../role-based-access-control-configure.md), 、PowerApps エンタープライズ ポータルにアクセスするロールを PowerApps Admin ユーザーに付与します。
- PowerApps をホストする専用のアプリ サービス環境を作成します。
- 専用のアプリ サービス環境内で実行される API と接続を作成します。
- PowerApps で作成したアプリだけでなく、Web アプリ、モバイル アプリ、API アプリ、ロジック アプリなどのアプリもアプリ サービス環境に追加することができます。

次の例では、Contoso 社が PowerApps にサインアップしています。 この新しい **PowerApps** ブレードで、この app service 環境を使用して作成されたアプリのさまざまな種類の概要を表示できます。  **管理 Api**, 、Microsoft が作成した Api (マイクロソフトの管理) の概要を確認し、Contoso が作成した Api (IT 管理) を参照してください。  

![企業の PowerApps ブレードのサンプル][3]  


## 手順 4: アプリ サービス環境を作成する
PowerApps の API と接続に加えて、モバイル アプリ、Web アプリ、API アプリ、およびロジック アプリをホストするアプリ サービス環境を作成します。 

アプリ サービス環境は、すべてのアプリを安全に実行するための分離された専用環境です。 コンピューティング リソースは、アプリ サービス環境ごとに有効になり、お使いのアプリを実行するためだけに使用されます。 PowerApps Enterprise にサインアップすると、アプリで使用される API と接続は専用のアプリ サービス環境を使用してホストされます。 このアプリ サービス環境は、"特殊な" タイプのアプリ サービス環境です。 具体的には次の処理が行われます。 

- このアプリ サービス環境は、任意の目的で使用できます。 これは、サブスクリプションではなく、会社に関連付けられます。
- PowerApps で作成したアプリで使用されるように、API と接続を構成します。 ただし、前述アプリ サービス環境には、Web アプリ、モバイル アプリ、ロジック アプリ、および API アプリを追加することもできます。 
- 課金は固定で、PowerApps Enterprise に含められます。  
- スケールは自動的に管理されます。 追加のコンピューティング リソースが必要なかどうかを判断するために、環境を監視する必要はありません。

通常の Azure アプリ サービス環境にはさまざまな機能があります。 参照してください [App Service 環境の概要](../app-service-app-service-environment-intro.md) います。

#### 使用を開始するための前提要件

- Azure の企業サブスクリプション
- 企業内でサブスクリプション管理者 [PowerApps については、会社をサインアップ](powerapps-get-started-azure-portal.md) Enterprise。
- PowerApps 管理者 (PowerApps の "所有者") またはサブスクリプション管理者として Azure ポータルにサインインしている。

### アプリ サービス環境を作成する
> [AZURE.NOTE] App service 環境を作成するオプションが表示されない場合は、テナントの作成が既にします。 詳細を表示するには、[ **設定** を app service 環境を開きます。

1.  [Azure ポータル](https://portal.azure.com/), 、仕事用アカウントでサインインします。 たとえばでサインイン *yourUserName*@*yourcompany ' と*. com です。 これにより、会社のサブスクリプションに自動的にサインインされます。
 
2. 選択 **参照** タスク バーで。  
![PowerApps を参照します。][1]
  
3. 一覧で、PowerApps を検索または入力するスクロールできる *powerapps*:  
![PowerApps の検索][2]  

4.  **PowerApps** ブレードで、 **App Service 環境の作成を始める** または選択 **App Service 環境** [ *設定*:  
![][5]

    > [AZURE.NOTE] クリックすると **App Service 環境の作成を始める**, 、App Service 環境の詳細を 1 つの余分なブレードが表示されます。 そのブレード上で作成リンクをクリックするだけで、作成ブレードが起動します。 

5. 次に、名前を入力し、使用するサブスクリプションを選択し、新しいリソース グループを選択または作成し、仮想ネットワークを選択します。 **通知** 仮想ネットワークを選択した後、変更できないこと。  
![][6]  
詳細については仮想ネットワークは、app service 環境を操作する方法を参照してください [App Service 環境を作成する方法](../app-service-web-how-to-create-an-app-service-environment.md)します。 

6. 選択 **追加** app service 環境の作成を完了します。 

> [AZURE.TIP] PowerApps を使用して app service 環境を作成する場合は、コンピューティング リソース プールの構成に求められません。 この手順は自動的に処理されます。

このアプリ サービス環境には、Web アプリ、モバイル アプリ、API アプリを追加することもできます。 実際には、App Service Environment でサポートされている任意のものを追加するための環境です。

### App Service Environment を管理する管理者を追加します。

アプリ サービス環境にアクセスするには、API や接続などのリソースを作成し、ユーザーを所有者ロールで追加する必要があります。

1. 先ほど作成したアプリ サービス環境を選択します。
2. Essentials での選択、 **リソース グループ** プロパティです。 App service 環境を含むリソース グループが開きます。  
![][7]
3. アクセス許可を管理する RBAC アイコンを選択します。  
![][8]  
    ユーザーを追加して、ロールの割り当てを使用するよう、 [ロールベースのアクセス制御](https://azure.microsoft.com/documentation/articles/role-based-access-control-configure) Azure 内で。

> [AZURE.NOTE] 現時点では、app service 環境の RBAC のアクセス許可を与えることはできません。 親のリソース グループ レベルでは RBAC アクセス許可を付与することができます。

## まとめと次のステップ
これで会社は PowerApps にサインアップし、アプリ サービス環境を取得しました。 次に、アプリで使用可能な API と接続を追加することができます。 

- [PowerApps アプリを監視する](powerapps-manage-monitor-usage.md)
- [PowerApps 用 API の開発](powerapps-develop-api.md)
- [新しい API の追加、接続の追加、ユーザーへのアクセス許可](powerapps-manage-api-connection-user-access.md)
- [既存 API とプロパティの更新](powerapps-configure-apis.md)


[1]: ./media/powerapps-get-started-azure-portal/browseall.png
[2]: ./media/powerapps-get-started-azure-portal/allresources.png
[3]: ./media/powerapps-get-started-azure-portal/powerappsblade.png
[4]: ./media/powerapps-get-started-azure-portal/noaccess.png
[5]: ./media/powerapps-get-started-azure-portal/createase.png
[6]: ./media/powerapps-get-started-azure-portal/aseproperties.png
[7]: ./media/powerapps-get-started-azure-portal/aseessentials.png
[8]: ./media/powerapps-get-started-azure-portal/resourcegrouprbac.png


