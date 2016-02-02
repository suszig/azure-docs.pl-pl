<properties
    pageTitle="RemoteApp VNET から Azure VNET に移行する方法の詳細 | Microsoft Azure"
    description="RemoteApp VNET から Azure VNET に移行する方法の詳細"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2015"
    ms.author="elizapo" />




# RemoteApp VNET から Azure VNET にハイブリッド コレクションを移行する方法

うれしいことに、 RemoteApp 固有の Vnet を作成せずに、ハイブリッド RemoteApp コレクションを直接既存の Azure 仮想ネットワーク (Vnet) にデプロイすることが可能になりました。 これにより、VNET の最新機能 (ExpressRoute など) を活用し、VNET にデプロイされている他の Azure サービスおよび仮想マシンにハイブリッド コレクションが直接ネットワーク アクセスできます。 (パフォーマンスが向上し、VNET-to-VNET 構成よりもセットアップが容易になります。)


*RemoteAppVNET* という名前の RemoteApp VNET を持つ *OriginalCollection* という名前のハイブリッド RemoteApp コレクションを既に作成してあるとしましょう。 このコレクションを *AzureVNET* という名前の新しい Azure VNET に移行するには、次の手順を実行します。

1.  **ネットワーク** ] タブで、 [管理ポータル](http://manage.windowsazure.com/), 、という名前の VNET を作成 *AzureVNET*, と同じ場所、DNS 構成を使用して、およびアドレス空間 (の少なくとも 1 つの *AzureVNET* サブネット) を使用するよう *RemoteAppVNET*します。
2.  *AzureVNET* を構成し、*OriginalCollection* の参加先ドメインになっている Active Directory デプロイメントをホストするか、これにネットワーク接続するようにします。
3.  **[Remoteapp]** タブで、*New Collection* という名前の RemoteApp コレクションを新規に作成します。 (**[VNet で作成]** オプションを使用し、**[簡易作成]** オプションは使用しないでください。)
3.  *NewCollection* が *AzureVNET* のサブネットにデプロイされるように構成します。
4.  *NewCollection* が *OriginalCollection* に使用したのと同じイメージとドメイン参加情報を使用するように構成します。
5.  数時間後にはアクティブな状態の *NewCollection* がコレクション リストに表示されます。

元のコレクションのユーザー情報を新しいコレクションに全く移行する必要がない場合は、次の手順を実行します。

6.  *OriginalCollection* を削除します。
7.  *RemoteAppVNET* を削除します。

これで完了です。

一方、元のコレクションのユーザー情報を新しいコレクションに移行する必要がある場合は、次の手順を実行します。

6.  電子メールを送信する [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20user%20information%20migration) 、Azure サブスクリプション ID、元のコレクションの名前と、新しいコレクションの名前を使用し、ユーザー情報を移行するように依頼します。
7.  RemoteApp チームにより、2 営業日以内に、ユーザー アクセス リスト、すべてのユーザー ドキュメント、およびユーザー設定が元のコレクションから新しいコレクションに移されます。
8.  *OriginalCollection* を削除します。
9.  *RemoteAppVNET* を削除します。

これで完了です。

ご質問や、特別なサポートが必要な場合は、電子メール [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20VNET%20migration%20help)します。





