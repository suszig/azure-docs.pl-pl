<properties 
    pageTitle="Windows 上での SSH を使用した Linux 仮想マシンへの接続 | Microsoft Azure" 
description="Windows コンピューターで SSH キーを生成して使用し、Azure 上の Linux 仮想マシンに接続する方法について説明します。" 
    services="virtual-machines" 
    documentationCenter="" 
    authors="squillace" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/05/2015" 
    ms.author="rasquill"/>

#Azure 上の Windows における SSH の使用方法

> [AZURE.SELECTOR]
- [Windows](../articles/virtual-machines/virtual-machines-windows-use-ssh-key.md)
- [Linux と Mac](../articles/virtual-machines/virtual-machines-linux-use-ssh-key.md)

作成および使用する方法について説明 **ssh rsa** と **.pem** 使用した Azure 上の Linux Vm への接続に使用できる Windows 上のパブリックおよびプライベートのキー ファイルの書式を設定、 **ssh** コマンドです。 既にある場合 **.pem** 作成されたファイルを使用できますにするが接続を使用して Linux Vm を作成する **ssh**します。 その他のいくつかのコマンドを使用して、 **SSH** プロトコルとキー ファイルを安全に作業を実行する特に **scp** または [セキュリティで保護されたコピー](https://en.wikipedia.org/wiki/Secure_copy), をサポートするコンピューターとの間に安全にファイルをコピーすることができます **SSH** 接続します。 


## 必要な SSH およびキー作成プログラム

**SSH** & #8212; または [シェルをセキュリティで保護](https://en.wikipedia.org/wiki/Secure_Shell) & #8212; 暗号化された接続のプロトコルをセキュリティ保護されていない接続を介してセキュリティで保護されたログインを許可します。 これは、他の何らかの接続メカニズムを有効にするように Linux VM を構成しない限り、Azure でホストされる Linux VM の既定の接続プロトコルとなります。 Windows のユーザーもに接続して使用して Azure で Linux Vm の管理、、 **ssh** クライアント実装が Windows コンピューター、一般的に付属していない、 **ssh** クライアント、1 つを選択する必要があります。 

インストールできる一般的なクライアントは次のとおりです。

- [puTTY および puTTYgen]((http://www.chiark.greenend.org.uk/~sgtatham/putty/)
- [MobaXterm](http://mobaxterm.mobatek.net/)
- [Cygwin](https://cygwin.com/)
- [Git For Windows](https://git-for-windows.github.io/), 、環境やツールに付属します。

高度な設定場合もお試しいただく、 [の新しいポート、 **OpenSSH** Windows するツールセット](http://blogs.msdn.com/b/powershell/archive/2015/10/19/openssh-for-windows-update.aspx)します。 ただし、このコードは現在開発途中であり、運用システムで使用する前にコードベースを確認する必要があります。

> [AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## 作成する必要があるキー ファイル

Azure の基本的な SSH 設定が含まれています、 **ssh rsa** 2048 ビットのパブリックおよびプライベートのキー ペア (既定では、 **で問題** としてこれらのファイルを格納 **~/.ssh/id_rsa** と **~/.ssh/id-rsa.pub** 、既定の設定を変更しない限り) と同様に、 `.pem` から生成されたファイル、 **id \_rsa** 従来のポータルの従来のデプロイ モデルで使用するための秘密キー ファイルです。 

次にデプロイメント シナリオとそれぞれのシナリオで使用されるファイルの種類を紹介します。

1. **ssh rsa** キーに使用した展開に必要な [プレビュー ポータル](https://portal.azure.com), 配置モデルに関係なく、します。
2. .pem ファイルを使用して Vm を作成するために必要な [旧ポータル](https://manage.windowsazure.com)します。 .pem ファイルが使用する従来の展開でサポートされても、 [Azure CLI](xplat-cli-install.md)します。

> [AZURE.NOTE] 従来のデプロイ モデルでデプロイされたサービスを管理する場合は、することも作成する、 **.cer** この方法は行われませんが、ポータルにアップロードするフォーマット ファイル **ssh** またはこの記事の対象になっている Linux VM に接続します。 Linux または Mac でこれらのファイルを作成するには、次のように入力します。  

## Windows 上で ssh-keygen および openssl を入手する ##

[このセクションで](#What-SSH-and-key-creation-programs-do-you-need) を含むいくつかのユーティリティを上記の一覧、 `ssh-keygen` と `openssl` Windows 向けです。 いくつかの例を次に示します。

### msysgit を使用する ###

1.  ダウンロードして、次の場所から msysgit をインストールします [http://msysgit.github.com/。](http://msysgit.github.com/)
2.  インストールしたディレクトリ (c:\msysgit\msys.exe など) から `msys` を実行します。
3.  「`cd bin`」と入力し、`bin` ディレクトリに移動します。


### Windows 用の GitHub を使用する ###

1.  ダウンロードして、次の場所から GitHub for Windows をインストールします [http://windows.github.com/。](http://windows.github.com/)
2.  [スタート] メニューをクリックし、[すべてのプログラム]、[GitHub] の順にクリックして、Git シェルを実行します。

> [AZURE.NOTE] 実行時に、次のエラーを生じる可能性があります、 `openssl` 上記のコマンド。

            Unable to load config info from /usr/local/ssl/openssl.cnf
    <!-- -->
        The easiest way to resolve this is to set the `OPENSSL_CONF` environment variable. The process for setting this variable will vary depending on the shell that you have configured in Github:
    <!-- -->
        **Powershell:**
    <!-- -->
            $Env:OPENSSL_CONF="$Env:GITHUB_GIT\ssl\openssl.cnf"
    <!-- -->
        **CMD:**
    <!-- -->
            set OPENSSL_CONF=%GITHUB_GIT%\ssl\openssl.cnf
    <!-- -->
        **Git Bash:**
    <!-- -->
            export OPENSSL_CONF=$GITHUB_GIT/ssl/openssl.cnf
    

###Cygwin を使用する###

1.  ダウンロードして、次の場所から Cygwin をインストールします [http://cygwin.com/。](http://cygwin.com/)
2.  OpenSSL パッケージとその依存パッケージがすべてインストールされていることを確認します。
3.  `cygwin` を実行します。

## 秘密キーを作成する##

1.  前の手順のどれかを実行して、`openssl.exe` を実行できるようにします。
2.  次のコマンドを入力します。

        # openssl.exe req -x509 -nodes -days 365 -newkey rsa:2048 -keyout myPrivateKey.key -out myCert.pem

3.  画面は次のようになります。

    ![linuxwelcomegit](./media/virtual-machines-linux-use-ssh-key/linuxwelcomegit.png)

4.  画面に表示されるメッセージに従って、設定します。
5.  `myPrivateKey.key` および `myCert.pem` の 2 ファイルが作成されます。
6.  API を直接使用し、管理ポータルを使用しない場合は、次のコマンドを使用して `myCert.pem` を `myCert.cer` (DER エンコードされた X509 証明書) に変換します。

        # openssl.exe  x509 -outform der -in myCert.pem -out myCert.cer

## Putty 用の PPK を作成する ##

1. ダウンロードして、次の場所から Puttygen をインストールします [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html。](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

2. Puttygen は、以前作成された秘密キー (`myPrivateKey.key`) を読み取ることができない可能性があります。 それを Puttygen で認識できる RSA 秘密キーに変換するには、次のコマンドを使用してください。

        # openssl rsa -in ./myPrivateKey.key -out myPrivateKey_rsa
        # chmod 600 ./myPrivateKey_rsa

    このコマンドにより、myPrivateKey_rsa という秘密キーが新たに生成されます。

3. `puttygen.exe` を実行します。

4. [File] の [Load a Private Key] をクリックします。

5. 先ほど生成した `myPrivateKey_rsa` という名前の秘密キー ファイルを見つけます。 ファイル フィルターを変更する必要があります **のすべてのファイル (\ * します \ *)。**

6. クリックして **開く**します。 次のような画面が表示されます。

    ![linuxgoodforeignkey](./media/virtual-machines-linux-use-ssh-key/linuxgoodforeignkey.png)

7. クリックして **OK**

8. クリックして **[Save Private Key**, 、次の図でハイライト表示されています。

    ![linuxputtyprivatekey](./media/virtual-machines-linux-use-ssh-key/linuxputtygenprivatekey.png)

9. ファイルを PPK として保存します。


## Putty を使用して Linux 仮想マシンに接続する ##

1.  ダウンロードして、次の場所から putty をインストールします [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html。](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
2.  putty.exe を実行します。
3.  ホスト名として、管理ポータルの IP を入力します。

    ![linuxputtyconfig](./media/virtual-machines-linux-use-ssh-key/linuxputtyconfig.png)

4.  選択する前に **開く**, 、[接続] をクリックして > SSH > キーを選択する認証] タブをクリックします。 入力するフィールドについては、下図を参照してください。

    ![linuxputtyprivatekey](./media/virtual-machines-linux-use-ssh-key/linuxputtyprivatekey.png)

5.  クリックして **開く** 仮想マシンに接続するには
 
