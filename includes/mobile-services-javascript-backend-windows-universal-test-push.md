
1. Visual Studio では、Windows ストア プロジェクトを右クリックし、クリックして **スタートアップ プロジェクトとして設定**, 、F5 キーを押して Windows ストア アプリを実行します。
    
    アプリが起動すると、デバイスがプッシュ通知に登録されます。

2. Windows ストア アプリを停止し、Windows Phone ストア アプリを実行する前の手順を繰り返します。

    この時点で、両方のデバイスがプッシュ通知を受信するように登録されています。

3. Windows ストア アプリを再実行し、テキストを入力して **Insert a TodoItem**, 、] をクリックし、 **保存**します。

    ![](./media/mobile-services-javascript-backend-windows-universal-test-push/mobile-quickstart-push1.png)

    挿入が完了すると、Windows ストアと Windows Phone アプリの両方が WNS からのプッシュ通知を受信します。

    ![](./media/mobile-services-javascript-backend-windows-universal-test-push/mobile-quickstart-push2.png)

    通知は、アプリが実行していなくても Windows Phone に表示されます。

    ![](./media/mobile-services-javascript-backend-windows-universal-test-push/mobile-quickstart-push5-wp8.png)


