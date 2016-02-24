<properties 
    pageTitle="Azure RemoteApp のハイブリッド コレクションの作成方法 | Microsoft Azure" 
    description="内部ネットワークに接続する RemoteApp のデプロイを作成する方法について説明します。" 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/04/2015" 
    ms.author="elizapo"/>

# Azure RemoteApp のハイブリッド コレクションの作成方法

Azure RemoteApp のコレクションには、次の 2 種類があります。 

- クラウド: Azure に完全に常駐します。 クラウドにすべてのデータを保存するか (したがって、クラウドのみのコレクション)、コレクションを VNET に接続して、そこにデータを保存するかを選択できます。   
- ハイブリッド: オンプレミス アクセス用の仮想ネットワークを含みます。これには、Azure AD およびオンプレミスの Active Directory 環境を使用する必要があります。

何が必要かわかりませんか。 チェック アウト [Azure RemoteApp の必要なコレクションの種類](remoteapp-collections.md)します。

このチュートリアルでは、ハイブリッド コレクションを作成する手順について説明します。 次の 8 つの手順があります。 

1.  対応を決める [イメージ](remoteapp-imageoptions.md) をコレクションに使用します。 これには、カスタム イメージを作成するか、サブスクリプションに含まれているいずれかの Microsoft イメージを使用します。
2. 仮想ネットワークをセットアップする。 チェック アウト、 [VNET 計画](remoteapp-planvpn.md) と [sizing](remoteapp-vnetsizing.md) 情報。
2.  コレクションを作成する。
2.  コレクションをローカル ドメインに参加させます。
3.  テンプレート イメージをコレクションに追加する。
4.  ディレクトリ同期を構成する。 Azure RemoteApp を使用するには、次のいずれかの方法で Azure Active Directory と統合する必要があります。1) パスワード同期オプションを使用して Azure Active Directory 同期を構成する。2) パスワード同期オプションを使用しないで Azure Active Directory 同期を構成する。ただし、この場合は AD FS にフェデレーションされたドメインを使用する。 チェック アウト、 [RemoteApp での Active Directory の構成情報](remoteapp-ad.md)します。
5.  RemoteApp のアプリを発行する。
6.  ユーザー アクセスを構成する。

**開始する前に**

コレクションを作成する前に以下の操作が必要です:

- [サインアップ](http://azure.microsoft.com/services/remoteapp/) Azure RemoteApp のです。 
- Azure RemoteApp サービス アカウントとして使用するためのユーザー アカウントを Active Directory に作成します。 ドメインへのマシンの参加のみが実行可能になるように、このアカウントのアクセス許可を制限します。
- オンプレミスのネットワークに関する情報、つまり IP アドレス情報と VPN デバイスの詳細情報を収集します。
- インストール、 [Azure PowerShell](../install-configure-powershell.md) モジュールです。
- アクセス権を付与するユーザーに関する情報を集めます。 Azure Active Directory のユーザー プリンシパル名 (name@contoso.com など) は、各ユーザーの必要があります。 Azure AD と Active Directory 間で UPN が一致していることを確認します。
- テンプレート イメージを選択します。 Azure RemoteApp テンプレート イメージには、ユーザーに発行するアプリケーションとプログラムが含まれます。 参照してください [Azure RemoteApp のイメージ オプション](remoteapp-imageoptions.md) の詳細。 
- Office 365 ProPlus イメージを使用する必要がありますか。 チェック アウト情報 [ここ](remoteapp-officesubscription.md)します。
- [RemoteApp の Active Directory を構成する](remoteapp-ad.md)です。



## 手順 1. 仮想ネットワークをセットアップする
既存の Azure 仮想ネットワークを使用するハイブリッド コレクションをデプロイすることも、新しい仮想ネットワークを作成することもできます。 ユーザーは仮想ネットワークを利用することで、ローカル ネットワーク上のデータに RemoteApp リモート リソースを介してアクセスします。 Azure 仮想ネットワークを使用すると、コレクションは、他の Azure サービスおよびその仮想ネットワークにデプロイされている仮想マシンへの直接ネットワーク アクセスが可能になります。

確認してください、 [VNET 計画](remoteapp-planvnet.md) と [VNET サイズ](remoteapp-vnetsizing.md) については、VNET を作成する前にします。

### Azure VNET を作成して Active Directory のデプロイに参加させる

まず、作成、 [仮想ネットワーク](../virtual-network/virtual-networks-create-vnet.md)します。 これを行う、 **ネットワーク** Azure 管理ポータルでタブをクリックします。 仮想ネットワークを、Azure Active Directory テナントと同期している Active Directory デプロイに接続する必要があります。

参照してください [仮想ネットワークの設定について、管理ポータルで](../virtual-network/virtual-networks-settings.md) の詳細。

### 仮想ネットワークで Azure RemoteApp を使用する準備ができていることを確認します。
コレクションを作成する前に、新しい仮想ネットワークで準備ができていることを確認します。 これは次の手順を実行することで検証できます。

1. RemoteApp 用に作成した仮想ネットワークのサブネット内に Azure 仮想マシンを作成します。
2. リモート デスクトップを使用して仮想マシンに接続します (をクリックして **接続**.)
3. RemoteApp 用に使用するのと同じ Active Directory デプロイに参加します。

これで動作するかを確認します。 動作すれば、仮想ネットワークとサブネットは Azure RemoteApp を使用する準備ができています。

リモート デスクトップを使用して Azure の仮想マシンの作成と接続の詳細についてを見つけることができます [ここ](https://msdn.microsoft.com/library/azure/jj156003.aspx)します。

## 手順 2: Azure RemoteApp コレクションを作成する ##



1.  [Azure ポータル](http://manage.windowsazure.com), 、Azure RemoteApp] ページに移動します。
2. クリックして **新しい > VNET で作成**します。
3. コレクションの名前を入力します。
4. Standard または Basic から、使用するプランを選択します。
5. ドロップダウン リストから VNET を選択し、サブネットを選択します。
6. それがドメインに参加するよう選択します。
5. クリックして **RemoteApp コレクションの作成**します。

Azure RemoteApp コレクションが作成されたら、コレクションの名前をダブルクリックします。 表示されます、 **クイック スタート** ページ - これは、コレクションの構成完了します。

何か問題がありますか。 チェック アウト、 [ハイブリッド コレクションのトラブルシューティング情報](remoteapp-hybridtrouble.md)します。

## 手順 3. コレクションをローカル ドメインに関連付ける ##

 
1.  **クイック スタート** ] ページで [ **ローカル ドメインに参加させる**します。
2. Azure RemoteApp サービス アカウントをローカル Active Directory ドメインに追加します。 追加するには、ドメイン名、組織単位、サービス アカウントのユーザー名とパスワードが必要になります。 

    これは、手順を実行するかどうかは、収集した情報は [Azure RemoteApp の Active Directory の構成](remoteapp-ad.md)します。


## 手順 4: Azure RemoteApp イメージのリンクを作成する ##

Azure RemoteApp テンプレート イメージにはユーザーと共有するプログラムが含まれています。 作成することも、新しい [テンプレート イメージ](remoteapp-imageoptions.md) または既存のイメージ (既にインポートされたか、Azure RemoteApp にアップロードされるもの) にリンクします。 Azure RemoteApp のいずれかにリンクすることもできます。 [テンプレート イメージ](remoteapp-images.md) Office 365 または Office 2013 (評価版) プログラムが含まれています。 

新しいイメージをアップロードする場合は、イメージの名前を入力して場所を選択する必要があります。 ウィザードの次のページに PowerShell コマンドレットのセットが表示されます。特定のイメージをアップロードするために、このコマンドレットをコピーして管理者特権の Windows PowerShell プロンプトから実行します。

既存のテンプレート イメージを関連付ける場合は、そのイメージ名、保管場所、関連先の Azure のサブスクリプションを指定するだけで実行できます。



## 手順 5. Active Directory ディレクトリの同期を構成する ##

Azure RemoteApp を使用するには、次のいずれかの方法で Azure Active Directory と統合する必要があります。1) パスワード同期オプションを使用して Azure Active Directory 同期を構成する。2) パスワード同期オプションを使用しないで Azure Active Directory 同期を構成する。ただし、この場合は AD FS にフェデレーションされたドメインを使用する。 

チェック アウト [AD Connect](http://blogs.technet.com/b/ad/archive/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect.aspx) - この記事の 4 つの手順で、ディレクトリ統合を設定することができます。

参照してください [ディレクトリ同期のロードマップ](http://msdn.microsoft.com//library/azure/hh967642.aspx) 計画に関する情報、および詳細な手順を実行します。

## 手順 6: アプリを発行する ##

Azure RemoteApp アプリケーションは、ユーザーに提供するアプリケーションまたはプログラムのことです。 このプログラムは、コレクション用にアップロードしたテンプレート イメージに置かれています。 ユーザーがアプリケーションにアクセスすると、アプリケーションはユーザーのローカル環境で実行しているように見えますが、実際には Azure で実行しています。 

ユーザーからアプリケーションへのアクセスを可能にするには、最初にこのアプリケーションをエンド ユーザー フィードに発行する必要があります。エンド ユーザー フィードとは、ユーザーがリモート デスクトップ クライアントを通じてアクセスできるアプリケーションの一覧です。
 
コレクションには複数のアプリケーションを発行できます。 発行ページからクリックして **発行** アプリケーションを追加します。 発行するか、 **開始** またはアプリのテンプレート イメージ上のパスを指定して、テンプレート イメージのメニュー。 追加する場合、 **開始** ] メニューの [プログラムの追加を選択します。 アプリケーションのパスを指定して発行する場合は、アプリケーションの名前と、アプリケーションがインストールされるテンプレート イメージ上の保存場所までのパスを指定します。

## 手順 7. ユーザー アクセスを構成する ##

これでコレクションが作成できたので、リモート リソースを使用可能にするユーザーを追加する必要があります。 アクセス権を付与するユーザーは、この Azure RemoteApp コレクションを作成するために使用したサブスクリプションに関連付けられた Active Directory テナントに存在している必要があります。

1.  クイック スタート] ページで、次のようにクリックします。 **ユーザー アクセスの構成**します。 
2.  アクセス権を付与する (Active Directory の) 仕事用アカウントか、または Microsoft アカウントを入力します。

    **注:** 

    "user@domain.com" 形式を使用するようにしてください。

    コレクションで Office 365 ProPlus を使用している場合は、ユーザーの Active Directory ID を使用する必要があります。 これにより,ライセンスを検証できます。 


3.  ユーザーが検証されたら、クリックして **保存**します。


## 次のステップ ##
これで、Azure RemoteApp ハイブリッド コレクションの作成とデプロイが正常に完了しました。 次のステップは、ユーザーによるリモート デスクトップ クライアントのダウンロードとインストールです。 このクライアントの URL は、Azure RemoteApp の [クイック スタート] ページにあります。 次に、ユーザーがクライアントにログインし、発行したアプリケーションにアクセスします。


 
### サポートのお願い 
記事を評価したり、下にコメントを投稿したりするだけでなく、記事自体を変更できることを知っていましたか。 説明不足ですか。 間違いがありますか。 わかりにくいことが書いてありますか。 上にスクロールしてクリックして **GitHub の編集** の変更を加えるについては、私たちに来るし、次に、筆に一度表示されます、変更および強化ここです。
