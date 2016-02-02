ディスクに関する詳細については、次を参照してください。 [はディスクとバーチャル マシンの Vhd](../articles/virtual-machines-disks-vhds.md)します。

<a id="attachempty"></a>
## 方法: 空のディスクの接続

空のディスクを接続すると、Azure が .vhd ファイルを作成してそれをストレージ アカウントに保存するため、データ ディスクを比較的簡単に追加できます。

1.  Mac、Linux、および Windows の Azure CLI を開き、Azure サブスクリプションに接続します。 参照してください [Connect
    Azure CLI から azure] (../articles/xplat-cli-connect.md) の詳細。

2.  」と入力して、既定である Azure サービス管理モードであるかどうかを確認 ' azure config
    モード asm' です。

3.  コマンドを使用して `azure vm ディスクを接続-新しい` を作成し、次のように、新しいディスクを接続します。 なお、
    _ubuntuVMasm_ 、サブスクリプションで作成した Linux 仮想マシンの名前に置き換えられます。 この例では、30 は GB 単位でのディスクのサイズです。

        azure vm disk attach-new ubuntuVMasm 30

4.  出力に表示されているが、データ ディスクが作成されて接続されると、' azure vm ディスクの一覧
    <virtual-machine-name>' 次のようにします。

        $ azure vm disk list ubuntuVMasm
        info:    Executing command vm disk list
        + Fetching disk images
        + Getting virtual machines
        + Getting VM disks
        data:    Lun  Size(GB)  Blob-Name                         OS
        data:    ---  --------  --------------------------------  -----
        data:         30        ubuntuVMasm-2645b8030676c8f8.vhd  Linux
        data:    0    30        ubuntuVMasm-76f7ee1ef0f6dddc.vhd
        info:    vm disk list command OK


<a id="attachexisting"></a>
## 方法: 既存のディスクの接続

既存のディスクを接続する場合は、ストレージ アカウントで利用できる .vhd を持っている必要があります。

1.  Mac、Linux、および Windows の Azure CLI を開き、Azure サブスクリプションに接続します。 参照してください [Connect
    Azure CLI から azure] (../articles/xplat-cli-connect.md) の詳細。

2.  既定の Azure サービス管理モードであることを確認します。 変更した場合
    」と入力して、リソース管理モードを単に元に戻す `azure config モード asm`します。

3.  接続する VHD が Azure サブスクリプションに既にアップロードされているかどうか、以下を使用して確認します。

        $azure vm disk list
        info:    Executing command vm disk list
        + Fetching disk images
        data:    Name                                          OS
        data:    --------------------------------------------  -----
        data:    myTestVhd                                     Linux
        data:    ubuntuVMasm-ubuntuVMasm-0-201508060029150744  Linux
        data:    ubuntuVMasm-ubuntuVMasm-0-201508060040530369
        info:    vm disk list command OK

4.  使用するディスクがない場合は、可能性があります VHD をアップロードするローカル サブスクリプションを使用して
    `azure vm ディスクを作成` または `azure vm ディスクのアップロード`します。 次に例を示します。

        $azure vm disk create myTestVhd2 .\TempDisk\test.VHD -l "East US" -o Linux
        info:    Executing command vm disk create
        + Retrieving storage accounts
        info:    VHD size : 10 GB
        info:    Uploading 10485760.5 KB
        Requested:100.0% Completed:100.0% Running:   0 Time:   25s Speed:    82 KB/s
        info:    Finishing computing MD5 hash, 16% is complete.
        info:    https://portalvhdsq1s6mc7mqf4gn.blob.core.windows.net/disks/test.VHD was
        uploaded successfully
        info:    vm disk create command OK

    使用することも、 `azure vm ディスクのアップロード` コマンドを特定のストレージ アカウントに VHD をアップロードします。 詳細については、Azure の仮想マシン データ ディスクの管理用コマンド [ここ](../virtual-machines-command-line-tools.md#commands-to-manage-your-azure-virtual-machine-data-disks)します。

5.  次のコマンドを入力して、アップロードした VHD を仮想マシンに接続します。

        $azure vm disk attach ubuntuVMasm myTestVhd
        info:    Executing command vm disk attach
        + Getting virtual machines
        + Adding Data-Disk
        info:    vm disk attach command OK

    忘れずに、_ubuntuVMasm_ は仮想マシンの名前に、_myTestVhd_ は目的の VHD に置き換えてください。

6.  コマンドを使用して仮想マシンにディスクが接続されているかどうかを確認する ' azure vm ディスクの一覧
    <virtual-machine-name>' として。

        $azure vm disk list ubuntuVMasm
        info:    Executing command vm disk list
        + Fetching disk images
        + Getting virtual machines
        + Getting VM disks
        data:    Lun  Size(GB)  Blob-Name                         OS
        data:    ---  --------  --------------------------------  -----
        data:         30        ubuntuVMasm-2645b8030676c8f8.vhd  Linux
        data:    1    10        test.VHD
        data:    0    30        ubuntuVMasm-76f7ee1ef0f6dddc.vhd
        info:    vm disk list command OK



> [AZURE.NOTE]
> データ ディスクを追加した後、仮想マシンがストレージとしてそのディスクを使用できるように、仮想マシンにログオンしてディスクを初期化する必要があります。





