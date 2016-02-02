1. 示された手順を使用して、Azure サブスクリプションへのサインイン [Azure CLI から Azure への接続](../articles/xplat-cli-connect.md)します。

2. 以下を使用してサービス管理モードであることを確認します。

        azure config mode asm

3. 利用可能なイメージからロードする Linux イメージを探します。

        azure vm image list | grep "Linux"

   Windows コマンド プロンプト ウィンドウでは、grep ではなく find を使用します。

4. 使用 `azure 仮想マシンの作成` を上記のリストから Linux イメージに新しいバーチャル マシンを作成します。 この手順では、新しいクラウド サービスだけでなく新しいストレージ アカウントも作成されます。 既存のクラウド サービスにこのバーチャル マシンを接続することもでした、 `-c` オプション。 Linux 仮想マシンにログインするための SSH エンドポイントも作成、 `-e` オプション。

        ~$ azure vm create "MyTestVM" b4590d9e3ed742e4a1d46e5424aa335e__suse-opensuse-13.1-20141216-x86-64 "adminUser" -z "Small" -e -l "West US"
        info:    Executing command vm create
        + Looking up image b4590d9e3ed742e4a1d46e5424aa335e__suse-opensuse-13.1-20141216-x86-64
        Enter VM 'adminUser' password:*********
        Confirm password: *********
        + Looking up cloud service
        info:    cloud service MyTestVM not found.
        + Creating cloud service
        + Retrieving storage accounts
        + Creating a new storage account 'mytestvm1437604756125'
        + Creating VM
        info:    vm create command OK

    >[AZURE.NOTE] Linux 仮想マシンを指定する必要があります、 `-e` オプション `仮想マシンの作成`; バーチャル マシンを作成した後は、SSH を有効にすることはできません。 SSH の詳細については、読み取り [ssh Azure 上の Linux の使用方法](../articles/virtual-machines/virtual-machines-linux-use-ssh-key.md)します。

    上の手順ではイメージ リストからイメージ *b4590d9e3ed742e4a1d46e5424aa335e__suse-opensuse-13.1-20141216-x86-64* を選択することに注意してください。 *MyTestVM* は新しい仮想マシンの名前であり、*adminUser* は仮想マシンに SSH するために使用するユーザー名です。 これらの変数は、必要に応じて置き換えることができます。 このコマンドの詳細については、次を参照してください。、 [Azure サービス管理で Azure CLI の使用](../articles/virtual-machines/virtual-machines-command-line-tools.md)します。

5. 新しく作成された Linux 仮想マシンが、次のものによって指定される一覧に表示されます。

        azure vm list

6. 次のコマンドを使用して、仮想マシンの属性を確認できます。

        azure vm show MyTestVM

7. 新しく作成された仮想マシンが最初に準備が、 `azure vm start` コマンドです。

これらすべての Azure CLI 仮想マシン コマンドの詳細については、参照、 [サービス管理 API で Azure CLI の使用](../articles/virtual-machines/virtual-machines-command-line-tools.md)します。





