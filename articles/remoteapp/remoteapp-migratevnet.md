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

うれしいことに、 RemoteApp 固有の Vnet を作成せずに、ハイブリッド RemoteApp コレクションを直接既存の Azure 仮想ネットワーク (Vnet) にデプロイすることが可能になりました。 これにより、VNET の最新機能 (ExpressRoute など) を活用し、VNET にデプロイされている他の Azure サービスおよび仮想マシンにハイブリッド コレクションが直接ネットワーク アクセスできます。  (パフォーマンスが向上し、VNET-to-VNET 構成よりもセットアップが容易になります。)


という名前のハイブリッド RemoteApp コレクションを既に作成したとしましょう *OriginalCollection* RemoteApp VNET を持つと呼ばれる *RemoteAppVNET*します。 ここではという名前の新しい Azure VNET に移行する手順 *AzureVNET*します。

1.   **ネットワーク** ] タブで、 [管理ポータル](http://manage.windowsazure.com/), 、という名前の VNET を作成 *AzureVNET*, と同じ場所、DNS 構成を使用して、およびアドレス空間 (の少なくとも 1 つの *AzureVNET* サブネット) を使用するよう *RemoteAppVNET*します。
2.  構成 *AzureVNET* か、ホスト、または Active Directory の展開へのネットワーク接続を *OriginalCollection* ドメインに参加します。
3.   **Remoteapp** という新しい RemoteApp コレクションを作成] タブで、 *新しいコレクション*します。 (使用する、 **VNET で作成** オプションいない **簡易作成**.)
3.  構成 *NewCollection* 内のサブネットにデプロイされる *AzureVNET*します。
4.  構成 *NewCollection* を使用したのと同じイメージとドメイン参加情報を使用する *OriginalCollection*します。
5.  数時間後に *NewCollection* はアクティブな状態にコレクションの一覧に表示されます。

元のコレクションのユーザー情報を新しいコレクションに全く移行する必要がない場合は、次の手順を実行します。

6.  削除 *OriginalCollection*します。
7.  削除 *RemoteAppVNET*します。

これで完了です。

一方、元のコレクションのユーザー情報を新しいコレクションに移行する必要がある場合は、次の手順を実行します。

6.  電子メールを送信する [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20user%20information%20migration) 、Azure サブスクリプション ID、元のコレクションの名前と、新しいコレクションの名前を使用し、ユーザー情報を移行するように依頼します。
7.  RemoteApp チームにより、2 営業日以内に、ユーザー アクセス リスト、すべてのユーザー ドキュメント、およびユーザー設定が元のコレクションから新しいコレクションに移されます。
8.  削除 *OriginalCollection*します。
9.  削除 *RemoteAppVNET*します。

これで完了です。

ご質問や、特別なサポートが必要な場合は、電子メール [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20VNET%20migration%20help)します。

