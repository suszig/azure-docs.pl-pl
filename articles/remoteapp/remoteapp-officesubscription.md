
<properties 
    pageTitle="Azure RemoteApp で Office 365 サブスクリプションを使用する方法 | Microsoft Azure"
    description="Azure RemoteApp で Office 365 サブスクリプションを使用して Office アプリを共有する方法を説明します。"
    services="remoteapp"
    documentationCenter="" 
    authors="piotrci" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/14/2015" 
    ms.author="elizapo" />



# Azure RemoteApp で Office 365 サブスクリプションを使用する方法

既存の Office 365 サブスクリプションを Azure RemoteApp で使用して、Office アプリをクラウドから共有できます。 ここでは Office 365 + Azure RemoteApp のオプションについて説明し、サブスクリプションを活用するのに役立つ Office 365 についての記事へのリンクを示します。

## Azure RemoteApp で Office 365 アカウントを使用する方法
すべての情報、Peter の新しい記事: [Office 365 ユーザー アカウントで Azure RemoteApp を使用する方法](remoteapp-o365user.md)

## Office 365 サブスクリプションを使用して Azure RemoteApp で Office アプリケーションを実行できますか?

はい。 実際、Azure RemoteApp で Office アプリケーションを実行する唯一の方法は、Office 365 サブスクリプションを使用することです。

(注: ホスティング パートナーで、Azure RemoteApp のデプロイメントが配信される場合に基づく Office のライセンスを提供するためにはできる、 [サービス プロバイダーの使用許諾契約書](http://www.microsoft.com/en-us/Licensing/licensing-programs/spla-program.aspx))


Office 365 サブスクリプションがすばらしい点は、Azure Cloud を含む多くのさまざまなプラットフォームおよび環境で同じユーザー ライセンスを使用できることです。 Azure RemoteApp で Office アプリケーションを使用する場合、追加のライセンスを購入したり、特別な方法で既存のライセンスを構成したりする必要はありません。 必要なは、Office 365 サブスクリプションを含む [Office 365 ProPlus](https://technet.microsoft.com/library/Gg702619.aspx)します。

Office 365 ProPlus により [共有コンピューターのライセンス認証](https://technet.microsoft.com/library/Dn782860.aspx) - この機能により、一時的なユーザーによるライセンス認証を Office 仮想にし、クラウド環境など、Azure RemoteApp (リモート デスクトップ サービス)。

Office 365 ProPlus を含む Office 365 プランについては、 チェック アウト、 [各プラン内での可用性をサービス](https://technet.microsoft.com/library/office-365-plan-options.aspx) テーブルです。 Office 365 ProPlus を含まないプランがあることに注意してください (たとえば、Office 365 Business プラン)。 お使いのプランに Office 365 ProPlus が含まれない場合は、含むプラン (Office 365 Education E3 など) へのアップグレードを検討してください。

## Azure RemoteApp で Office 365 ProPlus ライセンスを使用するにはどうすればよいですか?

Office 365 ProPlus の各ユーザー ライセンスでは、1 人のユーザーが最大 5 台のコンピューターとタブレットおよび携帯電話で Office アプリケーションをアクティブ化できます。 デバイスで Office を非アクティブ化するまで、各アクティブ化はユーザーに登録されています (ユーザーがそのデバイスを管理できる、 [Office 365 ポータル](https://portal.office365.com/).)

Azure RemoteApp では、1 人のユーザーが、自覚なしで同じ日に複数台のコンピューターにログインできます。 これは、サービスがクラウドのリソースを自動的に管理してスケーリングするためであり、ユーザーは共有するアプリとプログラムだけを認識します。 このシナリオでは、Office 365 ProPlus は、コンピューターのアクティブ化モードの共有を提供します。つまり、ユーザーはリソースにアクセスするためにライセンス管理を行う必要がなく、個々のコンピューターは 5 台というアクティブ化制限の対象としてカウントされることがなくなります。

管理者が Office 365 ProPlus ライセンスをユーザーに割り当てると、ユーザーは個人のデバイスで、および Azure RemoteApp コレクションから、Office を使用できます。

## Office 365 と Azure RemoteApp ではどの Office アプリケーションを使用できますか?

Office 365 ProPlus サブスクリプションを使用すると、Office 2013 および Office 2016 (リリース後) を共有できます。 Azure RemoteApp は、これより前のバージョンの Office をサポートしていません。

## Visio Pro または Project Pro はどうですか?

RemoteApp サブスクリプションに含まれる Office 365 ProPlus のイメージには、Visio Pro と Project Pro の両方が含まれます。 ただし、Office 365 ProPlus サブスクリプションを使用してこれらのプログラムをアクティブ化することはできません。それぞれに専用のライセンスがあります。 コンポーネントを起動する、 [Office 365 ポータル](https://portal.office365.com/)します。 

これらのプログラムを使用しない場合は、ライセンスを取得する必要はありません。 使用するプログラムだけをアクティブ化し、他はスキップしてください。 イメージにはまだ表示されますが、それらを使用することはできません。 

## Office 365 と Azure RemoteApp を使い始めるにはどうすればよいですか?

Office 365 ライセンスの詳細がわかったら、Azure RemoteApp で使ってみてください。とても簡単です。

Azure RemoteApp コレクションを作成するときに使用して、 **Office 365 ProPlus (サブスクリプションが必要)** イメージ。

![Azure RemoteApp イメージと Office 365 Pro Plus](./media/remoteapp-officesubscription/remoteapp-officeimage.png)


このイメージには、最新バージョンの Windows Server と Office 365 ProPlus が含まれます。 コレクション (発行アプリを含む) を構成した後、ユーザーは (RemoteApp クライアントを使用して) Azure RemoteApp にログインし、Office アプリに Office 365 資格情報を提供するだけです。 ライセンスは自動的に提供され、どのようなセットアップも管理も必要ありません。

## Office 365 ProPlus でカスタム イメージを作成できますか?

Office 365 ProPlus を含むコレクションのカスタム イメージを作成できます。 2 つの選択肢があり、提供される Azure ギャラリー イメージを使用するか、または独自のカスタム イメージを作成してそこに Office 365 ProPlus をインストールします。

### Azure ギャラリー イメージを使用する

コレクションに Office 365 ProPlus を展開する最も簡単には [Azure ギャラリー イメージのいずれかで開始](remoteapp-image-on-azurevm.md) Azure RemoteApp サブスクリプションに含まれています。 選択するかどうかを確認、 **Windows Server リモート デスクトップ セッション ホストに Office 365 ProPlus のプレインストールされている** イメージ。 次に、そのイメージに必要な他のアプリをインストールすれば、準備完了です。

### カスタム イメージを使用する

カスタム イメージをいつでも作成できます: 作成することができます、 [Azure VM](remoteapp-image-on-azurevm.md) または [イメージをローカルで作成](remoteapp-create-custom-image.md) を Azure にアップロードします。 どちらの場合も、共有コンピューター アクティブ化ノードを使用して Office 365 ProPlus をインストールしてください。 使用して、 [Office 展開ツール](http://blogs.technet.com/b/odsupport/archive/2014/07/11/using-the-office-deployment-tool.aspx) に従って、 [指示](https://technet.microsoft.com/library/Dn782858.aspx) のインストール。  

### カスタム イメージで Office 365 ProPlus の自動更新を無効にする - 重要

カスタム イメージは、ユーザーの要求が増えたときに新しいリソースを追加するためテンプレートとして、Azure RemoteApp によって使用されます。 遅延や接続の問題を回避するため、イメージでは Office の自動更新を無効にしてください。 無効にしないと、そのテンプレートで作成されたすべてのリソースが、開始時に自動的に更新されます。 代わりに、カスタム イメージの更新には標準の Azure RemoteApp プロセスを使用します。 テンプレート イメージで Office アプリケーションを 1 回更新した後、Azure RemoteApp でユーザーを更新します。

自動更新を無効にするには、Office 展開ツールの構成ファイルに以下を追加します。

        <Updates Enabled="FALSE" />

構成ファイルには以下の行が含まれるようになります。
    
        <Display Level="NONE" AcceptEULA="TRUE" />
        <Property Name="SharedComputerLicensing" Value="1" />
        <Updates Enabled="FALSE" />

## Office 365 ProPlus でイメージを更新するにどうすればよいですか?

コレクション内のイメージを更新する多くの理由があります。 次に、いくつか示します。

- Windows の最新の更新プログラムを取得する 
- Office 365 ProPlus アプリケーションの最新の更新プログラムを取得する
- カスタム アプリを更新する
- イメージ自体の他の構成設定を変更する

更新したイメージを使用するコレクションの更新のエンド ツー エンド手順については、次のように移動します。 [ここ](remoteapp-update.md)します。 ただし、イメージおよび Office 365 ProPlus を更新する方法については、以下の情報を確認してください。

イメージを更新するには 2 つのオプションがあります。イメージを完全に新しいものに置き換える方法と、既存イメージを手動で更新する方法です。

### 最新の Azure ギャラリー イメージでイメージを置き換え、カスタマイズを追加する
このオプションでは、Microsoft に Windows Server および Office 365 ProPlus を更新させます。 既存のイメージを更新する代わりに、最新のギャラリー イメージを基にしてまったく新しいイメージを作成します。 次に、イメージをカスタマイズしたときの手順を繰り返し、カスタム アプリのインストールや、イメージ構成の変更などを行います。

ギャラリーのイメージは定期的に更新されるので、Windows Server および Office 365 ProPlus アプリも更新されます。 もちろん欠点もあり、新しいイメージを取得するたびに、カスタマイズを適用する必要があります。 カスタマイズの設定を自動化するスクリプトを作成できます。

### 既存のイメージを手動で更新する

このオプションでは、標準の Windows ツールを使用して、イメージに更新を適用します。 Office 365 ProPlus の場合、Office デプロイメント ツールを使用して、Office 365 ProPlus の最新の更新プログラムまたはバージョンをダウンロードしてインストールします。

> [AZURE.IMPORTANT] 注意してください: Office 365 ProPlus の自動更新を無効にします。

Office 展開ツールを使用した更新の詳細については以下をご覧ください。

- [Office 展開ツールを使用して Office 365 製品のクイック実行を展開する](https://technet.microsoft.com/library/JJ219423.aspx)
- [Office 展開ツールを使用して更新する Office 365 ProPlus の展開と](https://channel9.msdn.com/Events/Ignite/2015/BRK3168) (ビデオ)
- [Office 365 ProPlus の更新設定を構成する](https://technet.microsoft.com/library/dn761708.aspx)

