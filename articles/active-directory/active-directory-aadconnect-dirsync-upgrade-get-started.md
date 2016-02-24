<properties
   pageTitle="Azure AD Connect: Windows Azure AD 同期ツール (DirSync) からのアップグレード | Microsoft Azure"
   description="DirSync から Azure AD Connect にアップグレードする方法について説明します。  この記事では、Windows Azure AD 同期ツール (DirSync) を Azure AD Connect へアップグレードするための手順について説明します。"
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="12/16/2015"
   ms.author="shoatman;billmath"/>

# Windows Azure Active Directory 同期 (DirSync) の Azure AD Connect へのアップグレード

次のドキュメントは、既存の DirSync インストールの Azure AD Connect へのアップグレードに役立ちます。

## 関連ドキュメント
上のドキュメントを読んでいない場合 [内部設置型 id と Azure Active Directory の統合](active-directory-aadconnect.md), 、次の表は、関連するトピックへのリンクを提供します。 DirSync からのアップグレードを開始する前に、太字で表示した最初の 2 つのトピックをお読みいただく必要があります。

| トピック |  |
| --------- | --------- |
| **Azure AD Connect のダウンロード** | [Azure AD Connect のダウンロード](http://go.microsoft.com/fwlink/?LinkId=615771) |
| **ハードウェアと前提条件** | [Azure AD Connect: ハードウェアと前提条件](active-directory-aadconnect-prerequisites.md) |
| **インストールで使用するアカウント** | [Azure AD Connect アカウントとアクセス許可の詳細](active-directory-aadconnect-accounts-permissions.md) |

## DirSync からのアップグレード
現在の DirSync のデプロイメントに応じて、アップグレードのさまざまなオプションがあります。 予想されるアップグレード時間が 3 時間未満の場合は、インプレース アップグレードを実行することをお勧めします。 予想されるアップグレード時間が 3 時間を超える場合は、別のサーバーで並列デプロイメントを行うことをお勧めします。 オブジェクトの数が 50,000 を超える場合は、アップグレード時間が 3 時間を超えることが予想されます。

| シナリオ | |
| ---- | ---- |
| [インプレース アップグレード](#in-place-upgrade)  | アップグレード時間が 3 時間未満と予想される場合に推奨されるオプションです。 |
| [並列デプロイ](#parallel-deployment) | アップグレード時間が 3 時間を超えると予想される場合に推奨されるオプションです。 |

>[AZURE.NOTE] DirSync から Azure AD Connect にアップグレードする場合をアンインストールしないでディレクトリ同期自分でアップグレードする前にします。 Azure AD Connect が DirSync から構成を読み取って移行し、サーバーを検査した後に、アンインストールします。

**インプレース アップグレード**

ウィザードに、アップグレードを完了する予定時刻が表示されます。  この推定値は、50,000 のオブジェクト (ユーザー、連絡先、およびグループ) を含むデータベースのアップグレードが完了するまでに 3 時間かかるという前提に基づいています。  Azure AD Connect は、現在の DirSync の設定を分析し、データベース内のオブジェクトの数が 50,000 未満である場合は、インプレース アップグレードをお勧めします。  続行すると、現在の設定がアップグレード中に自動的に適用され、サーバーが自動的にアクティブな同期を再開します。

構成の移行を実行して、並列デプロイメントを実行する場合は、インプレース アップグレードに関する推奨事項をオーバーライドできます。 たとえば、ハードウェアとオペレーティング システムを更新することができます。 参照してください、 [並列展開](#parallel-deployment) の詳細セクションです。

**並列デプロイ**

オブジェクトの数が 50,000 を超える場合は、並列デプロイメントの使用をお勧めします。 これにより、ユーザーによって発生する操作時の遅延が回避されます。 Azure AD Connect のインストールでは、アップグレードのためのダウンタイムを予測しますが、過去に DirSync をアップグレードしたことがある場合は、その経験が最善の指標となります。

### アップグレード対象の、サポートされている DirSync の構成
DirSync では次の構成の変更が サポートされており、アップグレードされます。

- ドメインと OU のフィルター処理
- 別の ID (UPN)
- パスワード同期と Exchange ハイブリッドの設定
- フォレスト/ドメインと Azure AD の設定
- ユーザー属性に基づくフィルター処理

次のような変更をアップグレードすることはできません。 これらの変更を加えた場合は、アップグレードがブロックされます。

![アップグレードのブロック](./media/active-directory-aadconnect-dirsync-upgrade-get-started/analysisblocked.png)

ような場合、推奨設定は [新しい Azure AD Connect サーバーをインストールする [ステージング モード](active-directory-aadconnectsync-operations.md#staging-mode) し古いディレクトリ同期と新しい Azure AD Connect の構成を確認します。 」の説明に従って、カスタム構成を使用して変更を再適用 [カスタム構成を Azure AD Connect Sync](active-directory-aadconnectsync-whatis.md)します。

- サポートされていない DirSync の変更 (削除された属性やカスタム拡張 DLL の使用など)

DirSync がサービス アカウントで使用したパスワードは取得できず、移行されません。 これらのパスワードはアップグレード中にリセットされます。

### DirSync から Azure AD Connect へのアップグレードの大まかな手順

1. Azure AD Connect へようこそ
2. 現在の DirSync 構成の分析
3. Azure AD のグローバル管理者のパスワードの収集
4. エンタープライズ管理者アカウントの資格情報の収集 (Azure AD Connect のインストール時にのみ使用)
5. Azure AD Connect のインストール
    * DirSync をアンインストールします。
    * Azure AD Connect をインストールする
    * 必要に応じて、同期を開始します。

次の場合、追加の手順が必要になります。

* 完全バージョンの SQL Server を現在使用している場合 - ローカルまたはリモート
* 同期するオブジェクトが 50,000 以上ある場合

## インプレース アップグレード

1. Azure AD Connect インストーラー (MSI) を起動します。
2. ライセンス条項とプライバシーに関する声明を確認し、同意します。
![Azure AD へようこそ](./media/active-directory-aadconnect-dirsync-upgrade-get-started/Welcome.png)
3. 次をクリックすると、既存の DirSync インストールの分析を開始します。
![既存のディレクトリ同期のインストールの分析](./media/active-directory-aadconnect-dirsync-upgrade-get-started/Analyze.png)
4. 分析が完了すると、続行方法の推奨事項が提示されます。  
    - SQL Server Express を使用して 5万台未満のオブジェクトがある場合は、次の画面が表示されます。
![分析が完了し、DirSync からアップグレードする準備が整いました。](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AnalysisReady.png)
    - ディレクトリ同期を完全な SQL Server を使用する場合は、このページを代わりに表示されます。
![分析が DirSync からアップグレードする準備が完了しました](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AnalysisReadyFullSQL.png)<BR/>
ディレクトリ同期で使用されている既存の SQL Server データベース サーバーに関する情報が表示されます。 必要に応じて、適切に調整を行います。 クリックして **次** インストールを続行します。
    - オブジェクトが 50,000 個を超える場合は、この画面を代わりに表示されます。
![分析が DirSync からアップグレードする準備が完了しました](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AnalysisRecommendParallel.png)<BR/>
インプレース アップグレードを続行するには、このメッセージの横にあるチェック ボックスをクリックします。 **このコンピューターに DirSync のアップグレードを続行します。**
行うには、 [並列展開](#parallel-deployment) 代わりにディレクトリ同期の構成設定をエクスポートして、新しいサーバーに移動します。
5. Azure AD への接続に現在使用しているアカウントのパスワードを入力します。 これは、DirSync によって現在使用されているアカウントでなければなりません。
![Azure ADの資格情報を入力します。](./media/active-directory-aadconnect-dirsync-upgrade-get-started/ConnectToAzureAD.png)
6. Active Directory のエンタープライズ管理者アカウントを指定します。
![ADDS の資格情報を入力する](./media/active-directory-aadconnect-dirsync-upgrade-get-started/ConnectToADDS.png)
7. 構成する準備が整いました。  クリックすると、 **アップグレード**, 、DirSync がアンインストールされ、Azure AD Connect が構成され、同期を開始します。
![構成の準備完了](./media/active-directory-aadconnect-dirsync-upgrade-get-started/ReadyToConfigure.png)


## 並列デプロイ

### DirSync の構成をエクスポートする
**並列デプロイメント - 50,000 以上のオブジェクト**

50,000 以上のオブジェクトがある場合、Azure AD Connect のインストールでは並列デプロイメントが推奨されます。

次のような画面が表示されます。

![分析完了](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AnalysisRecommendParallel.png)

並列デプロイを開始する場合は、次の手順を実行する必要があります。

- クリックして、 **設定をエクスポートする** ] ボタンをクリックします。  Azure AD Connect を別のサーバーにインストールすると、これらの設定がインポートされ、現在の DirSync から新しい Azure AD Connect インストールにすべての設定が移行されます。

設定が正常にエクスポートされたら、DirSync サーバーで、Azure AD Connect ウィザードを終了できます。 次の手順を続行 [別のサーバーでの Azure AD Connect のインストール](#installation-of-azure-ad-connect-on-separate-server)

**並列デプロイメント - 50,000 未満のオブジェクト**

オブジェクトの数が 50,000 未満の場合に並列デプロイメントを実行するには、次の手順を実行します。

1. Azure AD Connect インストーラー (MSI) を実行します。
2. 表示されている場合、 **Azure AD Connect へようこそ** 画面で、ウィンドウの右上隅で [X] をクリックして、インストール ウィザードを終了します。
3. コマンド プロンプトを開きます。
4. Azure AD Connect のインストール場所から (既定: C:\Program files \microsoft Azure Active Directory の接続)、次のコマンドを実行します。
    `AzureADConnect.exe /ForceExport`
5. クリックして、 **設定をエクスポートする** ] ボタンをクリックします。  Azure AD Connect を別のサーバーにインストールすると、これらの設定がインポートされ、現在の DirSync から新しい Azure AD Connect インストールにすべての設定が移行されます。

![分析完了](./media/active-directory-aadconnect-dirsync-upgrade-get-started/forceexport.png)

設定が正常にエクスポートされたら、DirSync サーバーで、Azure AD Connect ウィザードを終了できます。 次の手順を続行 [別のサーバーでの Azure AD Connect のインストール](#installation-of-azure-ad-connect-on-separate-server)

### 別のサーバーに Azure AD Connect をインストールする

Azure AD Connect を新しいサーバーにインストールする場合、Azure AD Connect のクリーン インストールを実行するものと見なされます。 DirSync の構成を使用する必要があるため、実行する手順が増えます。

1. Azure AD Connect インストーラー (MSI) を実行します。
2. 表示されている場合、 **Azure AD Connect へようこそ** 画面で、ウィンドウの右上隅で [X] をクリックして、インストール ウィザードを終了します。
3. コマンド プロンプトを開きます。
4. Azure AD Connect のインストール場所から (既定: C:\Program files \microsoft Azure Active Directory の接続)、次のコマンドを実行します。
    `AzureADConnect.exe /migrate`
    Azure AD Connect インストール ウィザードが起動され、次の画面に表示されます。
![Azure AD 資格情報を入力します。](./media/active-directory-aadconnect-dirsync-upgrade-get-started/ImportSettings.png)
5. DirSync インストールからエクスポートされた設定ファイルを選択します。
6. 次の高度なオプションを構成します。
    - Azure AD Connect のカスタムのインストール場所。
    - SQL Server の既存のインスタンス (既定: Azure AD Connect は、SQL Server 2012 Express をインストールします)。 DirSync サーバーと同じデータベース インスタンスは使用しないでください。
    - SQL Server への接続に使用するサービス アカウント (SQL Server データベースがリモートの場合、このアカウントはドメイン サービス アカウントにする必要があります)。
これらのオプションは、この画面に表示できます。
![Azure ADの資格情報を入力します。](./media/active-directory-aadconnect-dirsync-upgrade-get-started/advancedsettings.png)
7. クリックして **次**します。
8.  **を構成する準備ができて** ページでは、 **構成が完了したらすぐに同期プロセスを開始** チェックします。 サーバーがされる [ステージング モード](active-directory-aadconnectsync-operations.md#staging-mode) ため、この時点での Azure AD への変更はエクスポートされません。
9. クリックして **インストール**します。

>[AZURE.NOTE] Windows Server Active Directory と Azure Active Directory の間で同期が開始されますが、Azure AD への変更はエクスポートされません。  一度にアクティブにし変更をエクスポートできる同期ツールは 1 つだけです。 これと呼ばれる [ステージング モード](active-directory-aadconnectsync-operations.md#staging-mode)します。

### Azure AD Connect の同期を開始する準備が完了していることを確認する

Azure AD Connect が DirSync を開く必要がありますからを引き継ぐことを確認するために  **Synchronization Service Manager** グループ **Azure AD Connect** [スタート] メニューからです。

アプリケーション内で表示する必要がある、 **操作** ] タブをクリックします。 このタブでは、次の操作が完了していることを確認します。

- AD コネクタへのインポート
- Azure AD コネクタへのインポート
- AD コネクタへの完全な同期
- Azure AD コネクタへの完全な同期

![インポートと同期の完了](./media/active-directory-aadconnect-dirsync-upgrade-get-started/importsynccompleted.png)

これらの操作の結果を確認し、エラーが発生しないことを確認します。

下にある構成を確認する方法が、読み取りを確認し、Azure AD にエクスポートしようとしてされる変更を調べる場合は、 [ステージング モード](active-directory-aadconnectsync-operations.md#staging-mode)します。 予期しない内容が表示されなくなるまで構成の変更を行ってください。

上記の 4 つの操作が完了し、エラーがなくなり、エクスポートされる変更に問題がないことが確認されると、DirSync のアンインストールと、Azure AD Connect の同期の有効化ができるようになります。 移行を完了するには、次の 2 つの手順を実行します。

### DirSync (古いサーバー) をアンインストールする

-  **プログラムと機能** 検索 **Windows Azure Active Directory 同期ツール**
- アンインストール **Windows Azure Active Directory 同期ツール**
- アンインストールが完了するまで 15 分ほどかかる場合があります。

DirSync がアンインストールされていると、アクティブなサーバーが Azure AD にエクスポートされません。 オンプレミスの Active Directory のすべての変更が引き続き Azure AD に同期されるには、次の手順を完了する必要があります。

### Azure AD Connect (新しいサーバー) を有効化する
インストール後に Azure AD Connect をもう一度開くと、追加の構成変更ができるようになります。 開始 **Azure AD Connect** [スタート] メニューとデスクトップのショートカットからです。 MSI のインストールをもう一度実行しようとしないでください。

次のように表示されます。

![追加のタスク](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AdditionalTasks.png)

- 選択 **ステージング モードの構成**します。
- オフにすると、ステージングをオフにする、 **ステージング モードで有効** チェック ボックスをオンします。

![Azure ADの資格情報を入力します。](./media/active-directory-aadconnect-dirsync-upgrade-get-started/configurestaging.png)

- クリックして、 **次** ボタン
- [確認] ページで、をクリックして、 **インストール** ] ボタンをクリックします。

これで、Azure AD Connect がアクティブなサーバーになりました。

## 次のステップ
Azure AD Connect がインストールされているがあるわかったできます [のインストールを確認し、ライセンスを割り当てる](active-directory-aadconnect-whats-next.md)します。

詳細について [内部設置型 id と Azure Active Directory の統合](active-directory-aadconnect.md)します。

