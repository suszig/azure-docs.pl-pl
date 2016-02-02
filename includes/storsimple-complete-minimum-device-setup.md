
#### StorSimple の最小デバイス セットアップを完了するには

1. **[デバイス]** ページで、デバイスを選択し、デバイス名を指している矢印をクリックして特定のデバイス ページに進みます。

    ![オンライン デバイスの ](./media/storsimple-complete-minimum-device-setup/HCS_DevicesPageM-include.png)

2. クイック スタート アイコンをクリックして ![クイック スタート アイコン](./media/storsimple-complete-minimum-device-setup/HCS_QuickStartIcon-include.png) デバイスのクイック スタート ページにアクセスします。 **[デバイス セットアップを完了する]** をクリックして、**デバイスの構成**ウィザードを起動します。

    ![デバイスの ](./media/storsimple-complete-minimum-device-setup/Device_Quick_Start_page_1M.png)

2. **[基本設定]** ページで次の操作を行います。
  1. デバイスの **[表示名]** を指定します。 既定のデバイス名は、デバイスのモデルやシリアル番号などの情報を反映します。 デバイスを管理するために、最大 64 文字の表示名を割り当てることができます。
  2. デバイスをデプロイする地理的な場所に基づいて**タイム ゾーン**を設定します。 デバイスは、スケジュールされたすべての操作でこのタイム ゾーンを使用します。
  3. **[DNS 設定]** で **[セカンダリ DNS サーバー]** のアドレスを指定します。 IPv6 を使用する場合、このフィールドには、Windows PowerShell インターフェイスに提供される IPv6 プレフィックスに基づいて値が入力されます。 
  セカンダリ DNS サーバーが構成されていない場合、デバイスの構成を保存することはできません。
  4. [iSCSI 対応インターフェイス] で、iSCSI 用に少なくとも 1 つのネットワークを有効にします。 1 つ以上のネットワーク インターフェイスをクラウド対応とし、1 つのインターフェイスを iSCSI 対応とする必要があります。 DATA 0 は自動的にクラウド対応となります。

      ![StorSimple の最小限のデバイス セットアップ基本設定](./media/storsimple-complete-minimum-device-setup/HCS_MinDeviceSetupBasicSettings1-include.png)

3. 矢印アイコンをクリックします。 ![StorSimple の矢印アイコン](./media/storsimple-complete-minimum-device-setup/HCS_ArrowIcon-include.png)

4. **[ネットワーク インターフェイス]** ページで、コントローラー 0 とコントローラー 1 の固定 IP アドレスを指定します。 DATA 0 インターフェイスが IPv4 に対して構成されている場合、固定 IP アドレスは IPv4 形式で指定する必要があります。 IPv6 構成でプレフィックスを指定した場合、これらのフィールドには固定 IP アドレスが自動的に入力されます。

    > [AZURE.NOTE] 
    > 
    > -コント ローラーの固定 IP アドレスは、デバイスの IP アドレスによってアクセス可能なサブネット内で空き Ip となっている必要があります。
    > -サービス、デバイスに更新プログラムの固定の IP アドレス、コント ローラーが使用され、固定の ip アドレスでルーティング可能で、インターネットに接続することにする必要がありますので。

    ![StorSimple の最小限のデバイス セットアップのネットワーク インターフェイス](./media/storsimple-complete-minimum-device-setup/HCS_MinDeviceSetupNetworkInterfaces2-include.png)

5. チェック アイコンをクリックして ![StorSimple のチェック マーク アイコン](./media/storsimple-complete-minimum-device-setup/HCS_CheckIcon-include.png)します。
  デバイスの **[クイック スタート]** ページが再び表示されます。
 > [AZURE.NOTE] **[構成]** ページにアクセスすれば、その他すべてのデバイス設定をいつでも変更できます。

![ビデオ](./media/storsimple-complete-minimum-device-setup/Video_icon.png) **ビデオ**

デバイスの最小セットアップを完了する方法を示すビデオを見るには、次のようにクリックします。 [ここ](http://azure.microsoft.com/documentation/videos/minimum-storsimple-device-setup/)します。




