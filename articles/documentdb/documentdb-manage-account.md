<properties 
    pageTitle="Azure ポータルを使用して DocumentDB アカウントを管理する | Microsoft Azure" 
    description="Azure ポータルを使用して DocumentDB アカウントを管理する方法について説明します。 Azure ポータルを使用してアカウントを表示、コピー、削除、およびアカウントにアクセスする方法について説明したガイドを紹介します。" 
    keywords="Azure Portal, documentdb, azure, Microsoft azure"
    services="documentdb" 
    documentationCenter="" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/18/2015" 
    ms.author="anhoh"/>

# DocumentDB アカウントの管理方法

Azure ポータルで、キーおよび整合性の設定を操作する方法と、アカウントを削除する方法について説明します。

## <a id="keys"></a>アクセス キーを表示、コピー、および再生成する
DocumentDB アカウントを作成する場合は 2 つのサービスが生成されますマスター アクセス キー 
認証に使用できるときに、documentdb に関する
アカウントにアクセスします。 2 つのアクセス キー、DocumentDB によって提供
DocumentDB を中断することなくがキーを再生成します。
宣言します。

 [Microsoft Azure ポータル](https://portal.azure.com/),、
アクセス、 **キー** から、 **DocumentDB アカウント** ブレードを表示するには
コピー、および使用するアクセス キーを再生成にアクセスする、
DocumentDB アカウント。

![Azure ポータルのスクリーン ショット、[キー] ブレード](media/documentdb-manage-account/keys.png)

### Azure ポータルでアクセス キーを表示およびコピーする

1.      In the [Azure Portal](https://portal.azure.com/), access your DocumentDB account. 

2.      In the **Summary** lens, click **Keys**.

3.      On the **Keys** blade, click the **Copy** button to the right of the
コピー対象のキー。

  ![Azure ポータルの [キー] ブレードでアクセス キーを表示およびコピーする](./media/documentdb-manage-account/image004.jpg)

### アクセス キーを再生成する

DocumentDB アカウントへのアクセス キーを変更する必要があります。
定期的に更新より安全な接続のために役立ちます。 2 つのアクセス キー
DocumentDB への接続を維持するために割り当てられました。
その他のアクセス キーを再生成する 1 つのアクセス キーを使用するアカウント。

> [AZURE.WARNING] アプリケーションをすべてに影響を与えるアクセス キーを再生成
現在のキーに依存します。 すべてのクライアントへのアクセス キーを使用します。
DocumentDB アカウントにアクセスするは、新しいキーを使用して更新する必要があります。

アプリケーションまたはクラウド サービスの DocumentDB アカウントを使用している場合
接続失われますが、キーを再生成する場合を切り替えない限り、
キー。 次の手順は、キーの切り替えに含まれるプロセスの概要を示します。

1.      Update the access key in your application code to reference the
DocumentDB アカウントのセカンダリ アクセス キーです。

2.      Regenerate the primary access key for your DocumentDB account.
 [Azure ポータル](https://portal.azure.com/),、
DocumentDB アカウントにアクセスします。

3.      In the Summary lens, click **Keys**.

4.      On the **Keys** blade, click the **Regenerate Primary** command, then
クリックして **Ok** を新しいキーを生成することを確認します。

5.      Once you have verified that the new key is available for use
(約 5 分後の再生成)、[アクセス キーを更新します。
新しいプライマリ アクセス キーを参照するアプリケーション コードです。

6.      Regenerate the secondary access key.

*新しく生成されたキーができるまでに数分かかることができます。
DocumentDB アカウントにアクセスするために使用します。*

## <a id="consistency"></a>DocumentDB 整合性の設定を管理する
DocumentDB は、次の 4 つの明確に定義されたユーザーが構成可能なデータの一貫性をサポートしています。
開発者は、予測可能なを許可するようにレベル
整合性、可用性、待機時間のトレードオフです。

- **厳密な** 常に読み取り操作の一貫性の保証
最後に書き込まれた値を返します。

- **Bounded Staleness** は読み取ることは保証されます
いない古くなりすぎています。 具体的には、読み取ることを保証なし
複数の *K* 最後に書き込まれたバージョンより前のバージョン。 

- **セッション** モノトニックな読み取り (することは保証されます
古いデータを読み取る新しい、古いもう一度)、モノトニックな書き込み (書き込みは、
ordered)、および任意の 1 つの最新の書き込みを読み取る
クライアントの観点からです。

- **最終的な** 読み取り操作の一貫性の保証
常に有効な書き込みの一部を読みはやがて収束します。

*既定では、DocumentDB アカウントはセッションにプロビジョニングされます。
一貫性のレベル。  DocumentDB 整合性の詳細については
設定を参照してください、 [整合性
レベル](http://go.microsoft.com/fwlink/p/?LinkId=402365) セクションです。*

### DocumentDB アカウントの既定の整合性を指定するには

1.      In the [Azure Portal](https://portal.azure.com/), access your DocumentDB account. 

2.      In the **Configuration** lens, click **Default Consistency**.

3.      On the **Default Consistency** blade, select the default consistency
DocumentDB アカウントに設定レベル。

![既定の整合性セッション](./media/documentdb-manage-account/image005.png)

![制限付きの既定の整合性](./media/documentdb-manage-account/image006.png)

4.      Click **Save**.

5.      The progress of the operation may be monitored via the Azure Portal Notifications hub.

*既定値に変更するまでに数分かかることができます。
一貫性の設定は、DocumentDB アカウントで有効です。*

## <a id="delete"></a> 方法: Azure ポータルで DocumentDB アカウントを削除します。
不要になったを使用して、Azure ポータルから、DocumentDB アカウントを削除するには、使用、
**削除** コマンドを **DocumentDB アカウント** ブレードです。

![Azure ポータルで DocumentDB アカウントを削除する方法](./media/documentdb-manage-account/image009.png)

1.      In the [Azure Portal](https://portal.azure.com/), access the DocumentDB Account you
削除しようとしてください。 

2.      On the **DocumentDB Account** blade, click the **Delete** command.

3.      On the resulting confirmation blade, type the DocumentDB Account
アカウントを削除することを確認する名前。

4.      Click the **Delete** button on the confirmation blade.

## <a id="next"></a>次のステップ

学習方法 [、DocumentDB を使ってみる
   アカウント](http://go.microsoft.com/fwlink/p/?LinkId=402364)します。

DocumentDB の詳細については、Azure DocumentDB を参照してください。
    ドキュメント
    [azure.com](http://go.microsoft.com/fwlink/?LinkID=402319&clcid=0x409)します。

 
 

