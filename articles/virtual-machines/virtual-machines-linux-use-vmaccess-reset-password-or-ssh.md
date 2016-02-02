<properties
    pageTitle="Azure CLI からの Linux VM パスワードのリセット | Microsoft Azure"
    description="Azure クラシック ポータルまたは CLI の VMAccess 拡張機能を使用して、Linux VM パスワードと SSH キー、SSH 構成をリセットし、ユーザー アカウントを削除する方法を説明します。"
    services="virtual-machines"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/28/2015"
    ms.author="cynthn"/>


# Linux 仮想マシンのパスワードまたは SSH をリセットする方法

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


パスワードを忘れた、Secure Shell (SSH) キーが正しくない、または SSH の構成に問題のため、Linux 仮想マシンに接続できない場合は、Azure ポータルまたは VMAccessForLinux 拡張機能を使用して、パスワードまたは SSH キーをリセットするか、または SSH 構成を修正します。 この記事は、**クラシック** デプロイ モデルを使用して作成された仮想マシンを対象としています。

## Azure ポータル

SSH の構成をリセットする、 [Azure ポータル](https://portal.azure.com), 、] をクリックして **参照** > **仮想マシン** > *Linux 仮想マシン* > **リモート アクセスのリセット**します。 たとえば次のようになります。

![](./media/virtual-machines-linux-use-vmaccess-reset-password-or-ssh/Portal-RDP-Reset-Linux.png)

名前と sudo 特権またはの SSH 公開キーを持つユーザー アカウントのパスワードをリセットする、 [Azure ポータル](https://portal.azure.com), 、] をクリックして **参照** > **仮想マシン** > *Linux 仮想マシン* > **すべての設定** > **パスワード リセット**します。 たとえば次のようになります。

![](./media/virtual-machines-linux-use-vmaccess-reset-password-or-ssh/Portal-PW-Reset-Linux.png)


## Azure CLI および PowerShell

以下のものが必要になります。

- Microsoft Azure Linux エージェント バージョン 2.0.5 またはそれ以降。 仮想マシン ギャラリー内のほとんどの Linux イメージには、バージョン 2.0.5 が含まれています。 インストールされているバージョンを確認するには、**waagent -version** を実行します。 エージェントを更新するには、[Azure Linux エージェント ユーザー ガイド] 設定を指示します。
- Azure コマンド ライン インターフェイス (CLI)。 Azure CLI の設定の詳細については、「 [インストールして、Azure コマンド ライン インターフェイスを構成する](../xplat-cli-install.md)します。
- Azure PowerShell。 Set-AzureVMExtension コマンドレットでコマンドを使用して、VMAccessForLinux 拡張機能を自動的に読み込み、構成します。 詳細については、Azure PowerShell の設定は、[Azure PowerShell を構成する方法] を参照してください。
- 新しいパスワードまたは一連の SSH キー (いずれかをリセットする場合)。 SSH の構成をリセットする場合、これらは必要ありません。

### インストールは必要ありません

VMAccess 拡張機能は使用前にインストールする必要はありません。 Linux エージェントが仮想マシンにインストールされていれば、**Set-AzureVMExtension** コマンドレットを使用する Azure PowerShell コマンドを実行すると自動的に拡張機能が読み込まれます。

## Azure CLI の使用

Azure CLI を使用すると、コマンド ライン インターフェイス (Bash、Terminal、Command プロンプト) から **azure** コマンドを使用してコマンドにアクセスできます。 拡張機能の詳しい使用方法については、**azure vm extension set –help** を実行します。

Azure CLI を使用すると、次のタスクを実行できます。

+ [パスワードをリセットします。](#pwresetcli)
+ [SSH キーをリセットします。](#sshkeyresetcli)
+ [パスワードと SSH キーをリセットします。](#resetbothcli)
+ [新しい sudo ユーザー アカウントを作成します。](#createnewsudocli)
+ [SSH 構成をリセットします。](#sshconfigresetcli)
+ [ユーザーを削除します。](#deletecli)
+ [VMAccess 拡張機能の状態を表示します。](#statuscli)

### <a name="pwresetcli"></a>パスワードをリセットします。

手順 1. プレースホルダーの値を置き換えて、次の内容を含む PrivateConf.json という名前のファイルを作成しします。

    {
    "username":"currentusername",
    "password":"newpassword",
    "expiration":"2016-01-01",
    }

手順 2. "vmname" を使用中の仮想マシンの名前に置き換えて、次のコマンドを実行します。

    azure vm extension set vmname VMAccessForLinux Microsoft.OSTCExtensions 1.* –-private-config-path PrivateConf.json

### <a name="sshkeyresetcli"></a>SSH キーをリセットします。

手順 1. プレースホルダーの値を置き換えて、次の内容を含む PrivateConf.json という名前のファイルを作成しします。

    {
    "username":"currentusername",
    "ssh_key":"contentofsshkey",
    }

手順 2. "vmname" を使用中の仮想マシンの名前に置き換えて、次のコマンドを実行します。

    azure vm extension set vmname VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

### <a name="resetbothcli"></a>パスワードと SSH キーをリセットします。

手順 1. プレースホルダーの値を置き換えて、次の内容を含む PrivateConf.json という名前のファイルを作成しします。

    {
    "username":"currentusername",
    "ssh_key":"contentofsshkey",
    "password":"newpassword",
    }

手順 2. "vmname" を使用中の仮想マシンの名前に置き換えて、次のコマンドを実行します。

    azure vm extension set vmname VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

### <a name="createnewsudocli"></a>新しい sudo ユーザー アカウントを作成します。

自身のユーザー名を忘れた場合は、VMAccess を使用して、sudo 権限を持つ新しいユーザーを作成できます。 この場合、既存のユーザー名とパスワードは変更されません。

パスワードのアクセス権を持つ新しい sudo ユーザーを作成するでスクリプトを使用 [パスワードをリセット](#pwresetcli) 新しいユーザー名を指定します。

SSH キーのアクセス権を持つ新しい sudo ユーザーを作成するには、内のスクリプトを使用して [SSH キーのリセット](#sshkeyresetcli) 新しいユーザー名を指定します。

使用することも [パスワードと SSH キーのリセット](#resetbothcli) パスワードと SSH キーへのアクセスの両方で、新しいユーザーを作成します。

### <a name="sshconfigresetcli"></a>SSH 構成をリセットします。

SSH の構成が望ましい状態でない場合は、VM にアクセスできなくなる可能性もあります。 VMAccess 拡張機能を使用して、構成を既定の状態にリセットすることができます。 そのために必要なのは、"reset_ssh" キーを "True" に設定することだけです。 拡張機能によって SSH サーバーが再起動し、VM 上の SSH ポートが開いて、SSH 構成が既定値にリセットされます。 ユーザー アカウント (名前、パスワード、または SSH キー) は変更されません。
> [AZURE.NOTE] リセットされる SSH の構成ファイルは、/etc/ssh/sshd_config にあります。

手順 1. 次の内容を含む PrivateConf.json という名前のファイルを作成します。

    {
    "reset_ssh":"True",
    }

手順 2. "vmname" を使用中の仮想マシンの名前に置き換えて、次のコマンドを実行します。

    azure vm extension set vmname VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

### <a name="deletecli"></a>ユーザーを削除します。

VM にログインせずに直接ユーザー アカウントを削除するには、このスクリプトを使用できます。

手順 1. プレースホルダーの値を置き換えて、次の内容を含む PrivateConf.json という名前のファイルを作成します。

    {
    "remove_user":"usernametoremove",
    }

手順 2. "vmname" を使用中の仮想マシンの名前に置き換えて、次のコマンドを実行します。

    azure vm extension set vmname VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

### <a name="statuscli"></a>VMAccess 拡張機能の状態を表示します。

VMAccess 拡張機能の状態を表示するには、次のコマンドを実行します。

    azure vm extension get

## Azure PowerShell の使用

**Set-AzureVMExtension** コマンドレットを使用して、VMAccess に変更を加えます。 すべてのケースで、クラウド サービス名と仮想マシン名を使用して開始して、仮想マシン オブジェクトを取得し、変数に格納します。

クラウド サービス名と仮想マシン名を入力して、管理者レベルの Azure PowerShell のコマンド プロンプトで次のコマンドを実行します。置換を含む引用符内のすべての < と > 文字です。

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName

クラウド サービスや仮想マシンの名前がわからない場合は、**Get-AzureVM** を実行します。現在のサブスクリプションのすべての VM の情報が表示されます。

> [AZURE.NOTE] $ で始まるコマンド ラインは、後で PowerShell コマンドで使用する PowerShell 変数を設定します。

Azure クラシック ポータルで仮想マシンを作成する場合は、次の追加のコマンドを実行します。

    $vm.GetInstance().ProvisionGuestAgent = $true

このコマンドは、次のセクションで Set-AzureVMExtension コマンドを実行する場合、"IaaS VM アクセス拡張機能を設定する前に、VM オブジェクトでゲスト エージェントのプロビジョニングを有効にする必要がある" というエラーを防ぐことができます。

次に、以下のタスクを実行できます。

+ [パスワードをリセットします。](#password)
+ [SSH キーをリセットします。](#SSHkey)
+ [パスワードと SSH キーをリセットします。](#both)
+ [SSH 構成をリセットします。](#config)
+ [ユーザーを削除します。](#delete)
+ [VMAccess 拡張機能の状態を表示します。](#status)

### <a name="password"></a>パスワードをリセットします。

Linux の現在のユーザー名と新しいパスワードを入力し、これらのコマンドを実行します。

    $UserName = "<current Linux account name>"
    $Password = "<new password>"
    $PrivateConfig = '{"username":"' + $UserName + '", "password": "' +  $Password + '"}'
    $ExtensionName = "VMAccessForLinux"
    $Publisher = "Microsoft.OSTCExtensions"
    $Version =  "1.*"
    Set-AzureVMExtension -ExtensionName $ExtensionName -VM $vm -Publisher $Publisher -Version $Version -PrivateConfiguration $PrivateConfig | Update-AzureVM

> [AZURE.NOTE] 既存のユーザー アカウント用のパスワードまたは SSH キーをリセットする場合は、必ず正確なユーザー名を入力してください。 別の名前を入力した場合は、VMAccess 拡張機能が新しいユーザー アカウントを作成し、そのアカウントにパスワードを割り当てます。


### <a name="SSHKey"></a>SSH キーをリセットします。

Linux の現在のユーザー名と、SSH キーを含む証明書へのパスを入力し、これらのコマンドを実行します。

    $UserName = "<current Linux user name>"
    $Cert = Get-Content "<certificate path>"
    $PrivateConfig = '{"username":"' + $UserName + '", "ssh_key":"' + $cert + '"}'
    $ExtensionName = "VMAccessForLinux"
    $Publisher = "Microsoft.OSTCExtensions"
    $Version =  "1.*"
    Set-AzureVMExtension -ExtensionName $ExtensionName -VM  $vm -Publisher $Publisher -Version $Version -PrivateConfiguration $PrivateConfig | Update-AzureVM

### <a name="both"></a>パスワードと SSH キーをリセットします。

Linux の現在のユーザー名、新しいパスワード、および SSH キーを含む証明書へのパスを入力し、これらのコマンドを実行します。

    $UserName = "<current Linux user name>"
    $Password = "<new password>"
    $Cert = Get-Content "<certificate path>"
    $PrivateConfig = '{"username":"' + $UserName + '", "password": "' +  $Password + '", "ssh_key":"' + $cert + '"}'
    $ExtensionName = "VMAccessForLinux"
    $Publisher = "Microsoft.OSTCExtensions"
    $Version =  "1.*"
    Set-AzureVMExtension -ExtensionName $ExtensionName -VM  $vm -Publisher $Publisher -Version $Version -PrivateConfiguration $PrivateConfig | Update-AzureVM

### <a name="config"></a>SSH 構成をリセットします。

SSH の構成にエラーがあると、仮想マシンにアクセスできない場合があります。 SSH の構成を既定の状態にリセットして修正することができます。 これにより、ユーザー名、パスワード、SSH キーなど、構成ですべての新しいアクセス パラメーターが削除されますが、ユーザー アカウントのパスワードまたは SSH キーは変わりません。 拡張機能によって SSH サーバーが再起動し、仮想マシン上の SSH ポートが開いて、SSH 構成が既定値にリセットされます。

これらのコマンドを実行します。

    $PrivateConfig = '{"reset_ssh": "True"}'
    $ExtensionName = "VMAccessForLinux"
    $Publisher = "Microsoft.OSTCExtensions"
    $Version = "1.*"
    Set-AzureVMExtension -ExtensionName $ExtensionName -VM  $vm -Publisher $Publisher -Version $Version -PrivateConfiguration $PrivateConfig | Update-AzureVM

> [AZURE.NOTE] SSH の構成ファイルは、/etc/ssh/sshd_config にあります。

### <a name="delete"></a> ユーザーを削除します。

削除するには、Linux ユーザー名を入力し、次のコマンドを実行します。

    $UserName = "<Linux user name to delete>"
    $PrivateConfig = "{"remove_user": "' + $UserName + '"}"
    $ExtensionName = "VMAccessForLinux"
    $Publisher = "Microsoft.OSTCExtensions"
    $Version = "1.*"
    Set-AzureVMExtension -ExtensionName $ExtensionName -VM $vm -Publisher $Publisher -Version $Version -PrivateConfiguration $PrivateConfig | Update-AzureVM

### <a name="status"></a>VMAccess 拡張機能の状態を表示します。

VMAccess 拡張機能の状態を表示するには、次のコマンドを実行します。

    $vm.GuestAgentStatus

## その他のリソース

[Azure VM 拡張機能と機能][]

[RDP または SSH で、Azure の仮想マシンに接続します。][]




[azure linux agent user guide]: virtual-machines-linux-agent-user-guide.md 
[how to install and configure azure powershell]: ../install-configure-powershell.md 
[azure vm extensions and features]: virtual-machines-extensions-features.md 
[connect to an azure virtual machine with rdp or ssh]: http://msdn.microsoft.com/library/azure/dn535788.aspx 

