<!--author=SharS last changed: 11/16/15-->

#### ボリュームを作成するには

1. デバイスの **クイック スタート** ] ページで [ **ボリュームの追加**します。 ボリュームの追加ウィザードが開始されます。

2. ボリューム ウィザードの追加 [ **基本設定**, を次の操作します。
   1. 指定、 **名前** 、ボリューム。
   2. 指定の **プロビジョニングされた容量** GB または TB 単位で、ボリューム。 物理デバイスの場合、1 GB ～ 64 TB の範囲で容量を指定する必要があります。
   3. ドロップダウン リストで選択、 **使用法の種類** 、ボリューム。 
   4. このボリュームのアーカイブ データを使用している場合は、選択、 **このボリュームを使用して、アクセス頻度の低いアーカイブ データ** チェック ボックスをオンします。 その他のすべてのユース ケースを選択するだけ **階層化されたボリューム**します。  (旧称はプライマリ ボリューム)。
   4. 矢印アイコンをクリックします。 ![矢印アイコン](./media/storsimple-create-volume/HCS_ArrowIcon-include.png) 次のページに移動します。

        ![ボリュームの追加](./media/storsimple-create-volume/AddVolume1-include.png)

3.  **追加設定** ダイアログ ボックスで新しいアクセス制御レコード (ACR) を追加します。
   1. 指定する **名前** ACR のです。
   2.  **ISCSI イニシエーターの名前**, 、iSCSI を提供する Windows ホストの修飾名 (IQN)。 IQN を取得していない場合は、「 [Windows Server ホスト IQN を取得する](#get-the-iqn-of-a-windows-server-host)です。
   3. 選択して、既定のバックアップを有効にすることをお勧めします **このボリュームの既定のバックアップを有効にする** チェック ボックスをオンします。 既定のバックアップでは、毎日 22:30 (デバイスの時刻) に実行し、このボリュームのクラウド スナップショットを作成するというポリシーが作成されます。

        > [AZURE.NOTE] バックアップがここで有効には元に戻せません。 この設定を変更するには、このボリュームを編集する必要があります。

        ![ボリュームの追加](./media/storsimple-create-volume/AddVolume2-include.png)

4. チェック マーク アイコンをクリックします。 ![チェック マーク アイコン](./media/storsimple-create-volume/HCS_CheckIcon-include.png). 指定された設定でボリュームが作成されます。

![使用可能なビデオ](./media/storsimple-create-volume/Video_icon.png) **ビデオ**

StorSimple ボリュームを作成する方法のデモ ビデオを視聴するにはクリックして [ここ](http://azure.microsoft.com/documentation/videos/create-a-storsimple-volume/)します。


