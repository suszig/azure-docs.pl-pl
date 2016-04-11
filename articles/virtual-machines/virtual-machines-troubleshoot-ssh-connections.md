<properties
    pageTitle="Azure VM への SSH 接続のトラブルシューティング | Microsoft Azure"
    description="Linux を実行する Azure 仮想マシンに対する SSH 接続の失敗や拒否などの SSH エラーをトラブルシューティングして修正します。"
    keywords="ssh connection refused,ssh error,azure ssh,SSH connection failed"
    services="virtual-machines"
    documentationCenter=""
    authors="dsk-2015"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2015"
    ms.author="dkshir"/>

# Linux ベースの Azure 仮想マシンに対する Secure Shell (SSH) 接続のトラブルシューティング

Linux ベースの Azure 仮想マシンに接続しようとしたときに発生する SSH エラーには、さまざまな原因が考えられます。 この記事は、そのような原因の特定と修正について説明します。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

この記事は、Linux を実行する Azure 仮想マシンにのみ適用されます。 Azure の仮想マシンが Windows を実行して、次を参照してください。 [Azure VM への接続をリモート デスクトップのトラブルシューティングを行う](virtual-machines-troubleshoot-remote-desktop-connections.md)します。

## Azure カスタマー サポートへの問い合わせ

この記事のどの時点でもその他のヘルプを必要がある場合で Azure のエキスパートに連絡することができます [MSDN Azure とスタック オーバーフロー フォーラム](http://azure.microsoft.com/support/forums/)します。

または、Azure サポート インシデントを送信できます。 移動して、 [Azure サポート サイト](http://azure.microsoft.com/support/options/) ] をクリック **サポートを受ける**します。 Azure のサポートを使用する方法の詳細については、読み取り、 [Microsoft Azure サポートに関する FAQ](http://azure.microsoft.com/support/faq/)します。


## クラシック デプロイメント モデルでの一般的な SSH エラーの修正手順

クラシック デプロイ モデルを使用して作成された仮想マシンの一般的な SSH 接続エラーを解決するには、次の手順を試してください。

1. **リモート アクセスのリセット** から、 [Azure ポータル](https://portal.azure.com)します。 をクリックして **[すべて参照** > **仮想マシン (クラシック)** > Windows 仮想マシン > **リモート アクセスのリセット**します。

    ![SSH 構成のリセットを示すスクリーンショット](./media/virtual-machines-troubleshoot-ssh-connections/Portal-SSH-Reset-Windows.png)  

2. **再起動** 仮想マシン。  [Azure ポータル](https://portal.azure.com), 、] をクリックして **[すべて参照** > **仮想マシン (クラシック)** > Windows 仮想マシン > **再起動**します。  [Azure クラシック ポータル](https://manage.windowsazure.com), を開き、 **仮想マシン** > **インスタンス** ] をクリック **再起動**します。

3. [**サイズを変更する** 仮想マシン](https://msdn.microsoft.com/library/dn168976.aspx)します。

4. 指示に従って、 [Linux ベースの仮想マシン用のパスワードまたは SSH をリセットする方法](virtual-machines-linux-use-vmaccess-reset-password-or-ssh.md) 仮想マシン上に。

    - パスワードまたは SSH キーをリセットする
    - 新しい sudo ユーザー アカウントを作成する
    - SSH 構成をリセットする

5. VM のリソースの状態でプラットフォームの問題を確認します。 
    すべての参照] をクリックして > 仮想マシン (クラシック) > Linux 仮想マシン > **正常性の確認**


## リソース マネージャー デプロイメント モデルでの一般的な SSH エラーの修正手順

リソース マネージャーのデプロイ モデルを使用して作成された仮想マシンの一般的な SSH の問題を解決するには、次の手順を試してください。

1. **SSH 接続をリセット** コマンド ラインで、Linux VM に、Azure CLI または Azure PowerShell を使用します。 必ず、 [Microsoft Azure Linux エージェント](virtual-machines-linux-agent-user-guide.md) バージョン 2.0.5 以降がインストールされています。

    **Azure CLI の使用**

    a. いない場合、 [Azure CLI をインストールし、Azure サブスクリプションに接続](../xplat-cli-install.md) を使用して、 `azure login` コマンドです。

    b. リソース マネージャー モードに切り替えます。

    ```
    azure config mode arm
    ```

    c. 次の方法のいずれかを使用して SSH 接続をリセットします。

    * 次の例のように、`vm reset-access` コマンドを使用します。

    ```
    azure vm reset-access -g TestRgV2 -n TestVmV2 -r
    ```

    これにより、`VMAccessForLinux` 拡張機能が仮想マシンにインストールされます。

    * あるいは、次の内容を含む PrivateConf.json という名前のファイルを作成します。

    ```
    {  
    "reset_ssh":"True"
    }
    ```

    その後、`VMAccessForLinux` 拡張機能を手動で実行して SSH 接続をリセットします。

    ```
    azure vm extension set "testRG" "testVM" VMAccessForLinux Microsoft.OSTCExtensions "1.2" --private-config-path PrivateConf.json
    ```

    **Azure PowerShell の使用**

    a. いない場合、 [Azure PowerShell をインストールし、Azure サブスクリプションに接続](../powershell-install-configure.md) 、Azure AD 方式を使用します。

    b. リソース マネージャー モードに切り替えます。

    ```
    Switch-AzureMode -Name AzureResourceManager
    ```

    c. 次の例のように、`VMAccessForLinux` 拡張機能を実行して SSH 接続をリセットします。

    ```
    Set-AzureVMExtension -ResourceGroupName "testRG" -VMName "testVM" -Location "West US" -Name "VMAccessForLinux" -Publisher "Microsoft.OSTCExtensions" -ExtensionType "VMAccessForLinux" -TypeHandlerVersion "1.2" -SettingString "{}" -ProtectedSettingString '{"reset_ssh":true}'
    ```

2. **再起動** ポータルから Linux 仮想マシン。  [Azure ポータル](https://portal.azure.com), 、] をクリックして **[すべて参照** > **仮想マシン** > Windows 仮想マシン > **再起動**します。

    ![V2 仮想マシンの再起動を示すスクリーンショット](./media/virtual-machines-troubleshoot-ssh-connections/Portal-SSH-Restart-V2-Windows.png)  

3. **パスワードまたは SSH キーをリセット** コマンド ラインで、Linux 仮想マシンの Azure CLI または Azure PowerShell を使用します。 次の例のように、sudo 権限を持つ新しいユーザー名/パスワードを作成することもできます。

    **Azure CLI の使用**

    既に説明したように Azure CLI をインストールして構成します。 リソース マネージャー モードに切り替えてから、次の方法のいずれかを使用して拡張機能を実行します。

    * `vm reset-access` コマンドを実行して任意の SSH 資格情報を設定します。

    ```
    azure vm reset-access TestRgV2 TestVmV2 -u NewUser -p NewPassword
    ```

    コマンド ラインで `azure vm reset-access -h` を入力すると、このコマンドの詳細が表示されます。

    * または、次の内容を含む PrivateConf.json という名前のファイルを作成します。
    ```
    {
    "username":"NewUsername", "password":"NewPassword", "expiration":"2016-01-01", "ssh_key":"", "reset_ssh":false, "remove_user":""
    }
    ```

    上記のファイルを使用して Linux 拡張機能を実行します。

    ```
    $azure vm extension set "testRG" "testVM" VMAccessForLinux Microsoft.OSTCExtensions "1.2" --private-config-path PrivateConf.json
    ```

    同様の手順を行うことができることに注意してください [Linux ベースの仮想マシン用のパスワードまたは SSH をリセットする方法](virtual-machines-linux-use-vmaccess-reset-password-or-ssh.md) 他のバリエーションを試みます。 必ずリソース マネージャー モード用に Azure CLI の命令を変更してください。

    **Azure PowerShell の使用**

    既に説明したように Azure PowerShell をインストールして構成します。 リソース マネージャー モードに切り替えてから、次のように拡張機能を実行します。

    ```
    $RGName = 'testRG'
    $VmName = 'testVM'
    $Location = 'West US'

    $ExtensionName = 'VMAccessForLinux'
    $Publisher = 'Microsoft.OSTCExtensions'
    $Version = '1.2'

    $PublicConf = '{}'
    $PrivateConf = '{"username":"NewUsername", "password":"NewPassword", "ssh_key":"", "reset_ssh":false, "remove_user":""}'

    Set-AzureVMExtension -ResourceGroupName $RGName -VMName $VmName -Location $Location -Name $ExtensionName -Publisher $Publisher -ExtensionType $ExtensionName -TypeHandlerVersion $Version -SettingString $PublicConf -ProtectedSettingString $PrivateConf

    ```

    必ず $RGName、$VmName、$Location、SSH 資格情報の値を、インストールに固有の値で置き換えてください。

## SSH エラーの詳細なトラブルシューティング

それでも SSH クライアントが仮想マシン上の SSH サービスに到達できない場合は、いくつかの理由が考えられます。 関連のあるコンポーネントを次に示します。

![SSH サービスのコンポーネントを示す図](./media/virtual-machines-troubleshoot-ssh-connections/ssh-tshoot1.png)

次のセクションは、エラーのソースを独立させて、解決策と回避策を見つけ出すのに役立ちます。

### トラブルシューティングする前の手順

まず、ポータルで仮想マシンの状態を確認します。

 [Azure クラシック ポータル](https://manage.windowsazure.com), 、従来のデプロイ モデル内の仮想マシンの。

1. クリックして **仮想マシン** > *VM 名*します。
2. [VM の] をクリックして **ダッシュ ボード** で状態をチェックします。
3. クリックして **モニター** にコンピューティング、ストレージ、およびネットワーク リソースの最近のアクティビティを参照してください。
4. クリックして **エンドポイント** に SSH トラフィックのエンドポイントがあることを確認します。

 [Azure ポータル](https://portal.azure.com):

1. 従来のデプロイ モデルで作成した仮想マシンをクリックして **参照** > **仮想マシン (クラシック)** > *VM 名*します。 リソース マネージャーを使用して作成された仮想マシンをクリックして **参照** > **仮想マシン** > *VM 名*します。 仮想マシンの状態ウィンドウを表示する必要があります **を実行している**します。 コンピューティング、ストレージ、およびネットワーク リソースの最近のアクティビティを確認するには、下にスクロールします。
2. クリックして **設定** エンドポイント、IP アドレス、およびその他の設定を確認します。 リソース マネージャーで作成された仮想マシン内のエンドポイントを特定する場合を確認、 [ネットワーク セキュリティ グループ](../traffic-manager/virtual-networks-nsg.md) 定義されると、対象となる規則とは、サブネット内の参照されているかどうか。

ネットワーク接続を確認するには、構成されているエンドポイントを確認します。また、HTTP などの別のプロトコルや他のサービスを使用して、VM に到達できるかどうかを確認します。

以上の手順を完了したら、SSH 接続を再試行してください。


### トラブルシューティングの手順

お使いのコンピューター上の SSH クライアントは、以下の問題または誤構成が原因で Azure 仮想マシンの SSH サービスに到達できない場合があります。

- SSH クライアント コンピューター
- 組織の境界デバイス
- クラウド サービス エンドポイントとアクセス制御リスト (ACL)
- ネットワーク セキュリティ グループ
- Linux ベースの Azure 仮想マシン

#### ソース 1: SSH クライアント コンピューター

エラーのソースであるコンピューターを排除するには、そのコンピューターから他のオンプレミスの Linux ベース コンピューターに SSH 接続できることを確認します。

![SSH クライアント コンピューターのコンポーネントを示す図](./media/virtual-machines-troubleshoot-ssh-connections/ssh-tshoot2.png)

接続できない場合は、そのコンピューターで以下を確認してください。

- 受信または送信の SSH トラフィック (TCP 22) をブロックしているローカル ファイアウォールの設定
- SSH 接続を妨げている、ローカルでインストールされたクライアント プロキシのソフトウェア
- SSH 接続を妨げている、ローカルでインストールされたネットワーク監視ソフトウェア
- トラフィックを監視する、あるいは特定種類のトラフィックを許可または禁止する別の種類のセキュリティ ソフトウェア

いずれの場合も、該当ソフトウェアを一時的に無効にし、オンプレミス コンピューターへの SSH 接続を試すことで、原因を確認してください。 次に、ネットワーク管理者とともに、SSH 接続が可能になるようにそのソフトウェアの設定を修正してください。

証明書認証を使用している場合は、ホーム ディレクトリの .ssh フォルダーに対してこれらのアクセス許可があることを確認します。

- Chmod 700 ~/.ssh
- Chmod 644 ~/.ssh/\*.pub
- Chmod 600 ~/.ssh/id_rsa (またはプライベート キーが格納されている他のファイル)
- Chmod 644 ~/.ssh/known_hosts (SSH 経由で接続したホストが含まれます)

#### ソース 2: 組織のエッジ デバイス

エラーのソースである組織のエッジ デバイスを排除するには、インターネットに直接接続されているコンピューターから Azure VM に SSH 接続できるかを確認します。 サイト間 VPN または ExpressRoute 接続経由で VM にアクセスする場合にスキップ [ソース 4: ネットワーク セキュリティ グループ](#nsg)します。

![組織の境界デバイスを示す図](./media/virtual-machines-troubleshoot-ssh-connections/ssh-tshoot3.png)

インターネットに直接接続されているコンピューターがない場合は、新しい Azure 仮想マシンをリソース グループまたはクラウド サービス内に簡単に作成して使用できます。 詳細については、次を参照してください。 [Azure で Linux を実行する仮想マシンを作成する](virtual-machines-linux-tutorial.md)です。 テストの完了後に、そのリソース グループまたは仮想マシンと、クラウド サービスを削除します。

インターネットに直接接続されているコンピューターとの SSH 接続を作成できるのなら、組織のエッジ デバイスで以下を確認してください。

- インターネットでの SSH トラフィックをブロックしている内部ファイアウォール
- SSH 接続を妨げているプロキシ サーバー
- SSH 接続を妨げている、エッジ ネットワーク内のデバイスで実行されている侵入検出またはネットワーク監視のソフトウェア

ネットワーク管理者と協力して、インターネットでの SSH トラフィックを許可するように組織のエッジ デバイスの設定を修正します。

#### ソース 3: クラウド サービス エンドポイントと ACL

> [AZURE.NOTE] このソースは、従来のデプロイ モデルを使用して作成されたバーチャル マシンにのみ適用されます。 リソース マネージャーを使用して作成された仮想マシンに進みます [ソース 4: ネットワーク セキュリティ グループ](#nsg)します。

Vm の作成を使用するために、エラーのソースとしてクラウド サービス エンドポイントと ACL を排除する、 [従来のデプロイ モデル](../resource-manager-deployment-model.md), 、同じ仮想ネットワーク内の別の Azure 仮想マシンが VM に SSH 接続を作成できることを確認します。

![クラウド サービス エンドポイントと ACL を示す図](./media/virtual-machines-troubleshoot-ssh-connections/ssh-tshoot4.png)

同じ仮想ネットワーク内に別の VM がない場合、新しい VM を簡単に作成することができます。 詳細については、次を参照してください。 [Azure で Linux を実行する仮想マシンを作成する](virtual-machines-linux-tutorial.md)です。 テストの完了後に、追加した VM を削除してください。

同じ仮想ネットワーク内にある VM に対して SSH 接続を作成できる場合は、次の点を確認します。

- ターゲットの VM での SSH トラフィック向けエンドポイントの構成。 エンドポイントのプライベート TCP ポートは、VM 上の SSH サービスがリッスンする TCP ポートと一致する必要があります (既定値は 22 です)。 テンプレートを使用して、リソース マネージャーの配置モデルに作成された Vm を使用して、Azure ポータルで SSH TCP ポート番号を確認 **参照** > **(v2) の仮想マシン** > *VM 名* > **設定** > **エンドポイント**します。
- ターゲットの仮想マシンでの、SSH トラフィック向けエンドポイントの ACL。 ACL を使用すると、発信元 IP アドレスに基づいて、インターネットからの受信トラフィックを許可または拒否するかを指定できます。 ACL が正しく構成されていないと、そのエンドポイントへの SSH 受信トラフィックを受け取れない場合があります。 ご利用になっているプロキシのパブリック IP アドレスからの受信トラフィック、または他のエッジ サーバーからの受信トラフィックが許可されているかを ACL で確認してください。 詳細については、次を参照してください。 [制御リスト (Acl) のネットワーク アクセスについて](../virtual-network/virtual-networks-acl.md)します。

エンドポイントをなくすため、問題の原因として、現在のエンドポイントを削除および新しいエンドポイントの作成を指定し、 **SSH** (TCP ポート 22 パブリックとプライベート ポート番号) の名前。 詳細については、次を参照してください。 [エンドポイントで、Azure の仮想マシンのセットアップ](virtual-machines-set-up-endpoints.md)します。

<a id="nsg"></a>
#### ソース 4: ネットワーク セキュリティ グループ

ネットワーク セキュリティ グループでは、許可された受信トラフィックと送信トラフィックをより細かく制御できます。 Azure 仮想ネットワーク内のサブネットまたはクラウド サービスの全体に適用されるルールを作成することができます。 ネットワーク セキュリティ グループ ルールで、インターネットからの SSH トラフィックが許可されていることを確認します。
詳細については、次を参照してください。 [ネットワーク セキュリティ グループについて](../traffic-manager/virtual-networks-nsg.md)します。

#### ソース 5: Linux ベースの Azure 仮想マシン

最後に考えられる問題のソースは、Azure 仮想マシン自体に関連するものです。

![Linux ベースの Azure 仮想マシンを示す図](./media/virtual-machines-troubleshoot-ssh-connections/ssh-tshoot5.png)

ない場合は、次の手順については、 [Linux ベースの仮想マシンのパスワードまたは SSH をリセットする](virtual-machines-linux-use-vmaccess-reset-password-or-ssh.md) 仮想マシンにします。

もう一度、コンピューターから接続を試みてください。 まだ接続できない場合は、以下の問題が考えられます。

- SSH サービスがターゲット仮想マシンで実行されていない。
- SSH サービスが TCP ポート 22 をリッスンしていない。 これをテストするローカル コンピューターに telnet クライアントをインストールして実行"telnet *cloudServiceName*..cloudapp.net 22" です。 これで、SSH エンドポイントに対する受信と送信の接続が仮想マシンで許可されているかどうかを確認できます。
- ターゲット仮想マシンのローカル ファイアウォールに、受信または送信の SSH トラフィックを妨げているルールがある。
- Azure 仮想マシンで実行されている侵入検出ソフトウェアまたはネットワーク監視ソフトウェアが、SSH 接続を妨げている。


## その他のリソース

従来のデプロイ モデル内の仮想マシンの [Linux ベースの仮想マシン用のパスワードまたは SSH をリセットする方法](virtual-machines-linux-use-vmaccess-reset-password-or-ssh.md)

[Windows ベースの Azure 仮想マシンへの Windows リモート デスクトップ接続に関するトラブルシューティング](virtual-machines-troubleshoot-remote-desktop-connections.md)

[Azure 仮想マシンで実行されているアプリケーションへのアクセスに関するトラブルシューティング](virtual-machines-troubleshoot-access-application.md)

