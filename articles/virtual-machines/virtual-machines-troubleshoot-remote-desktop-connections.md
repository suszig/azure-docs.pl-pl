<properties
    pageTitle="Azure VM へのリモート デスクトップ接続のトラブルシューティング |Microsoft Azure"
    description="Windows VM に対するリモート デスクトップ接続のエラーをトラブルシューティングします。 簡単な軽減手順、エラー メッセージごとのヘルプ、および詳細なネットワーク トラブルシューティングを説明します。"
    keywords="Remote desktop error,remote desktop connection error,cannot connect to VM,remote desktop troubleshooting"
    services="virtual-machines"
    documentationCenter=""
    authors="dsk-2015"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2015"
    ms.author="dkshir"/>

# Windows を実行する Azure 仮想マシンへの Remote Desktop 接続に関するトラブルシューティング

Windows ベースの Azure 仮想マシンに接続しようとしたときに発生するリモート デスクトップ (RDP) エラーにはいくつかの原因が考えられます。 VM 上の RDP ソフトウェア、基になっているホスト コンピューター、ネットワーク接続、接続元のクライアント側などに関する問題が発生します。 この記事は、原因を特定して修正するために役立ちます。  

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

この記事は、Windows を実行する Azure Virtual Machines にのみ適用されます。 Azure の仮想マシンが Linux を実行して、次を参照してください。 [のトラブルシューティングを行う SSH 接続 Azure VM に](virtual-machines-troubleshoot-ssh-connections.md)します。

この記事のどの時点でもその他のヘルプを必要がある場合で Azure のエキスパートに連絡することができます [MSDN Azure とスタック オーバーフロー フォーラム](http://azure.microsoft.com/support/forums/)します。 または、Azure サポート インシデントを送信できます。 移動して、 [Azure サポート サイト](http://azure.microsoft.com/support/options/) ] をクリック **サポートにお問い合わせ**します。

最初の「基本的な手順」セクションでは、一般的な接続の問題に対処する手順を示します。2 番目のセクションでは、具体的なエラー メッセージごとに解決手順を説明します。最後のセクションでは、各ネットワーク コンポーネントの詳細なトラブルシューティングの実行について説明します。

## クラシック デプロイメント モデルでの一般的なリモート デスクトップ エラーの修正手順

これらの基本手順を使用すると、クラシック デプロイメント モデルを使用して作成された仮想マシンのリモート デスクトップ接続に関する一般的なエラーをほとんど解決できます。 各手順を実行した後、仮想マシンに再接続してみてください。

- リモート デスクトップ サービスを再設定、 [Azure ポータル](https://portal.azure.com) RDP サーバーとスタートアップの問題を解決します。<br>
    すべての参照] をクリックして > 仮想マシン (クラシック) > Windows 仮想マシン > **リモート アクセスのリセット**します。

    ![RDP 構成のリセットを示すスクリーンショット](./media/virtual-machines-troubleshoot-remote-desktop-connections/Portal-RDP-Reset-Windows.png)

- その他の起動の問題に対処するには仮想マシンを再起動します。<br>
    すべての参照] をクリックして > 仮想マシン (クラシック) > Windows 仮想マシン > **再起動**します。

- ホストの問題を解決するには、VM のサイズを変更します。<br>
    すべての参照] をクリックして > 仮想マシン (クラシック) > Windows 仮想マシン > 設定 > **サイズ**します。 詳細については、次を参照してください。 [仮想マシンのサイズを変更する](https://msdn.microsoft.com/library/dn168976.aspx)です。

- 起動の問題を修正するには、VM のコンソール ログまたはスクリーンショットを確認してください。
    すべての参照] をクリックして > 仮想マシン (クラシック) > Windows 仮想マシン > **診断を起動**

- VM のリソースの状態でプラットフォームの問題を確認します。 
    すべての参照] をクリックして > 仮想マシン (クラシック) > Windows 仮想マシン > **正常性の確認**

## リソース マネージャー デプロイメント モデルでの一般的なリモート デスクトップ エラーの修正手順

これらの基本手順を使用すると、リソース マネージャー デプロイメント モデルを使用して作成された仮想マシンのリモート デスクトップ接続に関する一般的なエラーをほとんど解決できます。 各手順を実行した後、仮想マシンに再接続してみてください。

- Powershell を使用してリモート アクセスをリセットします。<br>
    a. いない場合、 [Azure PowerShell をインストールし、Azure サブスクリプションに接続](../powershell-install-configure.md) 、Azure AD 方式を使用します。

    b. リソース マネージャー モードに切り替えます。

    ```
    Switch-AzureMode -Name AzureResourceManager
    ```
    c. 次の例で示すように、Set-AzureVMAccessExtension コマンドを実行して RDP 接続をリセットします。

    ```
    Set-AzureVMExtension -ResourceGroupName "myRG" -VMName "myVM" -Name "myVMAccessExtension" -ExtensionType "VMAccessAgent" -Publisher "Microsoft.Compute" -typeHandlerVersion "2.0" -Location Westus
    ```

- その他の起動の問題に対処するには仮想マシンを再起動します。<br>
    すべての参照] をクリックして > 仮想マシン > Windows 仮想マシン > **再起動**します。

- ホストの問題を解決するには、VM のサイズを変更します。<br>
    すべての参照] をクリックして > 仮想マシン > Windows 仮想マシン > 設定 > **サイズ**します。

- 起動の問題を修正するには、VM のコンソール ログまたはスクリーンショットを確認してください。
    すべての参照] をクリックして > 仮想マシン > Windows 仮想マシン > **診断を起動**


## 特定のリモート デスクトップ接続エラーのトラブルシューティング

Azure 仮想マシンに Remote Desktop 接続しようとしたときに発生する可能性がある一般的なエラーは次のとおりです。

1. [リモート デスクトップ接続エラー: リモート デスクトップ ライセンスを提供するサーバーのライセンスがないために、リモート セッションが切断されました](#rdplicense)します。

2. [リモート デスクトップ接続エラー: リモート デスクトップはコンピューター"name"を見つけられない](#rdpname)します。

3. [Remote Desktop 接続エラー: 認証エラーが発生しました。ローカル セキュリティ機関にアクセスできません。](#rdpauth)

4. [Windows のセキュリティ エラー: 資格情報が正しくない](#wincred)します。

5. [リモート デスクトップ接続エラー: このコンピューターがリモート コンピューターに接続できません](#rdpconnect)します。

<a id="rdplicense"></a>
### Remote Desktop 接続エラー: ライセンスを提供する Remote Desktop ライセンス サーバーがないため、リモート セッションが切断されました。

原因: リモート デスクトップ サーバー ロールの 120 日間のライセンス有効期限が切れているため、ライセンスをインストールする必要がある。

回避策として、ポータルから RDP ファイルのローカル コピーを保存し、Windows PowerShell コマンド プロンプトで次のコマンドを実行して接続します。

        mstsc <File name>.RDP /admin

これによって、この接続のみに対するライセンスが無効になります。

仮想マシンに対して 2 つ以上のリモート デスクトップ接続が同時に必要でない場合、サーバー マネージャーを使用して、リモート デスクトップ サーバーのロールを削除することができます。

参照してください、 [「はリモート デスクトップ ライセンス サーバーを利用できる」Azure VM が失敗した](http://blogs.msdn.com/b/wats/archive/2014/01/21/rdp-to-azure-vm-fails-with-quot-no-remote-desktop-license-servers-available-quot.aspx) ブログの投稿です。

<a id="rdpname"></a>
### Remote Desktop 接続エラー:  Remote Desktop は、コンピューター "name" を見つけることができません。

原因: お使いのコンピューターの Remote Desktop クライアントが、RDP ファイルに設定されたコンピューターの名前を解決できません。

考えられる解決策:

- 組織のイントラネットを使用している場合は、コンピューターからプロキシ サーバーにアクセスでき、そのサーバーに HTTPS トラフィックを送信できることを確認してください。
- ローカルに保存した RDP ファイルを使用している場合は、ポータルで生成されたファイルを試してください。 仮想マシンまたはクラウド サービスの適切な DNS 名と、仮想マシンのエンドポイント ポートが使用されます。 ポータルで生成される RDP ファイルの例を次に示します。

        full address:s:tailspin-azdatatier.cloudapp.net:55919
        prompt for credentials:i:1

この RDP ファイルのアドレス部分には、VM を含むクラウド サービスの完全修飾ドメイン名 (この例では tailspin-azdatatier.cloudapp.net) と、Remote Desktop トラフィック用のエンドポイントの外部 TCP ポート (55919) が含まれます。

<a id="rdpauth"></a>
### Remote Desktop 接続エラー: 認証エラーが発生しました。 ローカル セキュリティ機関にアクセスできません。

原因: ターゲット VM が、資格情報のユーザー名の部分でセキュリティ機関を見つけることができませんでした。

ユーザー名があるとき、フォーム *SecurityAuthority*\\*ユーザー名* (例: corp \user1)、 *SecurityAuthority* 部分には、バーチャル マシンのコンピューター名 (ローカル セキュリティ機関) か、Active Directory ドメイン名です。

考えられる解決策:

- ユーザー アカウントが VM にとってローカルの場合は、VM 名のスペルが正しいことを確認します。
- ユーザー アカウントが Active Directory ドメイン アカウントの場合は、ドメイン名のスペルを確認します。
- ユーザー アカウントが Active Directory ドメイン アカウントであり、ドメイン名のスペルが正しい場合は、ドメイン コント ローラーがそのドメインで利用可能であることを確認します。 この問題は、Azure Virtual Network にドメイン コント ローラーを含まれていて、ドメイン コント ローラー コンピューターが起動されていない場合によく発生します。 回避策として、ドメイン アカウントではなく、ローカル管理者アカウントを使用できます。

<a id="wincred"></a>
### Windows セキュリティ エラー: 資格情報が正しくありません。

原因: ターゲット VM が、アカウント名とパスワードを検証できませんでした。

Windows ベースのコンピューターでは、ローカル アカウントとドメイン アカウントの資格情報を認証できます。

- ローカル アカウントを使用して、 *ComputerName*\\*UserName* 構文 (例: sql1 \admin4798)。
- ドメイン アカウントを使用して、 *DomainName*\\*UserName* 構文 (例: contoso \johndoe)。

新しい AD フォレスト内で仮想マシンがドメイン コントローラーに昇格している場合、ログイン時に使用したローカル管理者アカウントも、新しいフォレストとドメイン内で同じパスワードを持つ同等のアカウントに変換されます。 その後、ローカル管理者アカウントは削除されます。 たとえば、ローカル管理者アカウント DC1\DCAdmin でログインして、新しいフォレストで仮想マシンを corp.contoso.com ドメインのドメイン コント ローラーとして昇格すると、DC1\DCAdmin のローカル アカウントは削除され、新しいドメイン アカウント (CORP\DCAdmin) が同じパスワードで作成されます。

アカウント名が仮想マシンで有効なアカウントとして認証される名前であること、パスワードが正しいことを確認します。

ローカル管理者アカウントのパスワードを変更する必要がある場合は、次を参照してください。 [パスワードまたはリモート デスクトップ サービスを Windows 仮想マシンをリセットする方法](virtual-machines-windows-reset-password.md)します。

<a id="rdpconnect"></a>
### Remote Desktop 接続エラー: このコンピューターはリモート コンピューターに接続できません。

原因: 接続に使用しているアカウントに、Remote Desktop ログオン権限がありません。

すべての Windows コンピューターには Remote Desktop ユーザーのローカル グループがあり、このグループには、リモートでログオンできるアカウントとグループが含まれます。 ローカルの Administrators グループのメンバーもアクセスできますが、これらのアカウントは Remote Desktop ユーザーのローカル グループのメンバーとしてリストされません。 ドメインに参加しているマシンの場合、ローカルの Administrators グループにはドメインのドメイン管理者も含まれます。

接続するために使用しているアカウントに、Remote Desktop ログオン権限があることを確認してください。 この問題を回避するには、リモート デスクトップ経由で接続し、コンピューターの管理スナップインで使用するドメインまたはローカル管理者アカウントを使用 (**システム ツール > ローカル ユーザーとグループ > グループ > リモート デスクトップ ユーザー**) Remote Desktop Users ローカル グループに必要なアカウントを追加します。

## リモート デスクトップ エラーの詳細なトラブルシューティング

これらのエラーの発生したし、も接続できませんでしたリモート デスクトップを介して仮想マシンに、読み取る [この記事](virtual-machines-rdp-detailed-troubleshoot.md) その他の原因を確認します。


## その他のリソース

[Azure IaaS (Windows) 診断パッケージ](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)

[Windows 仮想マシンのパスワードまたはリモート デスクトップ サービスをリセットする方法](virtual-machines-windows-reset-password.md)

[Azure PowerShell のインストールおよび構成方法](../install-configure-powershell.md)

[Linux ベースの Azure 仮想マシンに対する Secure Shell (SSH) 接続のトラブルシューティング](virtual-machines-troubleshoot-ssh-connections.md)

[Azure 仮想マシンで実行されているアプリケーションへのアクセスに関するトラブルシューティング](virtual-machines-troubleshoot-access-application.md)

