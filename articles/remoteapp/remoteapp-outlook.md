<properties
    pageTitle="Azure RemoteApp で Outlook を使用する | Microsoft Azure" 
    description="Azure RemoteApp で Outlook を構成し、使用する方法について説明する | Microsoft Azure"
    services="remoteapp"
    documentationCenter=""
    authors="pavithir"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/04/2015"
    ms.author="elizapo" />


# Azure RemoteApp で Microsoft Outlook を使用する

Azure RemoteApp は Microsoft Outlook O365 をサポートします。 方法の詳細を参照 [Azure RemoteApp で Office が動作](remoteapp-officesubscription.md)します。 Azure RemoteApp で使用するとき、Outlook にはいくつかの推奨設定があります。

## キャッシュ モード

キャッシュ モードは、Azure RemoteApp で Outlook を使用するときの推奨構成です。 Exchange キャッシュ モードを使用するように Outlook 2013 を構成すると、オフライン アドレス帳 (OAB) と共に、ユーザーのコンピューターのオフライン データ ファイルに保存されているユーザーの Microsoft Exchange メールボックスのローカル コピーから Outlook 2013 は機能します。 キャッシュ メールボックスと OAB は O365 サービスから定期的に更新されます。 詳細について [キャッシュされ、オンライン モードの相違点](https://technet.microsoft.com/library/jj683103.aspx)します。

ユーザーは、アカウントの設定または変更時に **Exchange キャッシュ モード**か**オンライン モード**を選択できます。 Office カスタマイズ ツール (OCT) またはグループ ポリシーを利用し、いずれかのモードをデプロイすることもできます。

読み取り [キャッシュ モードが有効にする手順のステップ バイ ステップ](https://technet.microsoft.com/library/c6f4cad9-c918-420e-bab3-8b49e1885034#proc)します。

## 検索

Azure RemoteApp では、Outlook 内での検索利用に制限があります。 Azure RemoteApp はプールされた VM を利用し、ユーザー セッションに対応します。 検索インデックス作成はマシン ID に基づきます。このマシン ID は VM ごとに異なります。 Azure RemoteApp にログインするたびに、ユーザーを新しい VM に向けることができます。 つまり、ローカル検索を有効にすると、マシン ID が変わるたびに (ユーザーが別の VM にいるとき) インデクサーが実行されます。 .OST ファイルのサイズによっては、インデクサーが完了し、他のアプリに必要なリソースを使い果たすまで時間がかかることがあります。 検索が遅くなるだけでなく、結果が得られないこともあります。 これを回避する方法は、既定でオンライン検索を有効にすることです。 残念ながら、Outlook 2013 では、インデックス付きまたはローカルの検索は無効にできず、オンライン検索は既定で有効にできません。

Outlook 2016 にはこの問題に対処するソリューションが与えられます。Exchange 2016 にホストする (または Office 365 にホストする) メールボックスで新しいオンライン検索が利用できます。 それには、ローカル キャッシュに対してサーバーの検索結果が使用されます (OST)。 一部のシナリオでは Outlook が検索インデクサーの使用にフォールバックしますが、ほとんどの検索ではオンラインモードが使用されます。 Azure RemoteApp では、メール検索が非常に重要なシナリオの場合、Outlook 2016 を使用することを推奨しています。





