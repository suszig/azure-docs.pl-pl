<properties 
   pageTitle="StorSimple Snapshot Manager を使用したデバイス管理 | Microsoft Azure"
   description="StorSimple Snapshot Manager MMC スナップインを使用して、StorSimple デバイスを接続および管理する方法について説明します。"
   services="storsimple"
   documentationCenter=""
   authors="SharS"
   manager="carolz"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="12/02/2015"
   ms.author="v-sharos" />

# StorSimple Snapshot Manager を使用した StorSimple デバイスの接続と管理

## 概要

ノードを使用すると、StorSimple Snapshot Manager **スコープ** ペインに、インポートされた StorSimple デバイスのデータを確認し、接続されたストレージ デバイスを更新します。 さらをクリックすると、 **デバイス** ノード、接続されているデバイスと、対応するステータス情報の一覧を表示できる、 **結果** ウィンドウです。

![接続されているデバイス](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_connect_devices.png)

**図 1: StorSimple Snapshot Manager が接続されたデバイス** 

によって、 **ビュー** の選択、 **結果** ペインは、各デバイスに関する次の情報を表示します。 (表示の構成の詳細についてを参照してください [[表示] メニュー](storsimple-use-snapshot-manager.md#view-menu)します。


| 結果の列  |説明          |
|:----------------|:--------------------| 
| 名前            | Azure クラシック ポータルで構成されているデバイスの名前|
| モデル           | デバイスのモデル番号|
| バージョン         | デバイスにインストールされているソフトウェアのバージョン |
| 状態          | デバイスが使用できるかどうか |
| 最後の同期時刻     | デバイスが最後に同期された日時 |
| シリアル番号      | デバイスのシリアル番号 |
 
右クリックした場合、 **デバイス** 内のノード、 **スコープ** ] ウィンドウで、次のアクションから選択できます。

- デバイスの追加または置き換え 
- デバイスの接続、およびインポートの検証 
- 接続されたデバイスの更新 

クリックすると、 **デバイス** ノード、デバイス名を右クリック、 **結果** ] ウィンドウで、次のアクションから選択できます。

- デバイスの認証 
- デバイスの詳細の表示 
- デバイスの更新 
- デバイスの構成の削除 
- デバイスのパスワードの変更

>[AZURE.NOTE] これらの操作も使用できます、 **アクション** ウィンドウです。
 
このチュートリアルでは、StorSimple Snapshot Manager を使用してデバイスの接続および管理を行い、次のタスクを実行する方法について説明します。

- デバイスの追加または置き換え 
- デバイスの接続、およびインポートの検証 
- 接続されたデバイスの更新 
- デバイスの認証 
- デバイスの詳細の表示 
- 個々のデバイスの更新 
- デバイスの構成の削除 
- 有効期限が切れたデバイス パスワードの変更
- 障害が発生したデバイスの置き換え

>[AZURE.NOTE] 概要については、StorSimple Snapshot Manager のインターフェイスを使用して、参照してください [StorSimple Snapshot Manager ユーザー インターフェイス](storsimple-use-snapshot-manager.md)します。


## デバイスの追加または置き換え

StorSimple デバイスを追加または置き換えるには、以下の手順を実行します。

#### デバイスを追加または置き換えるには

1. デスクトップ アイコンをクリックして、StorSimple Snapshot Manager を起動します。

2.  **スコープ** ] ウィンドウを右クリックし、 **デバイス** ノード、およびクリック **デバイスを構成する**です。  **デバイスを構成する** ] ダイアログ ボックスが表示されます。

    ![StorSimple デバイスの構成](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_config_device.png) 

3.  **デバイス** ドロップダウン ボックスで、デバイスまたは仮想デバイスの IP アドレスを選択します。 

4.  **パスワード** テキスト ボックスに、Azure 旧ポータルでデバイス用に作成した StorSimple Snapshot Manager パスワードを入力します。 Click **OK**. StorSimple Snapshot Manager により、指定したデバイスが検索されます。 

    - デバイスを使用できる場合は、StorSimple Snapshot Manager によって接続が追加されます。 

    - 何かの理由でデバイスを使用できない場合は、StorSimple Snapshot Manager によってエラー メッセージが返されます。 をクリックして **[ok]** エラー メッセージを閉じる] をクリックして **キャンセル** を閉じる、 **デバイスを構成する** ] ダイアログ ボックス。

## デバイスの接続、およびインポートの検証

次の手順に従って StorSimple デバイスを接続し、関連するバックアップが含まれた既存のボリュームがインポートされていることを確認します。

#### デバイスを接続し、インポートを検証するには

1. デバイスを StorSimple Snapshot Manager に接続するには、デバイスの追加または置き換えに関する指示に従ってください。 デバイスに接続されると、StorSimple Snapshot Manager は次のように応答します。

    - 何かの理由でデバイスを使用できない場合は、StorSimple Snapshot Manager によってエラー メッセージが返されます。 

   - デバイスを使用できる場合は、StorSimple Snapshot Manager によって接続が追加されます。 デバイスを選択するときに表示されます、 **結果** ペイン、および status フィールドは、デバイスがあることを示します。 **可能**します。 StorSimple Snapshot Manager は、ボリューム グループに関連するバックアップがある場合、デバイス用に構成されたすべてのボリューム グループをインポートします。 バックアップ ポリシーはインポートされません。 関連付けられているバックアップがないボリューム グループはインポートされません。

2. デスクトップ アイコンをクリックして、StorSimple Snapshot Manager を起動します。

3. 最上位ノードを右クリックし、 **スコープ** ] ウィンドウで、クリックして **インポート表示の切り替え**します。

    ![[インポートの表示の切り替え] の選択](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Toggle_Imports_Display.png) 

4.  **インポート表示の切り替え** 、インポートされたボリューム グループとバックアップの状態を示すダイアログ ボックスが表示されます。 Click **OK**. 

ボリューム グループおよびバックアップが正常にインポートされたら、StorSimple Snapshot Manager を使用して作成および構成したボリューム グループやバックアップを管理する場合と同様に、StorSimple Snapshot Manager を使用してそれらを管理できます。 

## 接続されたデバイスの更新

接続された StorSimple デバイスを StorSimple Snapshot Manager と同期するには、以下の手順を実行します。

####接続されているデバイスを更新するには

1. デスクトップ アイコンをクリックして、StorSimple Snapshot Manager を起動します。

2.  **スコープ** ] ウィンドウを右クリックして **デバイス**, 、クリックして **デバイスの更新]**します。 これにより、接続されたデバイスと StorSimple Snapshot Manager が同期されるため、最近追加したものを含め、ボリューム グループとバックアップを表示できるようになります。 

    ![StorSimple デバイスの更新](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Refresh_devices.png)
 
 **デバイスの更新]** 操作では接続されているデバイスからすべての新しいボリューム グループと、関連付けられているバックアップが取得されます。 異なり、 **ボリュームの再スキャン** を利用できるアクション、 **ボリューム** ノード、 **デバイスの更新]** ではバックアップ レジストリが復元されません。

## デバイスの認証

次の手順に従って、StorSimple Snapshot Manager で StorSimple デバイスを認証します。

#### デバイスの認証するには

1. デスクトップ アイコンをクリックして、StorSimple Snapshot Manager を起動します。

2.  **スコープ** ] ウィンドウで、をクリックして **デバイス**します。

3.  **結果** ] ウィンドウでは、デバイスの名前を右クリックし、をクリックして **認証**します。

4.  **認証** ] ダイアログ ボックスが表示されます。 デバイスのパスワードを入力し、クリックして **OK**します。

    ![ダイアログ ボックスの認証](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Authenticate.png) 
 
## デバイスの詳細の表示

次の手順に従って、StorSimple デバイスの詳細を表示し、必要に応じてデバイスと StorSimple Snapshot Manager を再同期します。

#### デバイスの詳細を表示し、再同期するには

1. デスクトップ アイコンをクリックして、StorSimple Snapshot Manager を起動します。

2.  **スコープ** ] ウィンドウで、をクリックして **デバイス**します。

3.  **結果** ] ウィンドウでは、デバイスの名前を右クリックし、をクリックして **詳細**します。 

4. **デバイスの詳細** ] ダイアログ ボックスが表示されます。 このボックスには、名前、モデル、バージョン、シリアル番号、状態、対象の iSCSI 修飾名 (IQN)、および最終同期の日時が表示されます。 

   - クリックして **再同期** デバイスを同期します。

   - をクリックして **OK** または **キャンセル** ] ダイアログ ボックスを閉じます。

    ![Device details](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Device_details.png) 
 
## 個々のデバイスの更新

次の手順に従って、個々 の StorSimple デバイスを StorSimple Snapshot Manager と再同期します。

#### デバイスを更新するには

1. デスクトップ アイコンをクリックして、StorSimple Snapshot Manager を起動します。 

2.  **スコープ** ] ウィンドウで、をクリックして **デバイス**します。 

3.  **結果** ] ウィンドウでは、デバイスの名前を右クリックし、をクリックして **デバイスの更新**します。 これにより、デバイスが StorSimple Snapshot Manager と同期されます。 

## デバイスの構成の削除

次の手順に従って、StorSimple Snapshot Manager から個々の StorSimple デバイスの構成を削除します。

#### デバイス構成を削除するには

1. デスクトップ アイコンをクリックして、StorSimple Snapshot Manager を起動します。 

2.  **スコープ** ] ウィンドウで、をクリックして **デバイス**します。 

3.  **結果** ] ウィンドウでは、デバイスの名前を右クリックし、をクリックして **削除**します。 

4. 次のメッセージが表示されます。 をクリックして **はい** 構成を削除するか、クリックして **いいえ** 削除をキャンセルします。

    ![デバイスの構成の削除](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_DeleteDevice.png)

## 有効期限が切れたデバイス パスワードの変更

パスワードを入力して、StorSimple Snapshot Manager で StorSimple デバイスを認証する必要があります。 このパスワードは、Windows PowerShell インターフェイスを使用してデバイスを設定するときに構成します。 ただし、パスワードは期限切れになる場合があります。 この場合、Azure クラシック ポータルを使用してパスワードを変更することができます。 次に、デバイスはパスワードが期限切れになる前に StorSimple Snapshot Manager で構成されていたため、デバイスを StorSimple Snapshot Manager で再認証する必要があります。 

#### 有効期限が切れたパスワードを変更するには

1. Azure クラシック ポータルで StorSimple Manager サービスを開始します。

2. クリックして **デバイス** > **構成** デバイス用です。

3. StorSimple Snapshot Manager のセクションまでスクロールします。 14 から 15 文字のパスワードを入力します。 大文字、小文字、数字、および特殊文字を含むパスワードを使用してください。

4. 確認のためにパスワードを再入力します。

5. クリックして **保存** ページの下部にあります。

#### デバイスを再認証するには

1. StorSimple Snapshot Manager を開始します。

2.  **スコープ** ] ウィンドウで、をクリックして **デバイス**します。 構成されているデバイスの一覧に表示されます、 **結果** ウィンドウです。 

3. デバイスを右クリックし、クリックして選択 **認証**します。

4.  **認証** ウィンドウで、新しいパスワードを入力します。 

5. デバイス、右クリックし、選択の選択 **更新デバイス**します。 これにより、デバイスが StorSimple Snapshot Manager と同期されます。 

## 障害が発生したデバイスの置き換え

StorSimple device に障害が発生し、スタンバイ (フェールオーバー) デバイスによって置き換えられている場合は、次の手順に従って新しいデバイスに接続し、関連するバックアップを表示します。

#### フェールオーバー後に新しいデバイスに接続するには

1. 新しいデバイスへの iSCSI 接続を再構成します。 手順についてを参照してください"手順 7: マウント、初期化、およびボリュームの形式"で [オンプレミス StorSimple デバイスのデプロイ](storsimple-deployment-walkthrough.md)します。 

>[AZURE.NOTE] 新しい StorSimple デバイスに古いものと同じ IP アドレスがある場合は、古い構成で接続できる可能性があります。 

2. Microsoft StorSimple Management Service を停止します。

    1. Server Manager を起動します。

    2. サーバー マネージャー ダッシュ ボードで、 **ツール** メニューの [ **サービス**します。 

    3.  **サービス** ウィンドウを選択して、 **Microsoft StorSimple Management Service**します。 

    4. 右側のウィンドウで [ **Microsoft StorSimple Management Service**, 、クリックして **サービスを停止**します。 

3. 古いデバイスに関連する構成情報を削除します。 

    1. File Explorer で、C:\ProgramData\Microsoft\StorSimple\BACatalog を参照します。 

    2. BACatalog フォルダーでファイルを削除します。 

4. Microsoft StorSimple Management Service を再起動します。 

    1. サーバー マネージャー ダッシュ ボードで、 **ツール** メニューの [ **サービス**します。 

    2.  **サービス** ウィンドウを選択して、 **Microsoft StorSimple Management Service**します。 

    3. 右側のウィンドウで [ **Microsoft StorSimple Management Service**, をクリックして **サービスを再起動**します。 

5. StorSimple Snapshot Manager を開始します。 

6. 新しい StorSimple デバイスを構成するには、ステップ 2 の手順を完了します。 で StorSimple デバイスを接続 [StorSimple Snapshot Manager の展開](storsimple-snapshot-manager-deployment.md)します。 

7. トップ レベルのノードを右クリックし、 **スコープ** ウィンドウ (この例で StorSimple Snapshot Manager) とクリック **インポート表示の切り替え**します。 

8. インポートされたボリューム グループとバックアップが StorSimple Snapshot Manager に表示されると、メッセージが表示されます。 Click **OK**. 

## 次のステップ

- 学習方法 [StorSimple Snapshot Manager を使用した StorSimple ソリューションの管理](storsimple-snapshot-manager-admin.md)します。
- 学習方法 [StorSimple Snapshot Manager を使用して表示してボリュームを管理](storsimple-snapshot-manager-manage-volumes.md)します。


