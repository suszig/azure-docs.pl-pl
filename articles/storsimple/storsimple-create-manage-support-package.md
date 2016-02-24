<properties 
   pageTitle="StorSimple サポート パッケージの作成 | Microsoft Azure"
   description="StorSimple デバイスのサポート パッケージを作成、暗号化解除、編集する方法について説明します。"
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carolz"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/02/2015"
   ms.author="alkohli" />


# StorSimple サポート パッケージを作成および管理する

## 概要

このチュートリアルでは、サポート パッケージの作成と管理に関するさまざまなタスクについて説明します。 サポート パッケージには、関連するすべてのログが暗号化され、圧縮された形式で含まれています。また、サポート パッケージは、Microsoft サポート チームによる StorSimple デバイスの問題のトラブルシューティングに役立ちます。

このチュートリアルでは、次の手段を使用してサポート パッケージを作成および管理するための手順を取り上げています。

- **サポート パッケージ** のセクションで、 **メンテナンス** 、StorSimple Manager サービス ページ
- StorSimple 用 Windows PowerShell

このチュートリアルを読むと、次のことができるようになります。

- サポート パッケージを作成する
- サポート パッケージの暗号化を解除して編集する


## Azure クラシック ポータルでサポート パッケージを作成する

StorSimple Manager サービスに発生する可能性のある問題をトラブルシューティングするには、作成してサポート パッケージを使用して Microsoft サポート サイトにアップロード、 **メンテナンス** Azure クラシック ポータルで、サービスのページです。 アップロードを許可するには、サポート パスキーを指定する必要があります。 サポート パスキーは、サポート エンジニアから電子メールで提供されます。 暗号化されていない、圧縮されたサポート パッケージが作成されます (.cab ファイル)。 その後、このパッケージは、サポート エンジニアがパスキーを指定したときにサポート サイトから取得できます。

サポート パッケージを作成するには、クラシック ポータルで次の手順を実行します。

#### Azure クラシック ポータルでサポート パッケージを作成するには

1. 移動 **デバイス > メンテナンス**します。

2.  **サポート パッケージ** ] をクリックして **サポート パッケージの作成とアップロード**します。

3.  **サポート パッケージの作成とアップロード** ] ダイアログ ボックスで、次の操作します。

    ![サポート パッケージの作成](./media/storsimple-create-manage-support-package/IC740923.png)
                                            
    - 提供、 **サポート パスキー**します。 このキーは、Microsoft サポート エンジニアから電子メールで送信されます。
    
    - ことに同意するコンボ ボックスをオン **サポート パッケージを Microsoft サポート サイトに自動的にアップロード**します。
    
    - チェック マーク アイコンをクリックします。 ![チェック マーク アイコン](./media/storsimple-create-manage-support-package/IC740895.png).


## StorSimple 用 Windows PowerShell でサポート パッケージを作成する

パッケージを作成する前にログ ファイルを編集する必要がある場合は、StorSimple 用 Windows PowerShell を使用してパッケージを作成する必要があります。 

StorSimple 用 Windows PowerShell でサポート パッケージを作成するには、次の手順を実行します。


#### StorSimple 用 Windows PowerShell でサポート パッケージを作成するには

1. StorSimple デバイスへの接続に使用されるリモート コンピューターで、次のコマンドを入力し、Windows PowerShell セッションを管理者として起動します。

    `Start PowerShell`

2. Windows PowerShell セッションで、次の手順を実行し、デバイスの SSAdmin コンソール実行空間に接続します。 


    - At the command prompt, type: 
            
        `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`
        
        
    1. In the dialog box that opens, type your device administrator password. The default password is:
     
        `Password1`

        ![PowerShell Session To SSAdminConsole Runspace](./media/storsimple-create-manage-support-package/IC740962.png)

    2. Click **OK**.
    1. At the command prompt, type: 
        
        `Enter-PSSession $MS`


3. 表示されたセッションに、適切なコマンドを入力します。 


    - For network shares that are password protected, type:

        `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`

        You will be prompted for a password, a path to the network shared folder, and an encryption passphrase (because the support package is encrypted). When these are provided, a support package will be created in the specified folder.
                                            

    - For open network shared folders (those that are not password protected), you do not need the `-Credential` parameter. Type the following: 

        `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`

        The support package will be created for both controllers in the specified network shared folder. It is an encrypted, compressed file that can be sent to Microsoft Support for troubleshooting. For more information, see [Contact Microsoft Support](storsimple-contact-microsoft-support.md).


### Export-HcsSupportPackage コマンドレットの詳細
Export-HcsSupportPackage コマンドレットで使用できるさまざまなパラメーターを次の表にまとめています。

| 連続 番号 | パラメーター            | 必須/省略可能 | 説明                                                                                                                                                             |
|--------|----------------------|-------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1      | パス                 | 必須          | サポート パッケージが配置されるネットワーク共有フォルダーの場所を指定するためにします。                                                                 |
| 2      | EncryptionPassphrase | 必須          | サポート パッケージの暗号化に使用するパスフレーズを指定するためにします。                                                                                                        |
| 3      | 資格情報           | 省略可能          | ネットワーク共有フォルダーのアクセス資格情報を指定するためにします。                                                                                        |
| 4      | Force                | 省略可能          | 暗号化パスフレーズの確認手順をスキップするために使用します。                                                                                                                |
| 5      | PackageTag           | 省略可能          | サポート パッケージが配置される、Path で指定した場所の下のディレクトリを指定するために使用します。 既定値は [デバイス名]-[現在の日時 (yyyy-MM-dd-HH-mm-ss)] です。       |
| 6      | Scope                | 省略可能          | として指定 **クラスター** (既定値) の両方のコント ローラー サポート パッケージを作成します。 現在のコント ローラーのみのパッケージを作成する場合は、指定 **コント ローラー**します。 |


## サポート パッケージを編集する

サポート パッケージを生成した後、パッケージを編集して、ボリューム名、デバイスの IP アドレス、バックアップ名などのユーザー固有の情報をログ ファイルから削除することが必要になる場合があります。 

> [AZURE.IMPORTANT] StorSimple 用 Windows PowerShell を使って生成されたサポート パッケージのみ編集できます。 Azure クラシック ポータルで StorSimple Manager サービスを使用して作成したパッケージを編集することはできません。 

サポート パッケージを Microsoft サポート サイトにアップロードする前に編集するには、サポート パッケージの暗号化を解除し、ファイルを編集して、もう一度暗号化する必要があります。 サポート パッケージを編集するには、次の手順を実行します。

#### StorSimple 用 Windows PowerShell でサポート パッケージを編集するには

1. サポート パッケージの作成」の説明に従って [StorSimple 用 Windows PowerShell でサポート パッケージを作成](#create-a-support-package-in-windows-powershell-for-storsimple)します。

2. [スクリプトをダウンロード](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) クライアントのローカルにします。

3. Windows PowerShell モジュールをインポートします。 スクリプトをダウンロードしたローカル フォルダーへのパスを指定する必要があります。 モジュールをインポートするには、次のコマンドを入力します。
 
    `Import-module <Path to the folder that contains the Windows PowerShell script>`

4. サポート パッケージ フォルダーを開きます。 すべてのファイルがメモ *.aes* ファイルの圧縮し、暗号化します。 ファイルを開きます。 ファイルを開くには、次のコマンドを入力します。

    `Open-HcsSupportPackage <Path to the folder that contains support package files>`

    これにより、ファイルが展開され、暗号化が解除されます。 すべてのファイルに実際のファイルの拡張子が表示されます。
    
    ![サポート パッケージの編集 3](./media/storsimple-create-manage-support-package/IC750706.png)


5. 暗号化パスフレーズの入力を求められたら、サポート パッケージの作成時に使用したパスフレーズを入力します。

        cmdlet Open-HcsSupportPackage at command pipeline position 1
    
        Supply values for the following parameters:EncryptionPassphrase: ****
    
6. ログ ファイルが格納されているフォルダーに移動します。 ログ ファイルが展開され、暗号化が解除されているので、元のファイル拡張子が表示されます。 これらのファイルを変更し、ユーザー固有の情報 (ボリューム名やデバイスの IP アドレスなど) があれば削除して、ファイルを保存します。

7. ファイルを閉じます。 ファイルを閉じると、ファイルは Gzip で圧縮され、AES-256 を使用して暗号化されます。 これは、サポート パッケージをネットワーク経由で転送するときのセキュリティと速度のためです。 ファイルを閉じるには、次のコマンドを入力します。

    `Close-HcsSupportPackage <Path to the folder that contains support package files>`

    ![サポート パッケージの編集 2](./media/storsimple-create-manage-support-package/IC750707.png)

8. 入力を求められたら、変更したサポート パッケージの暗号化パスフレーズを入力します。

        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for the following parameters:EncryptionPassphrase: ****

9. 要求されたときに Microsoft サポートと共有できるように、新しいパスフレーズを書き留めます。


### 例: パスワードで保護された共有でサポート パッケージ内のファイルを編集する

サポート パッケージを暗号化解除、編集、再暗号化する方法の例は次のとおりです。

![サポート パッケージの編集 1](./media/storsimple-create-manage-support-package/IC750708.png)

        PS C:\WINDOWS\system32> Import-module C:\Users\Default\StorSimple\SupportPackage\HCSSupportPackageTools.psm1
    
        PS C:\WINDOWS\system32> Open-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw
    
        cmdlet Open-HcsSupportPackage at command pipeline position 1
    
        Supply values for the following parameters:
    
        EncryptionPassphrase: ****
    
        PS C:\WINDOWS\system32> Close-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw
    
        cmdlet Close-HcsSupportPackage at command pipeline position 1
    
        Supply values for the following parameters:
    
        EncryptionPassphrase: ****
    
        PS C:\WINDOWS\system32>

## 次のステップ

- 学習方法 [デバイスのデプロイをトラブルシューティングするサポート パッケージとデバイスのログを使用して](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting)します。 
- 学習方法 [StorSimple Manager サービスを使用して、StorSimple デバイスを管理する](storsimple-manager-service-administration.md)です。



