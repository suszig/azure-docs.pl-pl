<properties
    pageTitle="Azure での Linux VM へのログオン | Microsoft Azure"
    description="Linux を実行する Azure の仮想マシンに Secure Shell (SSH) クライアントを使用してログオンする方法について説明します。"
    services="virtual-machines"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/08/2015"
    ms.author="rasquill"/>


#Linux を実行する仮想マシンにログオンする方法 #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] [リソース マネージャーのデプロイ モデル](virtual-machines-linux-tutorial-portal-rm.md)します。

仮想マシンにログオンするときに使用するコンピューターには、SSH クライアントをインストールする必要があります。 SSH クライアント プログラムの選択肢は多数あります。 たとえば、次のプログラムを選択できます。

- Windows オペレーティング システムを実行しているコンピューターの場合、PuTTY などの SSH クライアントを使用できます。 詳細については、次を参照してください。、 [PuTTY のダウンロード ページ](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)します。
- Linux オペレーティング システムを実行する仮想マシンでは、Secure Shell (SSH) クライアントを使用してログオンします。このクライアントは、ほぼすべてのディストリビューションに既定でインストールされています。 参照してください [SSH を使用する方法](virtual-machines-linux-use-ssh-key.md) Linux についてのもう少しです。


>[AZURE.NOTE] 詳細な要件とトラブルシューティングのヒントについては、次を参照してください。 [RDP または SSH で Azure 仮想マシンに接続する](http://go.microsoft.com/fwlink/p/?LinkId=398294)です。

ここでは、PuTTY プログラムを使用して仮想マシンにアクセスする手順を示します。

1. 検索、 **ホスト名** と **ポート情報** から、 [管理ポータル](http://manage.windowsazure.com)します。 仮想マシンのダッシュボードから必要な情報を見つけることができます。 仮想マシンの名前をクリックし、検索、 **SSH の詳細** で、 **概要** ダッシュ ボードのセクションです。

    ![SSH の詳細の取得](./media/virtual-machines-linux-how-to-log-on/sshdetails.png)

2. PuTTY プログラムを開きます。

3. ホスト名と、ダッシュ ボードから収集したポート情報を入力し、クリックして **開く**します。

    ![PuTTY を開く](./media/virtual-machines-linux-how-to-log-on/putty.png)

4. マシンの作成時に指定したアカウントを使用して仮想マシンにログオンします。 ユーザー名とパスワードを持つ仮想マシンを作成する方法の詳細については、次を参照してください。 [を実行している Linux の仮想マシンを作成する](virtual-machines-linux-tutorial.md)です。

    ![仮想マシンへのログオン](./media/virtual-machines-linux-how-to-log-on/sshlogin.png)

>[AZURE.NOTE] VMAccess 拡張機能では、これを忘れてしまった場合は、SSH キーやパスワードをリセットできます。 ユーザー名を忘れた場合は、拡張機能を使用して sudo 権限で新しいユーザー名を作成できます。 手順については、次を参照してください。、 [How to Reset a Password or SSH for Linux Virtual Machines]します。

これで、仮想マシンを他のサーバーとまったく同様に扱うことができます。

<!-- LINKS -->
[How to Reset a Password or SSH for Linux Virtual Machines]: http://go.microsoft.com/fwlink/p/?LinkId=512138


