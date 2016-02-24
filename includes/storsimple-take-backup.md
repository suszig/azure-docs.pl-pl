<!--author=alkohli last changed: 9/17/15-->

### バックアップを作成するには

1. デバイスの **クイック スタート** ] ページで [ **バックアップ ポリシーの追加**します。 バックアップ ポリシーの追加ウィザードが開きます。 

2.  **バックアップ ポリシーの定義** ページ。
  1. バックアップ ポリシーに 3 ～ 150 文字の名前を指定します。
  2. バックアップするボリュームを選択します。 複数のボリュームを選択した場合、ボリュームはグループ化され、クラッシュ整合バックアップが作成されます。
  3. 矢印アイコンをクリックします。 ![矢印アイコン](./media/storsimple-take-backup/HCS_ArrowIcon-include.png). 
  
    ![Add-backup-policy](./media/storsimple-take-backup/HCS_AddBackupPolicyWizard1M-include.png)

3.  **スケジュールを定義する** ページ。
  1. ドロップダウン リストからバックアップの種類を選択します。 早く復元は、次のように選択します。 **ローカル スナップショット**します。 データの回復性は、選択 **クラウド スナップショット**します。
  2. バックアップの頻度を分、時間、日、または週で指定します。
  3. 保存期間を選択します。 保存期間の選択肢はバックアップの頻度によって異なります。 たとえば、日次のポリシーでは保存期間を日単位で指定できるのに対して、月次のポリシーでは保存期間は月単位で指定します。
  4. バックアップ ポリシーの開始日時を選択します。
  5. 選択、 **を有効にする** バックアップ ポリシーを有効にする] チェック ボックスです。 
  6. チェック マーク アイコンをクリックします。 ![チェック マーク アイコン](./media/storsimple-take-backup/HCS_CheckIcon-include.png) ポリシーを保存します。

    ![Add-backup-policy](./media/storsimple-take-backup/HCS_AddBackupPolicyWizard2M-include.png)
 
     You now have a backup policy that will create scheduled backups of your volume data.

デバイスの構成が完了しました。 

![使用可能なビデオ](./media/storsimple-take-backup/Video_icon.png) **ビデオ**

クリックして、StorSimple のバックアップを作成する方法を示すビデオを見る、 [ここ](http://azure.microsoft.com/documentation/videos/take-a-storsimple-backup/)します。
