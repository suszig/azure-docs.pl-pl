
#### StorSimple 用 Windows PowerShell を使用して通常の修正プログラムをインストールするには

1. デバイスのシリアル コンソールに接続します。 詳細については、次を参照してください。 [手順 1: シリアル コンソールに接続](storsimple-update-device.md#step1)します。

2. シリアル コンソール メニューで、オプション 1 の **[フル アクセスによるログイン]** を選択します。 パスワードを入力します。 既定のパスワードは **Password1** です。

3. コマンド プロンプトに、次のコマンドを入力します。

    `Start-HcsHotfix`

       >[AZURE.IMPORTANT]
       >
       >- This command applies only to regular hotfixes. You run this command on only one controller, but both controllers will be updated.
       >- You may notice a controller failover during the update process; however, the failover will not affect system availability or operation.

4. メッセージが表示されたら、修正プログラムのファイルが含まれているネットワーク共有フォルダーへのパスを入力します。

5. 確認を求められます。 「**Y**」と入力して修正プログラムのインストールを続行します。





