次のステップに従って、MongoDB を Windows Server が実行されている仮想マシンにインストールして実行します。

> [AZURE.IMPORTANT] 認証、IP アドレス バインドなどの MongoDB セキュリティ機能は、既定では無効です。 MongoDB を運用環境に展開する前に、セキュリティ機能を有効にすることをお勧めします。  参照してください [セキュリティと認証](http://www.mongodb.org/display/DOCS/Security+and+Authentication) の詳細。

1. にリモート デスクトップを使用して仮想マシンに接続した後は、から Internet Explorer を開き、 **開始** ] メニューの [仮想マシンにします。

2. 選択、 **ツール** 画面の右上隅のボタンをクリックします。   **インターネット オプション**, を選択、 **セキュリティ** タブをクリックし、選択、 **信頼済みサイト** ] アイコンをクリックして、 **サイト** ] ボタンをクリックします。 追加 _http://\*.mongodb.org_ 信頼済みサイトの一覧にします。

3. 移動して [MongoDB のダウンロード] [MongoDownloads]します。

4. 検索、 **Current Stable Release**, 、最新の選択 **64 ビット** Windows] 列でバージョンをダウンロードして、MSI インストーラーを実行します。

5. MongoDB は、通常は C:\Program Files\MongoDB にインストールされます。 デスクトップで環境変数を検索し、MongoDB のバイナリ パスを PATH 変数に追加します。 たとえば、バイナリはコンピューターの C:\Program Files\MongoDB\Server\3.0\bin に入っている可能性があります。

6. MongoDB データとログ ディレクトリを作成するデータ ディスク (ドライブ **f:**, など) 前の手順で作成しました。  **開始**, [ **コマンド プロンプト** コマンド プロンプト ウィンドウを開きます。  次のコマンドを入力します。

        C:\> F:
        F:\> mkdir \MongoData
        F:\> mkdir \MongoLogs

7. データベースを実行するには、次を実行します。

        F:\> C:
        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log

    すべてのログ メッセージが表示されます、 *F:\MongoLogs\mongolog.log* mongod.exe サーバーが起動し、事前のジャーナル ファイルに割り当てるファイルです。 MongoDB がジャーナル ファイルを事前に割り当てて、接続のリッスンを開始するには、数分かかる場合があります。

8. MongoDB 管理シェルを開始するから別のコマンド ウィンドウを開く **開始** と入力します。

        C:\> cd \my_mongo_dir\bin  
        C:\my_mongo_dir\bin> mongo  
        >db  
        test
        > db.foo.insert( { a : 1 } )  
        > db.foo.find()  
        { _id : ..., a : 1 }  
        > show dbs  
        ...  
        > show collections  
        ...  
        > help  

    データベースは挿入によって作成されます。

9. または、mongod.exe をサービスとしてインストールできます。

        C:\mongodb\bin>mongod --logpath F:\MongoLogs\mongolog.log --logappend --dbpath F:\MongoData\ --install

    これにより、"Mongo DB" という名前で、説明に "Mongo DB" が指定されたサービスが作成されます。  **--Logpath** オプションを使用して、実行中のサービスには出力を表示するコマンド ウィンドウがあるないので、ログ ファイルを指定する必要があります。   **--Logappend** オプションを指定するサービスを再起動すると出力が既存のログ ファイルに追加します。   **--Dbpath** オプションは、データ ディレクトリの場所を指定します。 他のサービス関連のコマンド ライン オプションを参照してください。 [サービス関連のコマンド ライン オプション] [MongoWindowsSvcOptions]します。

    サービスを開始するには、次のコマンドを実行します。

        C:\mongodb\bin>net start MongoDB

10. これで MongoDB がインストールされました。この MongoDB は現在実行されています。次は MongoDB にリモート接続するために、Windows ファイアウォールのポートを開く必要があります。   **開始** メニューの [ **管理者ツール** し **セキュリティが強化された Windows ファイアウォール**します。

11. 左側のウィンドウで [ **受信の規則**します。   **アクション** ] ウィンドウで、選択 **新しい規則]**します。

    ![Windows Firewall][Image1]

     **新しい受信の規則ウィザード**, [ **ポート** 順にクリック **次**します。

    ![Windows Firewall][Image2]

    選択 **TCP** し **特定のローカル ポート**します。  「27017」(MongoDB がリッスン既定のポート) ポートを指定し、、クリック **次**します。

    ![Windows Firewall][Image3]

    選択 **接続を許可する** ] をクリック **次**します。

    ![Windows Firewall][Image4]

    クリックして **次** 再度します。

    ![Windows Firewall][Image5]

    "MongoPort"など、ルールの名前を指定し、クリックして **完了**します。

    ![Windows Firewall][Image6]

12. 仮想マシンを作成したときに MongoDB のエンドポイントを構成しなかった場合は、ここで構成できます。 ファイアウォール ルールとエンドポイントの両方が MongoDB にリモートで接続できる必要があります。 管理ポータルで、クリックして **仮想マシン**, を新しい仮想マシンの名前をクリックして、クリックして **エンドポイント**します。

    ![エンドポイント][Image7]

13. クリックして **追加** ページの下部にあります。 選択 **スタンドアロン エンドポイントの追加** ] をクリック **次**します。

    ![エンドポイント][Image8]

14. 名前を「Mongo」、プロトコル エンドポイントを追加 **TCP**, 、および **パブリック** と **プライベート** ポートは、「27017」に設定します。 これにより、MongoDB へのリモート アクセスが可能になります。

    ![エンドポイント][Image9]

> [AZURE.NOTE] ポート 27017 は MongoDB によって使用される既定のポートです。 これを変更することができます、 _--ポート_ サブコマンド mongod.exe サーバーの起動時にします。 ファイアウォールでは、上記の手順の「Mongo」エンドポイントと同じポート番号を指定してください。


[MongoDownloads]: http://www.mongodb.org/downloads

[MongoWindowsSvcOptions]: http://www.mongodb.org/display/DOCS/Windows+Service


[Image1]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall1.png
[Image2]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall2.png
[Image3]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall3.png
[Image4]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall4.png
[Image5]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall5.png
[Image6]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall6.png
[Image7]: ./media/install-and-run-mongo-on-win2k8-vm/WinVmAddEndpoint.png
[Image8]: ./media/install-and-run-mongo-on-win2k8-vm/WinVmAddEndpoint2.png
[Image9]: ./media/install-and-run-mongo-on-win2k8-vm/WinVmAddEndpoint3.png

