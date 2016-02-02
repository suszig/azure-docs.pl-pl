<properties 
    pageTitle="Azure ポータルを使用して DocumentDB アカウントを管理する | Microsoft Azure" 
    description="Azure ポータルを使用して DocumentDB アカウントを管理する方法について説明します。Azure ポータルを使用してアカウントを表示、コピー、削除、およびアカウントにアクセスする方法について説明したガイドを紹介します。" 
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

## <a id="keys"></a>表示、コピー、およびアクセス キーを再生成

DocumentDB アカウントを作成する場合は 2 つのサービスが生成されますマスター アクセス キー 
認証に使用できるときに、documentdb に関する
アカウントにアクセスします。 2 つのアクセス キー、DocumentDB によって提供
DocumentDB を中断することなくがキーを再生成します。
宣言します。

[Microsoft Azure ポータル](https://portal.azure.com/),、
アクセス、 **キー** から、 **DocumentDB アカウント** ブレードを表示するには
コピー、および使用するアクセス キーを再生成にアクセスする、
DocumentDB アカウント。

![Azure ポータルのスクリーン ショット、](media/documentdb-manage-account/keys.png)

### Azure ポータルでアクセス キーを表示およびコピーする

1.      [Azure ポータル](https://portal.azure.com/), 、DocumentDB アカウントにアクセスします。

2.      **[概要]** レンズで、**[キー]** をクリックします。

3.      **キー** ブレードで、をクリックして、 **コピー** ボタンの右側に、
コピー対象のキー。

  ![Azure ポータルの ](./media/documentdb-manage-account/image004.jpg)

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

1.      アクセス キーを参照する、アプリケーション コードで更新します
DocumentDB アカウントのセカンダリ アクセス キーです。

2.      DocumentDB アカウントのプライマリ アクセス キーを再生成します。
[Azure ポータル](https://portal.azure.com/),、
DocumentDB アカウントにアクセスします。

3.      [概要] レンズで、**[キー]** をクリックします。

4.      **キー** ブレードで、をクリックして、 **プライマリの再生成** コマンドは、その後
クリックして **Ok** を新しいキーを生成することを確認します。

5.      新しいキーが使用できることを確認したら
(約 5 分後の再生成)、[アクセス キーを更新します。
新しいプライマリ アクセス キーを参照するアプリケーション コードです。

6.      セカンダリ アクセス キーを再生成します。

* 新しく生成されたキーができるまでに数分かかることに注意してください。
DocumentDB アカウントのアクセスに使用します。

## <a id="consistency"></a>DocumentDB 整合性の設定を管理します。

DocumentDB は、次の 4 つの明確に定義されたユーザーが構成可能なデータの一貫性をサポートしています。
開発者は、予測可能なを許可するようにレベル
整合性、可用性、待機時間のトレードオフです。

- **強力な** 常に読み取り操作の一貫性の保証
最後に書き込まれた値を返します。

- **Bounded Staleness** は読み取ることは保証されます
いない古くなりすぎています。 具体的には、読み取ることを保証なし
複数の *K* 最後に書き込まれたバージョンより前のバージョン。

- **セッション** モノトニックな読み取り (することは保証されます
古いデータを読み取る新しい、古いもう一度)、モノトニックな書き込み (書き込みは、
ordered)、および任意の 1 つの最新の書き込みを読み取る
クライアントの観点からです。

- **Eventual** 読み取り操作の一貫性の保証
常に有効な書き込みの一部を読みはやがて収束します。

* 既定では、DocumentDB アカウントはセッションとプロビジョニングに注意してください。
一貫性のレベル。 DocumentDB 整合性の詳細については
設定は、[整合性を参照してください。
Level] (http://go.microsoft.com/fwlink/p/?LinkId=402365) section.*

### DocumentDB アカウントの既定の整合性を指定するには

1.      [Azure ポータル](https://portal.azure.com/), 、DocumentDB アカウントにアクセスします。

2.      **[構成]** レンズで、**[既定の整合性]** をクリックします。

3.      **既定の一貫性** ブレードで、既定の一貫性を選択
DocumentDB アカウントに設定レベル。

![既定の整合性セッション](./media/documentdb-manage-account/image005.png)

![制限付きの既定の整合性](./media/documentdb-manage-account/image006.png)

4.      **[保存]** をクリックします。

5.      Azure ポータルの通知ハブで、操作の進行状況を監視できます。

* 既定値に変更するまでに数分をかかるできることに注意してください。
DocumentDB アカウント間での整合性の設定は有効

## <a id="delete"></a> 方法: Azure ポータルで DocumentDB アカウントを削除します。

不要になったを使用して、Azure ポータルから、DocumentDB アカウントを削除するには、使用、
**削除** コマンドを **DocumentDB アカウント** ブレードです。

![Azure ポータルで DocumentDB アカウントを削除する方法](./media/documentdb-manage-account/image009.png)

1.      [Azure ポータル](https://portal.azure.com/), 、した、DocumentDB のアクセス アカウント
削除しようとしてください。

2.      **[DocumentDB アカウント]** ブレードで、**[削除]** コマンドをクリックします。

3.      [結果の [確認] ブレードには、DocumentDB アカウントを入力します。
アカウントを削除することを確認する名前。

4.      クリックして、 **削除** [確認] ブレード] ボタンをクリックします。

## <a id="next"></a>次のステップ

については、DocumentDB の使用
    アカウント] (http://go.microsoft.com/fwlink/p/?LinkId=402364)。

DocumentDB の詳細については、Azure DocumentDB を参照してください。
    ドキュメント
    [azure.com](http://go.microsoft.com/fwlink/?LinkID=402319&clcid=0x409).








