<properties
    pageTitle ="Azure RemoteApp のアプリやリソースをセキュリティで保護された |Microsoft Azure]
    description =「Azure RemoteApp のアプリやリソースをロックダウンする方法について説明します」
    サービス =「remoteapp」
    documentationCenter=""
    authors ="lizap"
    manager ="mbaldwin"/>

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/05/2015"
    ms.author="elizapo"/>



# Azure RemoteApp でのアプリとリソースの保護

Azure RemoteApp には、一元管理された Windows アプリにユーザーがアクセスするための機能が用意されており、ユーザーが実行できることと、実行できないことを制御できます。 これは、管理されていないデバイス (個人の Macbook など) からユーザーが接続しているときに、そのユーザーのアクセスまたは操作を制御する必要がある場合に特に便利です。

たとえば、ユーザー認証に Active Directory を使用している場合、ユーザーがアプリからデータをコピーできないようにするには、ユーザーによるデータ コピーをブロックするようにリモート デスクトップのグループ ポリシーを構成します。

また、コレクション内の特定のアプリに対して、インターネット アクセスをブロックする必要があるときにも使用できます。 この場合は、コレクションのイメージを作成するときに、アクセスをブロックする Windows ファイアウォール ルールを作成します。

## 実装オプション

  重要な実装オプションを次に示します。これらのオプションは、必要に応じて個別に、または組み合わせて使用できます。

1.  RemoteApp コレクションがドメインに参加している場合は、いずれかを実施できる [グループ ポリシー](https://technet.microsoft.com/library/cc725828.aspx) (アイドル状態と切断のタイムアウト ポリシーの説明を除く [ここ](../azure-subscription-service-limits.md))します。
2.  構成することができます (、コレクションが参加しているドメインではないか、AD での適切な権限がない) 場合、グループ ポリシーの代わりに、 [ローカル ポリシー](https://technet.microsoft.com/library/cc775702.aspx) テンプレート イメージにします。 2 つのポリシーが競合している場合は、グループ ポリシーがローカル ポリシーに優先することに注意してください。
3.  OS/アプリ設定の一部はありませんが、ポリシーを使用して構成可能なレジストリ キーを使用して指定できます、 [RegEdit ツール](./remoteapp-hybridtrouble.md) テンプレート イメージを構成するときにします。
4.  使用する [Windows ファイアウォール](http://windows.microsoft.com/en-US/windows-8/Windows-Firewall-from-start-to-finish) 、アプリが実行されているコンピューターとの間のネットワーク アクセスを制御します。 ここで定義されている URL とポートをブロックしないようにするだけです。
5.  使用する [AppLocker](https://technet.microsoft.com/library/hh831440.aspx) コントロールがどのアプリケーションとファイルをユーザーが実行できます。 たとえば、あなたが発行していないアプリケーションでも、あなたがコレクションを作成するときに使用したイメージでそのアプリケーションを使用できれば、経験豊富なユーザーは、そのアプリケーションの実行方法を見つけ出すことができます。AppLocker はこれをブロックできます。

## 詳細情報

- 次の RDS ポリシーは、多くの場合、非常に役に立ちます。
    - [デバイスとリソースのリダイレクト](https://technet.microsoft.com/library/ee791794.aspx)
    - [プリンターのリダイレクト](https://technet.microsoft.com/library/ee791784.aspx)
    - [Profiles](https://technet.microsoft.com/library/ee791865.aspx).
- 構成 RemoteApp PowerShell モジュールによるリダイレクト (よう [ここ](./remoteapp-redirection.md)) セキュリティが主な目的である場合は、テンプレート イメージのローカル ポリシーを使用して、またはグループ ポリシーを使用してポリシーを適用するおくために、ポリシーを適用するクライアント コンピューターに依存しています。
- [Windows Server 2012 R2 ポリシー](https://technet.microsoft.com/library/hh831791.aspx)します。
- [Office 2013 のポリシー](https://technet.microsoft.com/library/cc178969.aspx) (を含む [Office ツールバーをカスタマイズする方法について](https://technet.microsoft.com/library/cc179143.aspx))します。





