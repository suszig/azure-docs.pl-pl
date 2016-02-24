### 接続文字列の取得
使用することができます、 **CloudStorageAccount** を表す型 
ストレージ アカウント情報を表すことができます。 Azure プロジェクト テンプレートを 
使用している場合や、
Microsoft.WindowsAzure.CloudConfigurationManager 名前空間への参照がある場合は、 
使用できる、 **CloudConfigurationManager** 型
を使用して Azure サービス構成からストレージ接続文字列とストレージ アカウント
情報を取得できます。

    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

アプリケーションを作成している、Microsoft.WindowsAzure.CloudConfigurationManager への参照にあり、接続文字列が内にある場合、 `web.config` または `app.config` 前に示したとおりを実行して **ConfigurationManager** 接続文字列を取得します。  System.Configuration.dll への参照をプロジェクトに追加し、対応する名前空間宣言を追加する必要があります。

    using System.Configuration;
    ...
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        ConfigurationManager.ConnectionStrings["StorageConnectionString"].ConnectionString);

