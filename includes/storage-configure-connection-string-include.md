## ストレージ接続文字列の設定

Azure .NET 用ストレージ クライアント ライブラリでは、ストレージ接続文字列を使用して、ストレージ サービスにアクセスするためのエンドポイントおよび資格情報を構成できます。 ストレージ接続文字列は、アプリケーション内にハードコーディングするのではなく、構成ファイルの中に保持することをお勧めします。 接続文字列の保存には、次の 2 つのオプションがあります。

- アプリケーションを Azure クラウド サービスで実行する場合は、Azure サービス構成システムを使用して、接続文字列を保存 (`*.csdef` と `*.cscfg` ファイル)。 参照してください [を作成して、クラウド サービスをデプロイする方法](../articles/cloud-services/cloud-services-how-to-create-deploy.md) の詳細については Azure クラウド サービスの構成。
- アプリケーションが Azure の仮想マシンで実行する場合、または Azure の外部で実行される .NET アプリケーションを構築している場合は、保存、.NET 構成システムを使用して、接続文字列 (例: `web.config` または `app.config` ファイル)。

後で、コードから接続文字列を取得する方法について説明します。

### Azure クラウド サービスからの接続文字列の構成

Azure クラウド サービスのサービス構成メカニズムは特有のものであり、これを使用すると、アプリケーションを再デプロイすることなく Azure の管理ポータルから構成設定を動的に変更できます。

Azure サービス構成で接続文字列を構成するには:

1.  Visual Studio のソリューション エクスプ ローラー内で、 **ロール**
    フォルダーから
    web ロールまたはワーカー ロールとクリック **プロパティ**します。  
    ![Visual Studio で、クラウド サービス ロールのプロパティを選択します。][connection-string1]

2.  をクリックして、 **設定** ] タブをクリックし、キーを押して、 **設定の追加** ] ボタンをクリックします。  
    ![Visual Studio でのクラウド サービス設定の追加][connection-string2]

    新しい **[Setting1]** エントリが設定グリッドに表示されます。

3.  **型** 新しいドロップダウン **Setting1** エントリを選択
    **接続文字列**します。  
    ![接続文字列の種類の設定][connection-string3]

4.  **[Setting1]** エントリの右端にある **[...]** をクリックします。
    **[ストレージ アカウント接続文字列]** ダイアログが開きます。

5.  (Microsoft ストレージ エミュレーターをターゲットとするかどうかを選択します。
    Microsoft Azure ストレージ) をターゲットにするか、それともクラウド内のストレージ アカウントをターゲットにするかを
    選択します。 このガイドに記載されているコードは、どちらのオプションにも対応しています。
    オプションを使用してテーブルを作成する必要があります。
    > [AZURE.NOTE] ストレージ エミュレーターをターゲットにすると、Azure Storage に関連する利用料金の発生を回避できます。 ただし、クラウド内の Azure ストレージ アカウントをターゲットとしても、このチュートリアルを実行するための利用料金はごくわずかです。

    クラウドのストレージ アカウントをターゲットにする場合は、そのストレージ アカウントのプライマリ アクセス キー入力します。 Azure 管理ポータルを使用してプライマリ アクセス キーをコピーする方法については、次を参照してください。 [表示、コピーおよび再生成するストレージ アクセス キー](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)します。
    > [AZURE.NOTE] ストレージ アカウント キーは、ストレージ アカウントの root パスワードに似ています。 キーは必ず保護してください。 キーを他のユーザーに配布したり、他のユーザーがアクセスできるプレーン テキスト ファイルに保存したりしないでください。 キーが侵害されたと思われる場合は、管理ポータルを使用してキーを再生成します。

    ![ターゲット環境を選択][connection-string4]

6.  エントリの変更 **名** から **Setting1** のわかりやすい名前を
    ような **StorageConnectionString**します。 この接続文字列は、
    このガイドの後半のコードで参照します。  
    ![接続文字列名の変更][connection-string5]

### .NET 構成を使用した接続文字列の構成

に、Azure クラウド サービスではない (前のセクションを参照してください)、アプリケーションを作成する場合は、.NET 構成システムの使用をお勧め (例: `web.config` または `app.config`)。これには、Azure Websites や Azure Virtual Machines のほか、Azure の外部で実行されるアプリケーションも含まれます。使用して接続文字列を格納する、 `< appSettings >` 要素として次のとおりです。置換 `アカウント名` 、ストレージ アカウントの名前と `アカウント キー` をアカウント アクセス キー。

    <configuration>
        <appSettings>
            <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key" />
        </appSettings>
    </configuration>

たとえば、構成ファイルの構成設定は次のようになります。

    <configuration>
        <appSettings>
            <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=nYV0gln9fT7bvY+rxu2iWAEyzPNITGkhM88J8HUoyofpK7C8fHcZc2kIZp6cKgYRUM74lHI84L50Iau1+9hPjB==" />
        </appSettings>
    </configuration>

これで、このガイドのハウツー タスクを実行する準備が整いました。


[connection-string1]: ./media/storage-configure-connection-string-include/connection-string1.png 
[connection-string2]: ./media/storage-configure-connection-string-include/connection-string2.png 
[connection-string3]: ./media/storage-configure-connection-string-include/connection-string3.png 
[connection-string4]: ./media/storage-configure-connection-string-include/connection-string4.png 
[connection-string5]: ./media/storage-configure-connection-string-include/connection-string5.png 
[configuring connection strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx 

