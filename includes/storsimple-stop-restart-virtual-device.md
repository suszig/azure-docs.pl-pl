#### 仮想デバイスを停止および開始するには
停止する仮想デバイスの名前をクリックし、クリックして **シャット ダウン**します。 仮想デバイスをシャット ダウン中の状態は **停止**します。 仮想デバイスを停止した後、状態は **Stopped**します。

仮想デバイスを停止および開始するには、以下のコマンドレットを使用します。

`Stop-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`


`Start-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`
    
#### 仮想デバイスを再起動するには

仮想デバイスで実行すると、それを再開する、その名前をクリックし、 **再起動**します。 仮想デバイスの再起動中の状態は **再開中**します。 使用するには、仮想デバイス、その状態は **を実行している**します。

仮想デバイスを再起動するには、以下のコマンドレットを使用します。

`Restart-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`





