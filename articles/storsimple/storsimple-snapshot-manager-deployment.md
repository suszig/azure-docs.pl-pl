<properties 
   pageTitle="StorSimple Snapshot Manager のデプロイ | Microsoft Azure"
   description="StorSimple Snapshot Manager、MMC スナップインをダウンロードしてインストールし、StorSimple データ保護およびバックアップ機能を管理する方法を説明します。"
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carolz"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="12/01/2015"
   ms.author="v-sharos" />


# StorSimple Snapshot Manager MMC スナップインのデプロイ

## 概要

StorSimple Snapshot Manager は、Microsoft Azure StorSimple 環境でのデータ保護とバックアップ管理を簡略化する Microsoft 管理コンソール (MMC) スナップインです。 StorSimple Snapshot Manager を使用すれば、Microsoft Azure StorSimple をオンプレミスで管理し、完全に統合されたストレージ システムであるかのようにクラウド ストレージを管理できるため、バックアップと復元処理が簡略化され、コストを削減できます。

このチュートリアルでは構成要件に加え、StorSimple Snapshot Manager のインストール、削除、およびアップグレード手順についても説明します。

## StorSimple Snapshot Manager のインストール

StorSimple Snapshot Manager は、Windows Server® 2008 R2 SP1、Windows Server 2012、または Windows Server 2012 R2 オペレーティング システムを実行しているコンピューターにインストールできます。
>[AZURE.NOTE] Windows 2008 R2 を実行しているサーバーでは、Windows Server 2008 SP1 と Windows Management Framework 3.0 をインストールする必要もあります。 

Microsoft 管理コンソール (MMC) 用に StorSimple Snapshot Manager スナップインをインストールまたはアップグレードする前に、Microsoft Azure StorSimple デバイスとホスト サーバーが正しく構成されていることを確認してください。

## 構成の前提条件

次の手順では、StorSimple Snapshot Manager をインストールする前に完了する必要がある構成タスクの概要を説明します。 Microsoft Azure StorSimple の構成を完了し、セットアップについては、システム要件と手順についてなどを参照してください。 [オンプレミス StorSimple デバイスのデプロイ](storsimple-deployment-walkthrough.md)します。
>[AZURE.IMPORTANT] 作業を開始する前に確認して、 [展開構成のチェックリスト](storsimple-deployment-walkthrough.md#deployment-configuration-checklist) と
> [展開の前提条件](storsimple-deployment-walkthrough.md#deployment-prerequisites) で [オンプレミス StorSimple デバイスのデプロイ](storsimple-deployment-walkthrough.md)します。
<br>

### StorSimple Snapshot Manager をインストールする前に

1. 開梱、マウント、および」の説明に従って、Microsoft Azure StorSimple デバイスを接続 [StorSimple 8100 デバイスの取り付け](storsimple-8100-hardware-installation.md) または [StorSimple 8600 デバイスの取り付け](storsimple-8600-hardware-installation.md)します。

2. ホスト コンピューターで、次のいずれかのオペレーティング システムが実行されていることを確認します。

    - Windows Server 2008 R2 (Windows 2008 R2 を実行しているサーバーでは、Windows Server 2008 SP1 および Windows Management Framework 3.0 もインストールする必要があります)
    - Windows Server 2012
    - Windows Server 2012 R2
    >[AZURE.NOTE] StorSimple 仮想デバイスの場合、ホストは Microsoft Azure Virtual Machine である必要があります。 

3. すべての Microsoft Azure StorSimple 構成要件を満たしていることを確認します。 詳細についてを参照してください [展開の前提条件](storsimple-deployment-walkthrough.md#deployment-prerequisites)します。

4. ホストにデバイスを接続し、初期構成を実行します。 詳細についてを参照してください [配置手順](storsimple-deployment-walkthrough.md#deployment-steps)します。

5. デバイスにボリュームを作成し、それらをホストに割り当て、ホストがボリュームをマウントし、使用できることを確認します。 StorSimple Snapshot Manager では、次のタイプのボリュームがサポートされています。

    - 基本ボリューム
    - シンプル ボリューム
    - 動的ボリューム
    - ミラー化された動的ボリューム (RAID 1)
    - クラスターの共有ボリューム

    StorSimple デバイスまたは StorSimple 仮想デバイスにボリュームを作成する方法の詳細についてを参照してください [手順 6: ボリュームを作成する](storsimple-deployment-walkthrough.md#step-6-create-a-volume), で、 [オンプレミス StorSimple デバイスのデプロイ](storsimple-deployment-walkthrough.md)します。

## StorSimple Snapshot Manager の新規インストール

StorSimple Snapshot Manager をインストールする前に、StorSimple デバイスまたは StorSimple 仮想デバイスに作成されたボリュームがマウント、初期化、および」の説明に従って書式設定することを確認してください [の前提条件構成](#configure-prerequisites)します。
>[AZURE.IMPORTANT]
>
>StorSimple 仮想デバイスでのホストは、Microsoft Azure の仮想マシンをする必要があります。 
>
>-ホストでは、Windows 2008 R2、Windows Server 2012、または Windows Server 2012 R2 が実行されている必要があります。 サーバーが Windows Server 2008 R2 を実行している場合、Windows Server 2008 SP1 と Windows Management Framework 3.0 もインストールする必要があります。
>
>に StorSimple Snapshot Manager をデバイスに接続する前に、ホストから StorSimple デバイスに iSCSI 接続を構成する必要があります。

次の手順に従って、StorSimple Snapshot Manager のフレッシュ インストールを実行します。 アップグレードをインストールする場合は、「 [アップグレード StorSimple Snapshot Manager を再インストールまたは](#upgrade-or-reinstall-storsimple-snapshot-manager)します。

- 手順 1: StorSimple Snapshot Manager をインストールします
- 手順 2: StorSimple Snapshot Manager をデバイスに接続します
- 手順 3: デバイスへの接続を確認します

### 手順 1: StorSimple Snapshot Manager をインストールします

StorSimple Snapshot Manager をインストールするには、以下の手順を実行します。

#### StorSimple Snapshot Manager をインストールするには

1. StorSimple Snapshot Manager ソフトウェアのダウンロード (に [StorSimple Snapshot Manager](https://www.microsoft.com/download/details.aspx?id=44220) 、Microsoft ダウンロード センターで)、ホスト上のローカルに保存します。

2. エクスプローラーで、圧縮ファイルを右クリックし、**[すべて展開する]** をクリックします。

3. **[圧縮 (ZIP 形式) フォルダーの展開]** ウィンドウの **[展開先の選択とファイルの展開]** ボックスに、ファイルの展開先にするパスを入力するか、参照して指定します。

       >[AZURE.IMPORTANT] You must install StorSimple Snapshot Manager on the C: drive.

4. **[完了時に展開されたファイルを表示する]** チェック ボックスを選択し、**[抽出]** をクリックします。

    ![ファイル展開ダイアログ ボックス](./media/storsimple-snapshot-manager-deployment/HCS_SSM_extract_files.png)

4. 展開が完了したら、展開先フォルダーが開きます。 展開先フォルダーに表示されるアプリケーションのセットアップ アイコンをダブルクリックします。

5. **[セットアップ完了]** メッセージが表示されたら、**[閉じる]** をクリックします。 デスクトップに [StorSimple Snapshot Manager] アイコンが表示されます。

    ![デスクトップ アイコン](./media/storsimple-snapshot-manager-deployment/HCS_SSM_desktop_icon.png)

### 手順 2: StorSimple Snapshot Manager をデバイスに接続します

次の手順に従って、StorSimple Snapshot Manager を StorSimple デバイスに接続します。

#### StorSimple Snapshot Manager をデバイスに接続するには

1. デスクトップの [StorSimple Snapshot Manager] アイコンをクリックします。 [StorSimple Snapshot Manager] ウィンドウが表示されます。 このウィンドウには、**[スコープ]** ウィンドウ、**[結果]** ウィンドウ、および **[操作]** ウィンドウがあります。

    ![「StorSimple Snapshot Manager user interface (StorSimple Snapshot Manager のユーザー インターフェイス)」](./media/storsimple-snapshot-manager-deployment/HCS_SSM_gui_panes.png)

    - **[スコープ]** ウィンドウ (左ウィンドウ) には、ノードのリストがツリー構造で編成されて表示されます。 一部のノードは展開して、そのノードに関連するビューまたは特定のデータを選択できます。 ノードを展開または折りたたむには、矢印アイコンをクリックします。 **[スコープ]** ウィンドウの項目を右クリックすると、その項目に使用可能な操作のリストが表示されます。

    - **[結果]** ウィンドウ (中央ウィンドウ) には、**[スコープ]** ウィンドウで選択したノード、ビュー、またはデータに関する詳細な状態情報が表示されます。

    - **[操作]** ウィンドウには、**[スコープ]** ウィンドウで選択したノード、ビュー、またはデータに実行できる操作がリストされます。

    StorSimple Snapshot Manager ユーザー インターフェイスの詳細については、次を参照してください。 [StorSimple Snapshot Manager ユーザー インターフェイス](storsimple-use-snapshot-manager.md)します。

2. **[スコープ]** ウィンドウで **[デバイス]** ノードを右クリックし、**[デバイスの構成]** をクリックします。 **[デバイスの構成]** ダイアログ ボックスが表示されます。

    ![デバイスの構成](./media/storsimple-snapshot-manager-deployment/HCS_SSM_config_device.png)

3. **[デバイス]** ボックスの一覧で、Microsoft Azure StorSimple デバイスまたは仮想デバイスの IP アドレスを選択します。 **[パスワード]** テキスト ボックスに、Azure クラシック ポータルでデバイス用に作成した StorSimple Snapshot Manager パスワードを入力します。 **[OK]** をクリックします。

4. StorSimple Snapshot Manager により、指定したデバイスが検索されます。 デバイスを使用できる場合は、StorSimple Snapshot Manager によって接続が追加されます。 実行できます [デバイスへの接続を確認](#to-verify-the-connection) 、接続が正常に追加されたことを確認します。

    何かの理由でデバイスを使用できない場合は、StorSimple Snapshot Manager によってエラー メッセージが返されます。 **[OK]** をクリックしてエラー メッセージを閉じ、**[キャンセル]** をクリックして **[デバイスの構成]** ダイアログ ボックスを閉じます。

5. ボリューム グループに関連するバックアップがある場合、デバイスに接続されると、StorSimple Snapshot Manager は、そのデバイス用に構成された各ボリューム グループをインポートします。 関連付けられているバックアップがないボリューム グループはインポートされません。 また、ボリューム グループに対して作成されたバックアップ ポリシーはインポートされません。 インポートされたグループを表示するには、**[スコープ]** ウィンドウの最上部にある **[ボリューム グループ]** ノードを右クリックし、**[インポートされたグループを切り替え]** をクリックします。

### 手順 3: デバイスへの接続を確認します

次の手順に従って、StorSimple Snapshot Manager が StorSimple デバイスに接続されていることを確認します。

#### 接続を確認するには

1. **[スコープ]** ウィンドウで、**[デバイス]** ノードをクリックします。

    ![StorSimple Snapshot Manager デバイスの状態](./media/storsimple-snapshot-manager-deployment/HCS_SSM_Device_status.png)

2. 次を参考にして、**[結果]** ウィンドウを確認します。

   - デバイス アイコンに緑色のインジケーターが表示され、**[状態]** 列に **[使用可能]** が表示されている場合、デバイスは接続されています。

   - デバイス アイコンに赤色のインジケーターが表示され、**[状態]** 列に [使用不可] が表示されている場合、デバイスは接続されていません。

   - **[状態]** 列に **[更新しています]** が表示されている場合、StorSimple Snapshot Manager は、接続されたデバイスのボリューム グループと関連するバックアップを取得中です。

## StorSimple Snapshot Manager のアップグレードまたは再インストール

StorSimple Snapshot Manager をアップグレードまたは再インストールするには、このソフトウェアを完全にアンインストールする必要があります。

StorSimple Snapshot Manager を再インストールする前に、ホスト コンピューターで既存の StorSimple Snapshot Manager データベースをバックアップします。 これにより、バックアップ ポリシーと構成情報が保存されるため、バックアップからこのデータを簡単に復元できます。

StorSimple Snapshot Manager をアップグレードまたは再インストールするには、以下の手順を実行します。

- 手順 1: StorSimple Snapshot Manager をアンインストールします
- 手順 2: StorSimple Snapshot Manager データベースをバックアップします
- 手順 3: StorSimple Snapshot Manager を再インストールし、データベースを復元します

### 手順 1: StorSimple Snapshot Manager をアンインストールします

StorSimple Snapshot Manager をアンインストールするには、以下の手順を実行します。

#### StorSimple Snapshot Manager をアンインストールするには

1. ホスト コンピューターで、**[コントロール パネル]** を開き、 **[プログラム]** をクリックしてから、**[プログラムと機能]** をクリックします。

2. 左側のウィンドウで、**[プログラムのアンインストールまたは変更]** をクリックします。

3. **[StorSimple Snapshot Manager]** を右クリックし、**[アンインストール]** をクリックします。

4. これにより、StorSimple Snapshot Manager のセットアップ プログラムが開始します。 **[セットアップの変更]** をクリックし、**[アンインストール]** をクリックします。
    >[AZURE.NOTE] StorSimple Snapshot Manager や ディスクの管理など、バック グラウンドで実行されている MMC プロセスがある場合、アンインストールは失敗し、プログラムのアンインストールを試行する前に、MMC のすべてのインスタンスを閉じるように求めるメッセージが表示されます。 **[セットアップの完了後、アプリケーションを自動的に終了して、再起動する]** を選択し、**[OK]** をクリックします。

5. アンインストール プロセスが完了したら、**[セットアップ完了]** メッセージが表示されます。 **[閉じる]** をクリックします。

### 手順 2: StorSimple Snapshot Manager データベースをバックアップします

StorSimple Snapshot Manager データベースのコピーを作成して保存するには、以下の手順を実行します。

#### データベースをバックアップするには

1. Microsoft StorSimple Management Service を停止します。

   1. Server Manager を起動します。

   2. Server Manager ダッシュボードの **[ツール]** メニューで、**[サービス]** を選択します。

   3. **[サービス]** ページで、**[Microsoft StorSimple Management Service]** を選択します。

   4. 右側のウィンドウで、**[Microsoft StorSimple Management Service]** の下にある **[サービスの停止]** をクリックします。

        ![StorSimple Manager サービスを停止します](./media/storsimple-snapshot-manager-deployment/HCS_SSM_stop_service.png)

2. C:\ProgramData\Microsoft\StorSimple\BACatalog を参照します。
    >[AZURE.NOTE] ProgramData は隠しフォルダーです。

3. カタログ XML ファイルを検索し、ファイルをコピーして、安全な場所、またはクラウドにコピーを保存します。

    ![StorSimple バックアップ カタログ ファイル](./media/storsimple-snapshot-manager-deployment/HCS_SSM_bacatalog.png)

4. Microsoft StorSimple Management Service を再起動します。

    1. Server Manager ダッシュボードの **[ツール]** メニューで、**[サービス]** を選択します。

    2. **[サービス]** ページで、**[Microsoft StorSimple Management Service]** を選択します。

    3. 右側のウィンドウで、**[Microsoft StorSimple Management Service]** の下にある **[サービスを再起動する]** をクリックします。

### 手順 3: StorSimple Snapshot Manager を再インストールし、データベースを復元します

StorSimple Snapshot Manager を再インストールする」の手順に従います [StorSimple Snapshot Manager の新しいインストール](#install-a-new-storsimple-snapshot-manager)します。 次に、以下の手順に従って、StorSimple Snapshot Manager データベースを復元します。

#### データベースを復元するには

1. Microsoft StorSimple Management Service を停止します。

    1. Server Manager を起動します。

    2. Server Manager ダッシュボードの **[ツール]** メニューで、**[サービス]** を選択します。

    3. **[サービス]** ページで、**[Microsoft StorSimple Management Service]** を選択します。

    4. 右側のウィンドウで、**[Microsoft StorSimple Management Service]** の下にある **[サービスの停止]** をクリックします。

2. C:\ProgramData\Microsoft\StorSimple\BACatalog を参照します。
     >[AZURE.NOTE] ProgramData は隠しフォルダーです。

3. カタログ XML ファイルを削除し、以前に保存したバージョンで置き換えます。

4. Microsoft StorSimple Management Service を再起動します。

    1. Server Manager ダッシュボードの **[ツール]** メニューで、**[サービス]** を選択します。

    2. **[サービス]** ページで、**[Microsoft StorSimple Management Service]** を選択します。

    3. 右側のウィンドウで、**[Microsoft StorSimple Management Service]** の下にある **[サービスを再起動する]** をクリックします。

## 次のステップ

- に StorSimple Snapshot Manager の詳細については、 [StorSimple Snapshot Manager は何ですか?](storsimple-what-is-snapshot-manager.md).

- StorSimple Snapshot Manager ユーザー インターフェイスに関する詳細についてをするには [StorSimple Snapshot Manager ユーザー インターフェイス](storsimple-use-snapshot-manager.md)します。

- StorSimple Snapshot Manager の使用に関する詳細についてをするには [StorSimple Snapshot Manager を使用した StorSimple ソリューションの管理に](storsimple-snapshot-manager-admin.md)します。




