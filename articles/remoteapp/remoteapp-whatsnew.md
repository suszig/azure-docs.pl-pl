
<properties
    pageTitle="Azure RemoteApp の新機能。 | Microsoft Azure"
    description="Azure RemoteApp の変更点と改善点について説明します。"
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
    ms.date="10/23/2015"
    ms.author="elizapo" />



# Azure RemoteApp の新機能。

Azure RemoteApp の利点の 1 つは、常に改善の努力がされていることです。 改善を行うたびに、どのような変更を加えたかをここでお知らせします。

## 2015 年 9 月
- Microsoft Office 365 テンプレートとギャラリー イメージの InfoPath を追加しました。 InfoPath を共有する場合、コレクションを最新のイメージで更新します。
- クライアント更新:
    - Windows クライアントが更新され、フィードバック (特に、接続問題に関する) を共有できるようになりました。
    - iOS クライアントが更新され、エラー メッセージングが修正され、資格情報の有効期限が予想より早く切れる問題が修正されました。
- 現在、Office 2016 サポートのテストに取り組んでいます。 完了したら、更新された画像を探してください。
- 新しいアーティクルをパブリッシュ、 [クラウドとハイブリッド コレクションの相違点](remoteapp-collections.md) -これアプリ - クラウドのみ、クラウド + VNET、またはハイブリッドにとって最適なコレクションの種類を選択することができます。
- Azure RemoteApp で QuickBooks を共有したいのに手順がわかりませんか。 チェック アウト [Eric の新しい記事](remoteapp-quickbooks.md) を行うにはどのように指示します。

## 2015 年 8 月
8 月に大きな変化がありました。ここに概要を記載します。

- クラウド コレクションで Azure VNET を使えるようになりました。 チェック アウト、 [クラウドの作成手順](remoteapp-create-cloud-deployment.md) 手順については、新しいします。
- Windows RemoteApp クライアントの [スタート] メニューをアプリに追加できるようになりました。 アプリがアプリケーションの一覧に表示されるので、それを Windows の [スタート] メニューに固定できます。
- Azure VM ギャラリーに新しいイメージが追加されました。「Windows Server Remote Desktop Session Host with Microsoft Office 365 ProPlus」です。
- Mac クライアントが修正され、モーダル ウィンドウのアプリのフリーズを解消しました。
- 使用する方法を文書化、 [Office 365 ProPlus サブスクリプション](remoteapp-officesubscription.md) Azure RemoteApp を使用します。
- 方法の詳細 [アプリとデータをセキュリティで保護された](remoteapp-secure.md) Azure RemoteApp コレクションにします。

## 2015 年 7 月

7 月は 8 月に予定されている変更の準備をしているため、現時点でお知らせはほとんどなく、大半はドキュメントの更新です。 最新の変更内容を次に示します。

- 追加、 **サポート** 、フォーラムなどのサポート リソースをより簡単にアクセスできるように、ポータルにタブをクリックします。
- ハイブリッド コレクションの作成に関するトラブルシューティング情報が修正されました。 チェック アウト [最新で最高](remoteapp-hybridtrouble.md) トラブルシューティングのヒントについて、VNET を構成する正しいポートを特定する方法などです。
- どのように記載されている [ユーザー データ](remoteapp-upd.md) を作成し、Azure RemoteApp に保存します。
- 文書化する方法を [アプリケーション ロックダウン](remoteapp-secure.md)します。
- 発行された、 [Azure RemoteApp のコマンドレット](https://msdn.microsoft.com/library/mt428031.aspx)します。
- 最後に、Microsoft は、一部の Azure RemoteApp ユーザーと用語についての話し合いを開始しました。 さまざまなコレクションのオプションの表現方法を変更することを検討しています。

## 2015 年 6 月

多くの機能が変更されました。 6 月、チームは非常に忙しい日々を過ごしました。

- Azure RemoteApp を再設計 [ランディング ページ](https://www.remoteapp.windowsazure.com/) のチェック アウトします。
- サブスクリプションの一部として利用できるすべてのイメージでソフトウェアを更新しました。
- ハイブリッド コレクションを改善しました。強制トンネリング対応となり、コレクションの作成前に IP サブネット サイズを確認します。
- Web カメラの場合、* ワイルドカードが適切に機能しないことが判明しました。 代わりに、インスタンス ID または GUID を指定する必要があります。 リダイレクト情報を更新し、これを反映する予定です。
- それにより、Azure ギャラリーからテンプレート イメージを作成するとき、イメージにカスタムのウイルス対策ソフトウェアを追加できます。

7 月にさらに多くの変更点をロールアウトします。近いうちに新しい更新で戻ってきます。

## 2015 年 5 月

最初にこのトピックを作ってから長い時間が経過し、さまざまな機能が追加されました。そのため、このリストは少し異なり、3 月の初めから 5 月の終わりまでを対象にしています。 新しい機能:

- すべてを自動化 - Azure RemoteApp になりました [Azure PowerShell モジュールのコマンドレット](remoteapp-tutorial-arawithpowershell.md)します。
- [Azure の仮想マシンから Azure RemoteApp イメージの作成](remoteapp-image-on-azurevm.md)します。 カスタム イメージを Azure にアップロードする作業が大幅に速くなりました。
- RemoteApp VNET ではなく Azure VNET を使用し、企業ネットワーク リソースを Azure に接続します。 更新しました、 [ハイブリッド コレクションの指示](remoteapp-create-hybrid-deployment.md) にする (ステップ 1 が) Azure VNET の作成について説明します。
- Vnet といえば、チェック アウト [新しいガイダンス](remoteapp-vnetsizing.md) VNET の周囲のサイズ制限および制約事項です。
- 同様の制限 - に関して、 [サービスの制限と既定の設定](remoteapp-servicelimits.md)でしょうか。

Azure RemoteApp について詳細を知りたいですか。 数週間前、RemoteApp チームは大勢で Ignite に出動しました。 Eric の動画、 [Microsoft Azure RemoteApp の管理の基礎と管理](http://channel9.msdn.com/Events/Ignite/2015/BRK3868)します。

Azure RemoteApp が実際に使われている様子を確認したいですか。 チェック アウト、 [あらゆるデバイスを任意の場所で任意のアプリを実行](remoteapp-anyapp.md) チュートリアル - ことでは、データベース ファイルを共有することも、ユーザーにアクセスを共有する方法です。 また、チュートリアルにある [Office 365](remoteapp-tutorial-o365anywhere.md) 同じ任意のデバイス上で実行します。

お読みいただいてありがとうございました。翌月はさらに多くの更新を届けます。


### サポートのサポートのお願い
記事を評価したり、下にコメントを投稿したりするだけでなく、記事自体を変更できることを知っていましたか。 説明不足ですか。 間違いがありますか。 わかりにくいことが書いてありますか。 上にスクロールしてクリックして **GitHub の編集** の変更を加えるについては、私たちに来るし、次に、筆に一度表示されます、変更および強化ここです。

