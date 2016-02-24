1. Azure の [管理ポータル](http://manage.windowsazure.com), 、] をクリックして **仮想マシン** し、先ほど作成した仮想マシンを選択し、(**testlinuxvm**)。

2. コマンド バーでをクリックして **アタッチ** ] をクリックし、 **[空のディスク**します。

     **[空のディスク** ] ダイアログ ボックスが表示されます。


3.  **仮想マシン名**, 、**記憶域の場所**, 、および **ファイル名** は既に定義されています。 必要なのは、ディスクのサイズを入力することだけです。 型 **5** で、 **サイズ** フィールドです。

    ![空のディスクの接続][Image2]

    **注:** すべてのディスクは Azure ストレージに .vhd ファイルから作成されます。 ストレージに追加する .vhd ファイルの名前は指定できますが、ディスクの名前は Azure によって自動的に生成されます。

4. チェック マークをクリックして、仮想マシンにデータ ディスクを接続します。

5. 仮想マシンの名前をクリックしてダッシュボードを表示します。こうすることでデータ ディスクが仮想マシンに正しく接続されたか確認することができます。 接続したディスクが表示されて、 **ディスク** テーブルです。

    データ ディスクは接続しただけでは使用できません。使用するには、ログインしてセットアップを完了する必要があります。

##SSH または PuTTY を使用して仮想マシンに接続し、セットアップを完了する
仮想マシンにログオンし、ディスクのセットアップを完了して、ディスクにデータを格納できるようにします。

1. 仮想マシンをプロビジョニングすると、SSH または PuTTY およびログインを使用して接続 **newuser** (ように前の手順で説明します)。 


2. SSH または PuTTY のウィンドウで次のコマンドを入力し、アカウントのパスワードを入力します。

    `$ sudo grep SCSI /var/log/messages`

    表示されたメッセージに追加された最後のデータ ディスクの識別子を見つけることができます (**sdc**, 、この例では)。

    ![GREP][Image4]


3. SSH または PuTTY のウィンドウで、[ディスクのパーティションを次のコマンドを入力 **/デベロッパー/sdc**:

    `$ sudo fdisk /dev/sdc`


4. 入力 **n** 新しいパーティションを作成します。

    ![FDISK][Image5]


5. 型 **p** するには、パーティションをプライマリ パーティションにするには、入力 **1** 最初のパーティションと、シリンダーの既定値 (1) を使用する入力を入力します。

    ![FDISK][Image6]


6. 型 **p** がパーティション分割されたディスクに関する詳細が表示されます。

    ![FDISK][Image7]


7. 型 **w** 、ディスクの設定を記述します。

    ![FDISK][Image8]


8. 新しいディスクを使用して、書式設定、 **mkfs** コマンド。

    `$ sudo mkfs -t ext4 /dev/sdc1`

9. 次に、新しいファイル システムをマウントするために利用可能なディレクトリが必要です。 たとえば、次のコマンドを入力してドライブをマウントする新しいディレクトリを作成し、アカウントのパスワードを入力します。

    `sudo mkdir /datadrive`


10. 次のコマンドを入力してドライブをマウントします。

    `sudo mount /dev/sdc1 /datadrive`

    データ ディスクとしてを使用する準備ができました **/datadrive**します。


11. 新しいドライブを /etc/fstab に追加します。

    再起動後にドライブを自動的に再マウントするために、そのドライブを /etc/fstab ファイルに追加する必要があります。 また、ドライブを参照する際に、デバイス名 (つまり /dev/sdc1) だけではなく、UUID (汎用一意識別子) を /etc/fstab で使用することもお勧めします。 使用する新しいドライブの UUID を確認する、 **blkid** ユーティリティ。
    
        `sudo -i blkid`

    次のような出力が表示されます。

        `/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"`
        `/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"`
        `/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"`

    >[AZURE.NOTE] blkid では、すべての場合に sudo アクセスは必要ありませんが、ただし、これを実行しやすくあります `sudo -i` /sbin または/usr/sbin がにない場合、一部のディストリビューションで、 `$PATH`です。

    **注意:** /etc/fstab ファイルを不適切に編集システムが起動できなくなる可能性があります。 編集方法がはっきりわからない場合は、このファイルを適切に編集する方法について、ディストリビューションのドキュメントを参照してください。 編集する前に、/etc/fstab ファイルのバックアップを作成することもお勧めします。

    テキスト エディターを使用して、/etc/fstab ファイルの最後の部分に新しいファイル システムに関する情報を入力します。  新しいに対して UUID 値を使用してこの例では **/dev/sdc1** 前の手順で、マウント ポイントとして作成したデバイス **/datadrive**:

        `UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults   1   2`

    追加のデータ ドライブやパーティションを作成する場合、それらも /etc/fstab に個別に入力する必要があります。

    テストするだけのマウントを解除して再マウントし、ファイル システム、つまり、ファイル システムが正しくマウントされるようになりました 例を使用してマウント ポイント `/datadrive` 前の手順で作成します。 

        `sudo umount /datadrive`
        `sudo mount /datadrive`

    2 番目のコマンドでエラーが発生した場合、/etc/fstab ファイルの構文が正しいかどうかを確認してください。


    >[AZURE.NOTE] Subsequently removing a data disk without editing fstab could cause the VM to fail to boot. If this is a common occurrence, then most distributions provide either the `nofail` and/or `nobootwait` fstab options that will allow a system to boot even if the disk is not present. Please consult your distribution's documentation for more information on these parameters.


[Image2]: ./media/attach-data-disk-centos-vm-in-portal/AttachDataDiskLinuxVM2.png
[Image4]: ./media/attach-data-disk-centos-vm-in-portal/GrepScsiMessages.png
[Image5]: ./media/attach-data-disk-centos-vm-in-portal/fdisk1.png
[Image6]: ./media/attach-data-disk-centos-vm-in-portal/fdisk2.png
[Image7]: ./media/attach-data-disk-centos-vm-in-portal/fdisk3.png
[Image8]: ./media/attach-data-disk-centos-vm-in-portal/fdisk4.png
[Image9]: ./media/attach-data-disk-centos-vm-in-portal/mkfs.png


