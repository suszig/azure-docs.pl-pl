<properties
    pageTitle="Azure RemoteApp テンプレート イメージの内容 | Microsoft Azure"
    description="Azure RemoteApp に含まれるテンプレート イメージについて説明します。"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/02/2015"
    ms.author="elizapo" />

# Azure RemoteApp テンプレート イメージの内容

Azure RemoteApp サブスクリプションには、次の 3 つのテンプレート イメージが含まれています:


- Windows Server 2012
- Microsoft Office 365 ProPlus (Office 365 サブスクリプションが必要)
- Microsoft Office 2013 Professional Plus (評価版のみ)

> [AZURE.IMPORTANT]Azure RemoteApp サブスクリプションでは、Office 365 ProPlus、個別のサブスクリプションを必要として実稼働環境で使用できない Office 2013 の場合を除いて、イメージにソフトウェアへのアクセスを許可します。 つまり、テンプレート イメージ上のプログラムまたはアプリをユーザーと共有できます。 たとえば、Windows Server 2012 R2 のイメージを使用するコレクションを作成する場合、ユーザーが RemoteApp でアクセスできる System Center Endpoint Protection を発行できます。
>
> チェック アウト、 [RemoteApp のライセンスの詳細](remoteapp-licensing.md) の詳細。  [Azure RemoteApp で Office を使用して](remoteapp-o365.md) for Office のライセンス情報です。

各イメージが含んでいるものについては、詳細を参照してください。

## Windows Server 2012 R2 (「バニラ イメージ」)
このイメージは Microsoft Windows Server 2012 R2 Datacenter オペレーティング システムに基づいており、Azure RemoteApp テンプレート イメージの要件を満たすために、次のようなロールと機能がインストールされています。


- .NET Framework 4.5、3.5.1、3.5
- デスクトップ エクスペリエンス
- インクと手書きサービス
- メディア ファンデーション
- リモート デスクトップ セッション ホスト
- Windows PowerShell 4.0
- Windows PowerShell ISE
- WoW64 サポート

このイメージには次のようなアプリケーションがインストールされています。

- Adobe Flash Player
- Microsoft Silverlight
- Microsoft System Center 2012 Endpoint Protection
- Microsoft Windows Media Player


## Microsoft Office 365 ProPlus (サブスクリプションが必要)
Office 365 は最もリクエストの多いアプリケーションなので、連携する "カスタム" イメージを作成しました。

このイメージは、バニラ イメージの拡張機能であり、Windows Server 2012 R2 のイメージで説明するコンポーネントに加え、次の Microsoft Office 365 ProPlus を含んでいます。


- Access
- Excel
- Lync
- OneNote
- OneDrive for Business
- Outlook
- PowerPoint
- Word
- Microsoft Office Proofing Tools

また、イメージにも Visio Pro と Project Pro が含まれています。

次のアプリケーションも同様にインストールされます。

- SQL Native Client
- ODBC ドライバー
- SQL Server データ マイニング クライアント
- MasterDataServices クライアント
- Microsoft Publisher
- Power Query
- Power Map


Office 365 ProPlus のアプリのすべての機能は、Office 365 ProPlus のプランを持つユーザーのみが使用できます。 詳細については、Office 365 サブスクリプション プランを参照してください [Office 365 プラン](http://technet.microsoft.com/library/office-365-plan-options.aspx)します。 まだ不明な点がある場合は、 チェック アウト、 [Office 365 と RemoteApp](remoteapp-o365.md) 情報。 新しい記事を確認しても [Azure RemoteApp で Office 365 サブスクリプションを使用する方法](remoteapp-officesubscription.md)します。

Office 365 ProPlus、Visio Pro、Project Pro のライセンスは個別に取得する必要があります (それぞれ独自のライセンスを持ちます)。

## Microsoft Office 2013 Professional Plus (評価版のみ)
無料試用期間中には、Office 2013 のイメージを使用してサービスをテストすることができます。

このイメージは、バニラ イメージの拡張機能であり、Windows Server 2012 R2 のイメージで説明するコンポーネントに加え、インストールされている次の Microsoft Office 2013 Professional Plus のコンポーネントを含んでいます。


- Access
- Excel
- Lync
- OneNote
- OneDrive for Business
- Outlook
- PowerPoint
- Project
- Visio
- Word
- Microsoft Office Proofing Tools

> [AZURE.IMPORTANT]**法的情報:** このイメージには、Microsoft Office のライセンスが含まれていないと *実稼働環境には使用できません*します。 Office 2013 Professional Plus イメージは評価版を使用する目的でのみ提供されます。 運用環境で Azure RemoteApp の Office アプリケーションを使用する場合は、Office 365 ProPlus のイメージを使用する必要があります。 Office のライセンスの詳細については、次を参照してください [Azure RemoteApp で Office 365 を使用する。](remoteapp-o365.md)

