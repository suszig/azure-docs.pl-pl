
ディスクに関する詳細については、次を参照してください。 [はディスクとバーチャル マシンの Vhd](../articles/virtual-machines-disks-vhds.md)します。

##<a id="attachempty"></a>方法: 空のディスクの接続

空のディスクを接続すると、Azure が .vhd ファイルを作成してそれをストレージ アカウントに保存するため、データ ディスクを比較的簡単に追加できます。

1. クリックして **仮想マシン**, 、適切な仮想マシンを選択します。

2. コマンド バー] をクリックして **アタッチ**, 、クリックして **[空のディスク**します。


    ![Attach an empty disk](./media/howto-attach-disk-window-linux/AttachEmptyDisk.png)

3.   **空のディスクをアタッチ** ] ダイアログ ボックスが表示されます。


    ![Attach a new empty disk](./media/howto-attach-disk-window-linux/AttachEmptyDetail.png)


    Do the following:

    - In **File Name**, accept the default name or type another one for the .vhd file, which is used for the disk. The data disk uses an automatically generated name, even if you type another name for the .vhd file.

    - Type the **Size (GB)** of the data disk.

    - Click the check mark to finish.

4.  データ ディスクが作成されて接続されると、仮想マシンのダッシュボードに表示されます。

    ![Empty data disk successfully attached](./media/howto-attach-disk-window-linux/AttachEmptySuccess.png)
    
> [AZURE.NOTE]
> 新しいデータ ディスクを追加した後で、仮想マシンがストレージとしてそのディスクを使用できるように、仮想マシンにログオンしてディスクを初期化する必要があります。 

##<a id="attachexisting"></a>方法: 既存のディスクの接続

既存のディスクを接続する場合は、ストレージ アカウントで利用できる .vhd を持っている必要があります。 使用して、 [Add-azurevhd](http://go.microsoft.com/FWLink/p/?LinkID=391684) .vhd ファイルをストレージ アカウントにアップロードするコマンドレットです。 .vhd ファイルを作成してアップロードした後は、それを仮想マシンに接続できます。

1. クリックして **仮想マシン**, 、適切な仮想マシンを選択します。

2. コマンド バー] をクリックして **アタッチ**, 、し、[ **のディスクの接続**します。


    ![Attach data disk](./media/howto-attach-disk-window-linux/AttachExistingDisk.png)

    The **Attach Disk** dialog box appears.



    ![Enter data disk details](./media/howto-attach-disk-window-linux/AttachExistingDetail.png)

3. 仮想マシンに接続するデータ ディスクを選択します。

4. チェック マークをクリックして、仮想マシンにデータ ディスクを接続します。

5.  データ ディスクが接続されると、仮想マシンのダッシュボードに表示されます。


    ![Data disk successfully attached](./media/howto-attach-disk-window-linux/AttachExistingSuccess.png)



