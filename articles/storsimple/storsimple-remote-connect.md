<properties 
   pageTitle="StorSimple デバイスにリモート接続する | Microsoft Azure"
   description="リモート管理用にデバイスを構成する方法と、HTTP または HTTPS を介して StorSimple 用 Windows PowerShell に接続する方法について説明します。"
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carolz"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="12/02/2015"
   ms.author="alkohli" />


# StorSimple デバイスにリモート接続する

## 概要

Windows PowerShell リモート処理を使用して StorSimple デバイスに接続できます。 この方法で接続すると、メニューは表示されません  (メニューは、デバイスのシリアル コンソールを使用して接続する場合のみ表示されます)。 Windows PowerShell リモート処理を使用して、特定の実行空間に接続します。 表示言語を指定することもできます。

Windows PowerShell リモート処理を使用して、デバイスを管理する方法の詳細についてを参照してください [StorSimple デバイスを管理する StorSimple 用 Windows PowerShell を使用して](storsimple-windows-powershell-administration.md)します。

このチュートリアルでは、リモート管理用にデバイスを構成する方法と、Windows PowerShell for StorSimple に接続する方法について説明します。 HTTP または HTTPS を使用して Windows PowerShell リモート処理経由で接続できます。 ただし、Windows PowerShell for StorSimple に接続する方法を決定するときは、次の点を考慮してください。

- デバイスのシリアル コンソールへの直接接続はセキュリティで保護されていますが、ネットワーク スイッチ経由での接続は保護されていません。 ネットワーク スイッチ経由でデバイスのシリアル コンソールに接続するときは、セキュリティ上のリスクに注意してください。

- HTTP セッション経由での接続は、ネットワーク経由でシリアル コンソールに接続するよりも安全性が高い可能性があります。 これは最も安全な方法ではありませんが、信頼されたネットワークでは容認できます。

- 自己署名証明書を使用する HTTPS セッション経由での接続は、最も安全であり、推奨されるオプションです。

Windows PowerShell インターフェイスにリモート接続できます。 ただし、Windows PowerShell インターフェイス経由での StorSimple デバイスへのリモート アクセスは既定では有効になっていません。 最初にデバイスでリモート管理を有効にし、次にデバイスにアクセスするために使用するクライアントでリモート管理を有効にする必要があります。

## HTTP 経由の接続

HTTP セッション経由の Windows PowerShell for StorSimple への接続は、StorSimple デバイスのシリアル コンソール経由での接続よりも高い安全性を提供します。 これは最も安全な方法ではありませんが、信頼されたネットワークでは容認できます。

Azure クラシック ポータルまたはシリアル コンソールを使用してリモート管理を構成できます。 次の手順から選択してください。

- [Azure クラシック ポータルを使用して HTTP 経由でリモート管理を有効にします。](#use-the-azure-classic-portal-to-enable-remote-management-over-http)

- [シリアル コンソールを使用して HTTP 経由でリモート管理を有効にします。](#use-the-serial-console-to-enable-remote-management-over-http)

リモート管理を有効にした後、次の手順に従ってリモート接続用のクライアントを準備します。

- [リモート接続のクライアントを準備します。](#prepare-the-client-for-remote-connection)

### Azure クラシック ポータルを使用して HTTP 経由でリモート管理を有効にする

Azure クラシック ポータルで、次の手順に従って、HTTP 経由でのリモート管理を有効にします。

#### Azure クラシック ポータル経由でリモート管理を有効にするには

1. **[デバイス]** を選択し、デバイス用の **[構成]** をクリックします。

2. 下へスクロールして **[リモート管理]** セクションに移動します。

3. **[リモート管理を有効にする]** を **[はい]** に設定します。

4. これで、HTTP を使用した接続を選択できるようになります。 (既定の設定は HTTPS 経由での接続です)。 HTTP が選択されていることを確認します。
    >[AZURE.NOTE] HTTP 経由での接続は、信頼のおけるネットワークでのみ許容されます。

6. ページの下部にある **[保存]** をクリックします。

### シリアル コンソールを使用して HTTP 経由でのリモート管理を有効にする

デバイスのシリアル コンソールで、次の手順を実行して、リモート管理を有効にします。

#### デバイスのシリアル コンソール経由でリモート管理を有効にするには

1. シリアル コンソール メニューで、オプション 1 を選択します。 詳細については、デバイスのシリアル コンソールを使用して、参照してください [デバイスのシリアル コンソール経由で StorSimple 用 Windows PowerShell への接続](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console)します。

2. プロンプトで「: `有効にする HcsRemoteManagement – AllowHttp`

3. HTTP を使用してデバイスに接続した場合のセキュリティの脆弱性についての通知が表示されます。 確認を求められたら、「**Y**」を入力して確認します。

4. HTTP が」と入力して有効になっていることを確認します `Get HcsSystem`。

5. **RemoteManagementMode** フィールドに **HttpsAndHttpEnabled** が表示されていることを確認します。次の図に、PuTTY の設定を示します。

     ![シリアル HTTPS と HTTP の有効化](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### リモート接続用のクライアントを準備する

クライアントで、次の手順を実行して、リモート管理を有効にします。

#### リモート接続用のクライアントを準備するには

1. Windows PowerShell セッションを管理者として開始します。

2. 次のコマンドを入力して、StorSimple デバイスの IP アドレスをクライアントの信頼されたホストの一覧に追加します。

     `Set-item n:\localhost\client\trustedhosts < device_ip >-Force を連結`

     置換 <*device_ip*> 、デバイスの IP アドレスを持つ例。

     `Set-item n:\localhost\client\trustedhosts 先 10.126.173.90 なっています-Force を連結`

3. 次のコマンドを入力して、デバイスの資格情報を変数に保存します。

     *$cred = Get-Credential*

4. 表示されるダイアログ ボックスで、次の操作を行います。

    1. ユーザー名を次の形式で入力します。*device_ip\SSAdmin*
    2. セットアップ ウィザードでデバイスを構成したときに設定したデバイスの管理者パスワードを入力します。 既定のパスワードは *Password1* です。

7. 次のコマンドを入力して、デバイスの Windows PowerShell セッションを開始します。

     `入力 pssession の資格情報の $cred ConfigurationName SSAdminConsole - ComputerName < device_ip >`
     >[AZURE.NOTE] StorSimple 仮想デバイスで使用するための Windows PowerShell セッションを作成するには、追加、 `– ポート` パラメーター StorSimple 仮想アプライアンスのリモート処理で構成したパブリック ポートを指定します。

     この時点で、デバイスに対するアクティブなリモート Windows PowerShell セッションが確立されます。

    ![HTTP を使用した PowerShell リモート処理](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## HTTPS 経由の接続

HTTPS セッション経由での Windows PowerShell for StorSimple への接続は、Microsoft Azure StorSimple デバイスへのリモート接続よりも安全性が高い推奨される方法です。 次の手順は、HTTPS を使用して Windows PowerShell for StorSimple に接続できるようにシリアル コンソールとクライアント コンピューターを設定する方法について説明しています。

Azure クラシック ポータルまたはシリアル コンソールを使用してリモート管理を構成できます。 次の手順から選択してください。

- [Azure クラシック ポータルを使用して HTTPS 経由でリモート管理を有効にします。](#use-the-azure-classic-portal-to-enable-remote-management-over-https)

- [シリアル コンソールを使用して HTTPS 経由でリモート管理を有効にします。](#use-the-serial-console-to-enable-remote-management-over-https)

リモート管理を有効にした後、次の手順に従ってリモート管理用のホストを準備し、リモート ホストからデバイスに接続します。

- [リモート管理のためホストを準備します。](#prepare-the-host-for-remote-management)

- [リモート ホストからデバイスへの接続します。](#connect-to-the-device-from-the-remote-host)

### Azure クラシック ポータルを使用して HTTPS 経由でリモート管理を有効にする

Azure クラシック ポータルで、次の手順に従って、HTTPS 経由でのリモート管理を有効にします。

#### Azure クラシック ポータル経由で HTTPS 経由でリモート管理を有効にするには

1. **[デバイス]** を選択し、デバイス用の **[構成]** をクリックします。

2. 下へスクロールして **[リモート管理]** セクションに移動します。

3. **[リモート管理を有効にする]** を **[はい]** に設定します。

4. これで、HTTPS を使用した接続を選択できるようになります  (既定の設定は HTTPS 経由での接続です)。 HTTPS が選択されていることを確認します。

5. **[リモート管理証明書をダウンロードする]** をクリックします。 このファイルを保存する場所を指定します。 この証明書を、デバイスに接続するために使用するクライアントまたはホスト コンピューターにインストールする必要があります。

6. ページの下部にある **[保存]** をクリックします。

### シリアル コンソールを使用して HTTPS 経由でのリモート管理を有効にする

デバイスのシリアル コンソールで、次の手順を実行して、リモート管理を有効にします。

#### デバイスのシリアル コンソール経由でリモート管理を有効にするには

1. シリアル コンソール メニューで、オプション 1 を選択します。 詳細については、デバイスのシリアル コンソールを使用して、参照してください [デバイスのシリアル コンソール経由で StorSimple 用 Windows PowerShell への接続](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console)します。

2. プロンプトで、次のように入力します。

     `有効にする HcsRemoteManagement`

    これで、デバイスの HTTPS が有効になります。

3. 次のように入力して、HTTPS が有効になっていることを確認します。

     `Get HcsSystem`

    **RemoteManagementMode** フィールドに **HttpsEnabled** が表示されていることを確認します。次の図に、PuTTY の設定を示します。

     ![シリアル HTTPS の有効化](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)

4. 出力から `Get HcsSystem`, 、デバイスのシリアル番号をコピーし、後で使用するために保存します。
    >[AZURE.NOTE] シリアル番号は、証明書の CN 名に対応します。

5. 次のように入力して、リモート管理証明書を取得します。

     `Get HcsRemoteManagementCert`

    証明書は、次の画面に似たものになります。

    ![リモート管理証明書の取得](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)

5. **-----BEGIN CERTIFICATE-----** から **-----END CERTIFICATE-----** までの証明書の情報をメモ帳などのテキスト エディターにコピーし、.cer ファイルとして保存します  (このファイルは、ホストを準備するときにリモート ホストにコピーします)。
    >[AZURE.NOTE] 新しい証明書を生成するには、使用、 `セット HcsRemoteManagementCert` コマンドレットです。

### リモート管理用のホストを準備する

HTTPS セッションを使用するリモート接続のホスト コンピューターを準備するには、次の手順を実行します。

- [クライアントまたはリモート ホストのルート ストアに .cer ファイルをインポート](#to-import-the-certificate-on-the-remote-host)します。

- [デバイスのシリアル番号をリモート ホスト上の hosts ファイルに追加](#to-add-device-serial-numbers-to-the-remote-host)します。

この後、これらの手順について説明します。

#### 証明書をリモート ホストにインポートするには

1. .cer ファイルを右クリックし、**[証明書のインストール]** を選択します。 証明書インポート ウィザードが起動します。

    ![証明書インポート ウィザード (その 1)](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)

2. **[ストアの場所]** で **[ローカル マシン]** を選択し、**[次へ]** をクリックします。

3. **[証明書をすべて次のストアに配置する]** を選択し、**[参照]** をクリックします。 リモート ホストのルート ストアに移動し、**[次へ]** をクリックします。

    ![証明書インポート ウィザード (その 2)](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)

4. **[完了]** をクリックします。 インポートが成功したことを通知するメッセージが表示されます。

    ![証明書インポート ウィザード (その 3)](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### デバイスのシリアル番号をリモート ホストに追加するには

1. メモ帳を管理者として起動し、\Windows\System32\Drivers\etc にあるホスト ファイルを開きます。

2. 次の 3 つのエントリを hosts ファイルに追加します。**DATA 0 の IP アドレス**、**コントローラー 0 の固定 IP アドレス**、および **コントローラー 1 の固定 IP アドレス**。

3. 前に保存したデバイスのシリアル番号を入力します。 次の図に示すように、IP アドレスにマップします。 コントローラー 0 とコントローラー 1 の場合は、シリアル番号 (CN 名) の末尾に **Controller0** と **Controller1** を追加します。

    ![hosts ファイルへの CN 名の追加](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)

4. hosts ファイルを保存します。

### リモート ホストからデバイスに接続する

Windows PowerShell と SSL を使用して、リモート ホストまたはクライアントからデバイスの SSAdmin セッションに移行します。 SSAdmin セッションは、オプション 1 にマップ、 [シリアル コンソール](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console) 、デバイスのメニュー。

Windows PowerShell のリモート接続元にするコンピューターで次の手順を実行します。

#### Windows PowerShell と SSL を使用してデバイスの SSAdmin セッションに移行するには

1. Windows PowerShell セッションを管理者として開始します。

2. 次のように入力して、クライアントの信頼されたホストにデバイスの IP アドレスを追加します。

     `Set-item n:\localhost\client\trustedhosts < device_ip >-Force を連結`

    ここで <*device_ip*> 、デバイスの IP アドレスです。 たとえば。

     `Set-item n:\localhost\client\trustedhosts 先 10.126.173.90 なっています-Force を連結`

3. 次のように入力して、新しい資格情報を作成します。

     `$cred = new-object は、pscredential @("< IP of target device > \SSAdmin"(convertto securestring - 強制的に-asplaintext「< デバイス管理者のパスワード >」))`

    場所 <*IP of target device*> 、デバイスの DATA 0 の IP アドレスは、たとえば、 **先 10.126.173.90 なっています** hosts ファイルの前の図に示すようにします。デバイスの管理者パスワードも指定します。

4. 次のように入力して、セッションを作成します。

     `$session = 新しい pssession usessl CN < ターゲット デバイスのシリアル番号 >-資格情報の $cred configurationname"SSAdminConsole"`

    コマンドレットの CN 名、提供、 <*serial number of target device*>します。このシリアル番号は、リモート ホストの hosts ファイルで、DATA 0 の IP アドレスにマップしたものです。たとえば、次の図に示す **SHX0991003G44MT** です。

5. 次のコマンドを入力します。

     `入力 PSSession $session`

6. 数分間待機する必要があります。その後、HTTPS 経由の SSL でデバイスに接続されます。 デバイスに接続されたことを示すメッセージが表示されます。

    ![HTTPS と SSL を使用した PowerShell リモート処理](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## 次のステップ

- 詳細について [StorSimple デバイスを管理する Windows PowerShell を使用して](storsimple-windows-powershell-administration.md)します。

- 詳細について [StorSimple Manager サービスを使用して、StorSimple デバイスを管理する](storsimple-manager-service-administration.md)します。





