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

>[AZURE.NOTE] Windows 2008 R2 を実行しているサーバーでは、Windows Server 2008 SP1 と Windows Management Framework 3.0 をインストールする必要があります。 

Microsoft 管理コンソール (MMC) 用に StorSimple Snapshot Manager スナップインをインストールまたはアップグレードする前に、Microsoft Azure StorSimple デバイスとホスト サーバーが正しく構成されていることを確認してください。 

## 構成の前提条件

次の手順では、StorSimple Snapshot Manager をインストールする前に完了する必要がある構成タスクの概要を説明します。 Microsoft Azure StorSimple の構成を完了し、セットアップについては、システム要件と手順についてなどを参照してください。 [オンプレミス StorSimple デバイスのデプロイ](storsimple-deployment-walkthrough.md)します。

>[AZURE.IMPORTANT] 作業を開始する前に確認して、 [展開構成のチェックリスト](storsimple-deployment-walkthrough.md#deployment-configuration-checklist) と
>  [展開の前提条件](storsimple-deployment-walkthrough.md#deployment-prerequisites) で [オンプレミス StorSimple デバイスのデプロイ](storsimple-deployment-walkthrough.md)します。
<br>
 
### StorSimple Snapshot Manager をインストールする前に

1. 開梱、マウント、および」の説明に従って、Microsoft Azure StorSimple デバイスを接続 [StorSimple 8100 デバイスの取り付け](storsimple-8100-hardware-installation.md) または [StorSimple 8600 デバイスの取り付け](storsimple-8600-hardware-installation.md)します。

2. ホスト コンピューターで、次のいずれかのオペレーティング システムが実行されていることを確認します。

    - Windows Server 2008 R2 (Windows 2008 R2 を実行しているサーバーでは、Windows Server 2008 SP1 および Windows Management Framework 3.0 もインストールする必要があります)
    - Windows Server 2012
    - Windows Server 2012 R2
 
    >[AZURE.NOTE] StorSimple 仮想デバイスの場合、ホストは Microsoft Azure の仮想マシンである必要があります。 

3. すべての Microsoft Azure StorSimple 構成要件を満たしていることを確認します。 詳細についてを参照してください [展開の前提条件](storsimple-deployment-walkthrough.md#deployment-prerequisites)します。

4. ホストにデバイスを接続し、初期構成を実行します。 詳細についてを参照してください [配置手順](storsimple-deployment-walkthrough.md#deployment-steps)します。

5. デバイスにボリュームを作成し、それらをホストに割り当て、ホストがボリュームをマウントし、使用できることを確認します。 StorSimple Snapshot Manager では、次のタイプのボリュームがサポートされています。 

    - 基本ボリューム
    - シンプル ボリューム
    - 動的ボリューム
    - ミラー化された動的ボリューム (RAID 1)
    - クラスターの共有ボリューム
 
    StorSimple デバイスまたは StorSimple 仮想デバイスにボリュームを作成する方法の詳細についてを参照してください [手順 6: ボリュームを作成する](storsimple-deployment-walkthrough.md#step-6-create-a-volume), の [オンプレミス StorSimple デバイスのデプロイ](storsimple-deployment-walkthrough.md)します。

## StorSimple Snapshot Manager の新規インストール

StorSimple Snapshot Manager をインストールする前に、StorSimple デバイスまたは StorSimple 仮想デバイスに作成されたボリュームがマウント、初期化、および」の説明に従って書式設定することを確認してください [の前提条件構成](#configure-prerequisites)します。

>[AZURE.IMPORTANT]
>
>- StorSimple 仮想デバイスの場合、ホストは Microsoft Azure Virtual Machine である必要があります。 
>
>- ホストは、Windows 2008 R2、Windows Server 2012、または Windows Server 2012 R2 を実行している必要があります。 サーバーが Windows Server 2008 R2 を実行している場合、Windows Server 2008 SP1 と Windows Management Framework 3.0 もインストールする必要があります。
>
>- デバイスを StorSimple Snapshot Manager に接続するには、ホストから StorSimple デバイスに iSCSI 接続を構成する必要があります。

次の手順に従って、StorSimple Snapshot Manager のフレッシュ インストールを実行します。 アップグレードをインストールする場合は、「 [アップグレード StorSimple Snapshot Manager を再インストールまたは](#upgrade-or-reinstall-storsimple-snapshot-manager)です。

- 手順 1: StorSimple Snapshot Manager をインストールします 
- 手順 2: StorSimple Snapshot Manager をデバイスに接続します 
- 手順 3: デバイスへの接続を確認します 

###手順 1: StorSimple Snapshot Manager をインストールします

StorSimple Snapshot Manager をインストールするには、以下の手順を実行します。

#### StorSimple Snapshot Manager をインストールするには

1. StorSimple Snapshot Manager ソフトウェアのダウンロード (に [StorSimple Snapshot Manager](https://www.microsoft.com/download/details.aspx?id=44220) 、Microsoft ダウンロード センターで)、ホスト上のローカルに保存します。

2. エクスプ ローラーで、圧縮フォルダーを右クリックし、をクリックして **すべて抽出**します。

3.  **[圧縮 (zip 形式) フォルダーの** ] ウィンドウで、 **先を選択し、ファイルを抽出** ボックスで、入力するか、ファイルを抽出するにはパスを参照します。 

       >[AZURE.IMPORTANT] C: ドライブには、StorSimple Snapshot Manager をインストールする必要があります。
 
4. 選択、 **表示が完了すると、ファイルを抽出** チェック ボックスをオンにし **抽出**します。

    ![ファイル展開ダイアログ ボックス](./media/storsimple-snapshot-manager-deployment/HCS_SSM_extract_files.png) 

4. 展開が完了したら、展開先フォルダーが開きます。 展開先フォルダーに表示されるアプリケーションのセットアップ アイコンをダブルクリックします。

5. ときに、 **セットアップ完了** をクリックしてメッセージが表示されたら、 **閉じる**します。 デスクトップに [StorSimple Snapshot Manager] アイコンが表示されます。

    ![デスクトップ アイコン](./media/storsimple-snapshot-manager-deployment/HCS_SSM_desktop_icon.png) 

### 手順 2: StorSimple Snapshot Manager をデバイスに接続します

次の手順に従って、StorSimple Snapshot Manager を StorSimple デバイスに接続します。

#### StorSimple Snapshot Manager をデバイスに接続するには

1. デスクトップの [StorSimple Snapshot Manager] アイコンをクリックします。 [StorSimple Snapshot Manager] ウィンドウが表示されます。 ウィンドウには、 **スコープ** ] ウィンドウで、 **結果** ] ウィンドウで、および **アクション** ウィンドウです。 

    ![StorSimple Snapshot Manager のユーザー インターフェイス](./media/storsimple-snapshot-manager-deployment/HCS_SSM_gui_panes.png) 

    -  **スコープ** ウィンドウ (左側のウィンドウ) には一連のノードがツリー構造で表示します。 一部のノードは展開して、そのノードに関連するビューまたは特定のデータを選択できます。 ノードを展開または折りたたむには、矢印アイコンをクリックします。 内の項目を右クリックし、 **スコープ** その項目に対して使用可能なアクションの一覧を表示するウィンドウです。 

    -  **結果** ウィンドウ (中央のウィンドウ) には、ノード、ビュー、または [選択したデータに関する詳細なステータス情報が含まれています。、 **スコープ** ウィンドウです。

    -  **アクション** ペインには、ノード、ビュー、または [選択したデータで実行できる操作が一覧表示、 **スコープ** ウィンドウです。

    StorSimple Snapshot Manager ユーザー インターフェイスの詳細については、次を参照してください。 [StorSimple Snapshot Manager ユーザー インターフェイス](storsimple-use-snapshot-manager.md)します。

2.  **スコープ** ] ウィンドウを右クリックし、 **デバイス** ノード、およびクリック **デバイスを構成する**です。  **デバイスを構成する** ] ダイアログ ボックスが表示されます。

    ![デバイスの構成](./media/storsimple-snapshot-manager-deployment/HCS_SSM_config_device.png) 

3.  **デバイス** ボックスの一覧を Microsoft Azure StorSimple デバイスまたは仮想デバイスの IP アドレスを選択します。  **パスワード** テキスト ボックスに、Azure 旧ポータルでデバイス用に作成した StorSimple Snapshot Manager パスワードを入力します。 Click **OK**.

4. StorSimple Snapshot Manager により、指定したデバイスが検索されます。 デバイスを使用できる場合は、StorSimple Snapshot Manager によって接続が追加されます。 実行できます [デバイスへの接続を確認](#to-verify-the-connection) 、接続が正常に追加されたことを確認します。

    何かの理由でデバイスを使用できない場合は、StorSimple Snapshot Manager によってエラー メッセージが返されます。 をクリックして **[ok]** エラー メッセージを閉じる] をクリックして **キャンセル** を閉じる、 **デバイスを構成する** ] ダイアログ ボックス。

5. ボリューム グループに関連するバックアップがある場合、デバイスに接続されると、StorSimple Snapshot Manager は、そのデバイス用に構成された各ボリューム グループをインポートします。 関連付けられているバックアップがないボリューム グループはインポートされません。 また、ボリューム グループに対して作成されたバックアップ ポリシーはインポートされません。 インポートされたグループを表示するには、最上位を右クリックして **ボリューム グループ** 内のノード、 **スコープ** ペイン、およびクリック **インポートされたグループを切り替える**します。

### 手順 3: デバイスへの接続を確認します

次の手順に従って、StorSimple Snapshot Manager が StorSimple デバイスに接続されていることを確認します。

#### 接続を確認するには

1.  **スコープ** ] ウィンドウで、をクリックして、 **デバイス** ノードです。

    ![StorSimple Snapshot Manager デバイスの状態](./media/storsimple-snapshot-manager-deployment/HCS_SSM_Device_status.png) 

2. チェック、 **結果** ウィンドウ。 

   - デバイス アイコンに緑色のインジケーターが表示された場合と **利用可能な** に表示されます、 **ステータス** 列で、デバイスが接続されています。 

   - デバイス アイコンに赤色のインジケーターが表示され、利用できませんで場合、 **ステータス** ] 列で、デバイスは接続されていません。 

   - 場合 **Refreshing** に表示されます、 **ステータス** ボリューム グループおよび関連するバックアップ、接続されたデバイスを列で、StorSimple Snapshot Manager を取得しています。

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

1. ホスト コンピューターで開く、 **コントロール パネルの [**, 、] をクリックして **プログラム**, 、クリックして **プログラムと機能**です。

2. 左のウィンドウで **のアンインストールと変更プログラム**します。

3. 右クリック **StorSimple Snapshot Manager**, 、クリックして **アンインストール**します。

4. これにより、StorSimple Snapshot Manager のセットアップ プログラムが開始します。 クリックして **セットアップの変更**, 、] をクリックし、 **アンインストール**します。

    >[AZURE.NOTE] バック グラウンドで実行されている MMC プロセスがある場合は、StorSimple Snapshot Manager やディスクの管理など、アンインストールは失敗し、プログラムをアンインストールしようとする前に、MMC のすべてのインスタンスを閉じてメッセージが表示されます。 選択 **自動的にアプリケーションを終了し、セットアップの完了後に再起動する**, 、順にクリック **OK**します。
 
5. アンインストール プロセスが完了すると、 **セットアップ完了** メッセージが表示されます。 クリックして **閉じる**します。

### 手順 2: StorSimple Snapshot Manager データベースをバックアップします

StorSimple Snapshot Manager データベースのコピーを作成して保存するには、以下の手順を実行します。

#### データベースをバックアップするには

1. Microsoft StorSimple Management Service を停止します。

   1. Server Manager を起動します。

   2. サーバー マネージャー ダッシュ ボードで、 **ツール** メニューの [ **サービス**します。

   3.  **サービス** ] ページで、[ **Microsoft StorSimple Management Service**します。

   4. 右側のウィンドウで [ **Microsoft StorSimple Management Service**, 、クリックして **サービスを停止**します。

        ![StorSimple Manager サービスを停止します](./media/storsimple-snapshot-manager-deployment/HCS_SSM_stop_service.png)

2. C:\ProgramData\Microsoft\StorSimple\BACatalog を参照します。 

    >[AZURE.NOTE] ProgramData は、非表示のフォルダーです。

3. カタログ XML ファイルを検索し、ファイルをコピーして、安全な場所、またはクラウドにコピーを保存します。

    ![StorSimple バックアップ カタログ ファイル](./media/storsimple-snapshot-manager-deployment/HCS_SSM_bacatalog.png)

4. Microsoft StorSimple Management Service を再起動します。 

    1. サーバー マネージャー ダッシュ ボードで、 **ツール** メニューの [ **サービス**します。

    2.  **サービス** ] ページで、[、 **Microsoft StorSimple Management サービス**e。

    3. 右側のウィンドウで [ **Microsoft StorSimple Management Service**, をクリックして **サービスを再起動**します。 

### 手順 3: StorSimple Snapshot Manager を再インストールし、データベースを復元します

StorSimple Snapshot Manager を再インストールする」の手順に従います [StorSimple Snapshot Manager の新しいインストール](#install-a-new-storsimple-snapshot-manager)します。 次に、以下の手順に従って、StorSimple Snapshot Manager データベースを復元します。

#### データベースを復元するには

1. Microsoft StorSimple Management Service を停止します。

    1. Server Manager を起動します。

    2. サーバー マネージャー ダッシュ ボードで、 **ツール** メニューの [ **サービス**します。

    3.  **サービス** ] ページで、[ **Microsoft StorSimple Management Service**します。

    4. 右側のウィンドウで [ **Microsoft StorSimple Management Service**, 、クリックして **サービスを停止**します。

2. C:\ProgramData\Microsoft\StorSimple\BACatalog を参照します。 

     >[AZURE.NOTE] ProgramData は、非表示のフォルダーです。

3. カタログ XML ファイルを削除し、以前に保存したバージョンで置き換えます。

4. Microsoft StorSimple Management Service を再起動します。 

    1. サーバー マネージャー ダッシュ ボードで、 **ツール** メニューの [ **サービス**します。

    2.  **サービス** ] ページで、[ **Microsoft StorSimple Management Service**します。

    3. 右側のウィンドウで [ **Microsoft StorSimple Management Service**, をクリックして **サービスを再起動**します。

## 次のステップ

- StorSimple Snapshot Manager についての詳細についてに移動 [What is StorSimple Snapshot Manager?](storsimple-what-is-snapshot-manager.md)します。

- StorSimple Snapshot Manager ユーザー インターフェイスに関する詳細についてをするには [StorSimple Snapshot Manager ユーザー インターフェイス](storsimple-use-snapshot-manager.md)します。

- StorSimple Snapshot Manager の使用に関する詳細についてをするには [StorSimple Snapshot Manager を使用した StorSimple ソリューションの管理に](storsimple-snapshot-manager-admin.md)します。

