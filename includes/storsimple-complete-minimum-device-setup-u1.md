<!--author=alkohli last changed: 9/17/15-->

#### StorSimple の最小デバイス セットアップを完了するには

1. デバイスを選択し、クリックして **クイック スタート**します。 クリックして **デバイス セットアップを完了** をデバイスの構成ウィザードを開始します。

2. デバイスの構成ウィザードで **基本設定** ] ダイアログ ボックスで、次の操作します。
  1. 指定、 **フレンドリ名** デバイス用です。 既定のデバイス名は、デバイスのモデルやシリアル番号などの情報を反映します。 デバイスを管理するために、最大 64 文字の表示名を割り当てることができます。
  2. 設定、 **タイム ゾーン** デバイスをデプロイする地理的場所に基づいています。 デバイスは、スケジュールされたすべての操作でこのタイム ゾーンを使用します。
  3.  **DNS 設定**, 、アドレスを指定して **セカンダリ DNS サーバー**します。 IPv6 を使用する場合、このフィールドには、Windows PowerShell インターフェイスに提供される IPv6 プレフィックスに基づいて値が入力されます。 
  セカンダリ DNS サーバーが構成されていない場合、デバイスの構成を保存することはできません。
  4. [iSCSI 対応インターフェイス] で、iSCSI 用に少なくとも 1 つのネットワークを有効にします。 1 つ以上のネットワーク インターフェイスをクラウド対応とし、1 つのインターフェイスを iSCSI 対応とする必要があります。 DATA 0 は自動的にクラウド対応となります。
 
      ![StorSimple の最小限のデバイス セットアップ基本設定](./media/storsimple-complete-minimum-device-setup-u1/HCS_MinDeviceSetupBasicSettings1-include.png)

3. 矢印アイコンをクリックします。 ![StorSimple の矢印アイコン](./media/storsimple-complete-minimum-device-setup/HCS_ArrowIcon-include.png)

4.  **ネットワーク インターフェイス** ] ダイアログ ボックスで、コント ローラー 0 とコント ローラー 1 の固定の IP アドレスを指定します。 **コント ローラーの固定 IP アドレスは、デバイスの IP アドレスによってアクセス可能なサブネット内で空き Ip となっている必要があります。**DATA 0 インターフェイスが IPv4 に対して構成されている場合、固定 IP アドレスは IPv4 形式で指定する必要があります。 IPv6 構成でプレフィックスを指定した場合、これらのフィールドには固定 IP アドレスが自動的に入力されます。


    ![StorSimple minimum device setup network interfaces](./media/storsimple-complete-minimum-device-setup-u1/HCS_MinDeviceSetupNetworkInterfaces2-include.png)

    The fixed IP addresses for the controller are used for servicing the updates to the device, and therefore the fixed IPs must be routable and able to connect to the Internet. You can check that your fixed controller IPs are routable by using the [Test-HcsmConnection][Test] cmdlet. The following example shows fixed controller IPs are routed to the Internet and can access the Microsoft Update servers. 

     ![Test-HcsmConnection showing routable IPs](./media/storsimple-complete-minimum-device-setup-u1/Test-HcsmConnectionOutputRegisteredDevice.png)

5. チェック アイコンをクリックして ![StorSimple のチェック マーク アイコン](./media/storsimple-complete-minimum-device-setup/HCS_CheckIcon-include.png)します。
  デバイスが再び表示 **クイック スタート** ページです。

 > [AZURE.NOTE] いつでもその他のすべてのデバイス設定を変更するにはアクセスすることによって、 **構成** ページです。

<!--Link reference-->
[Test]: https://technet.microsoft.com/library/dn715782(v=wps.630).aspx
