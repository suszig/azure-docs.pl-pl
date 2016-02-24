<!--author=alkohli last changed: 12/01/15-->


#### デバイスを構成して登録するには

1. StorSimple デバイスのシリアル コンソールで Windows PowerShell インターフェイス にアクセスします。 参照してください [デバイスのシリアル コンソールに接続するための PuTTY を使用して](#use-putty-to-connect-to-the-device-serial-console) についてです。 **手順に厳密に従うことを確認するまたはコンソールにアクセスすることはできません。**

2. 開いたセッションで、Enter キーを 1 回押して、コマンド プロンプトを開きます。 

3. デバイスに設定する言語を選択するように求められます。 言語を指定し、Enter キーを押します。 

    ![StorSimple によるデバイスの構成および登録 1](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice1-include.png)

4. 表示されるシリアル コンソール メニューで、オプション 1 を選択してフル アクセスでログインします。 

    ![StorSimple によるデバイスの登録 2](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice2-include.png)
  
     手順 5. ～ 12. に従って、デバイスで必要な最小のネットワーク設定を構成します。 **これらの構成手順は、デバイスのアクティブなコント ローラーで実行する必要があります。**シリアル コンソール メニューでは、バナー メッセージにコントローラーの状態が示されます。 アクティブ コントローラーに接続されていない場合は、切断してアクティブ コントローラーに接続します。

5. コマンド プロンプトにパスワードを入力します。 デバイスの既定のパスワードは **Password1**します。

6. 次のコマンドを入力します。

     `Invoke-HcsSetupWizard` 

7. デバイスのネットワーク設定の構成に役立つセットアップ ウィザードが表示されます。 次の情報を指定します。 
   - DATA 0 ネットワーク インターフェイスの IP アドレス
   - サブネット マスク
   - ゲートウェイ
   - プライマリ DNS サーバーの IP アドレス
   - プライマリ NTP サーバーの IP アドレス
   
      > [AZURE.NOTE] サブネット マスクおよび DNS 設定を適用するのには数分待機する必要があります。 「デバイスができていません」が表示された場合 エラー メッセージで、アクティブ コント ローラーの DATA 0 ネットワーク インターフェイス上の物理ネットワーク接続を確認します。

8. (省略可能) Web プロキシ サーバーを構成します。 Web プロキシ構成は省略可能、 **web プロキシを使用する場合、のみ、構成できることは、ここに注意してください**します。 詳細についてを参照してください [デバイスの web プロキシの構成](storsimple-configure-web-proxy.md)します。 この手順では、問題を実行する場合のトラブルシューティングの指針を参照してください。 [web プロキシ構成中にエラー](storsimple-troubleshoot-deployment.md#errors-during-the-optional-web-proxy-settings)します。
 

      > [AZURE.NOTE] You can press Ctrl + C at any time to exit the setup wizard. Any settings that you applied before you issued this command will be retained.

9. セキュリティ上の理由で、デバイス管理者のパスワードは最初のセッション後に期限が切れるため、以降のセッションでは変更する必要があります。 画面の指示に従って、デバイスの管理者パスワードを入力します。 デバイス管理者の有効なパスワードの長さは、8 ～ 15 文字です。 大文字、小文字、数字、および特殊文字を組み合わせたパスワードを使用してください。

10. StorSimple Snapshot Manager のパスワードもここで設定します。 StorSimple Snapshot Manager が実行されている Windows ホストに対してデバイスを認証する場合は、このパスワードを使用します。 入力を求められたら、14 ～ 15 文字のパスワードを入力します。 パスワードには、小文字、大文字、数字、および特殊文字のうち 3 種類の文字を組み合わせる必要があります。 

    ![StorSimple によるデバイスの登録 4](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice4-include.png)

    StorSimple Snapshot Manager のパスワードは StorSimple Manager サービス インターフェイスからリセットできます。 詳細な手順を参照してください [StorSimple Manager サービスを使用して StorSimple のパスワードを変更する](storsimple-change-passwords.md)です。

    この手順では、問題のトラブルシューティングを行うには、トラブルシューティングのガイダンスを参照してください [パスワードに関連するエラー](storsimple-troubleshoot-deployment.md#errors-related-to-device-administrator-and-storsimple-snapshot-manager-passwords)します。

11. セットアップ ウィザードの最後の手順では、お使いのデバイスを StorSimple Manager サービスに登録します。 そのためには、手順 2. で取得したサービス登録キーが必要です。 登録キーを指定したら、デバイスが登録されるまでに 2 ～ 3 分かかる場合があります。

    参照して、潜在的なデバイスの登録の失敗をトラブルシューティングするには、 [デバイスの登録中にエラー](storsimple-troubleshoot-deployment.md#errors-during-device-registration)します。 トラブルシューティングについては、参照することもを [ステップ バイ ステップのトラブルシューティングの例](storsimple-troubleshoot-deployment.md#step-by-step-storsimple-troubleshooting-example)します。

12. デバイスが登録されると、サービス データ暗号化キーが表示されます。 このキーをコピーし、安全な場所に保存しておきます。
    
    > [AZURE.WARNING] このキーは、StorSimple Manager サービスに追加のデバイスを登録するサービス登録キーと共に必要になります。 参照してください [StorSimple のセキュリティ](../articles/storsimple/storsimple-security.md) このキーの詳細についてです。

     ![StorSimple によるデバイスの登録 6](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice6-include.png)

     シリアル コンソール ウィンドウからテキストをコピーするには、単にテキストを選択します。 その状態で、クリップボードまたは任意のテキスト エディターに貼り付けることができます。 サービス データ暗号化キーをコピーするときには、Ctrl キーを押しながら C キーを押さないでください。 Ctrl キーを押しながら C キーを押すと、セットアップ ウィザードが終了します。 その場合、デバイスの管理者パスワードおよび StorSimple Snapshot Manager のパスワードは変更されず、デバイスは既定のパスワードに戻ります。

13. シリアル コンソールを終了します。

14. Azure クラシック ポータルに戻り、次の手順を実行します。
  1. StorSimple Manager サービスにアクセスする] をダブルクリック、 **クイック スタート** ページです。
  2. クリックして **ビューには、デバイスが接続されている**します。
  3.  **デバイス** ] ページで、デバイスが正常に接続されていることをサービスに、状態を参照することを確認します。 デバイスの状態にする必要があります **オンライン**します。 デバイスの状態が場合 **オフライン**, 、デバイスがオンラインにするまで数分待ちます。
   
    ![StorSimple Devices page](./media/storsimple-configure-and-register-device/HCS_DevicesPageM-include.png) 
  
      > [AZURE.IMPORTANT] After the device is online, plug in the network cables that you had unplugged in the beginning of this step.

デバイスを登録したが、オンラインにならない場合、`Test-HcsmConnection -Verbose` を実行し、ネットワーク接続の正常な状態を確認できます。 このコマンドレットの詳細な使用状況を参照してください [Test-hcsmconnection のコマンドレット リファレンス](https://technet.microsoft.com/library/dn715782.aspx)します。

![使用可能なビデオ](./media/storsimple-configure-and-register-device/Video_icon.png) **ビデオ**

クリックして構成し、StorSimple 用 Windows PowerShell を使用してデバイスを登録する方法を示すビデオを見る、 [ここ](http://azure.microsoft.com/documentation/videos/initialize-the-storsimple-appliance/)します。
