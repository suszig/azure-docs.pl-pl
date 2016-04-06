<properties 
    pageTitle="エラスティック データベース ジョブのインストールの概要 | Microsoft Azure" 
    description="弾力性ジョブの機能のインストールについて説明します。" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/04/2015" 
    ms.author="ddove;sidneyh"/>

# Elastic Database ジョブのインストールの概要
[**弾力性データベース ジョブ**](sql-database-elastic-jobs-overview.md) Azure クラシック Portal.You を通じてアクセスできる PowerShell パッケージをインストールする場合にのみ、PowerShell API を使用してジョブを作成および管理または PowerShell を使用してをインストールできます。 さらに現在のところ、PowerShell API はポータルよりもはるかに多数の機能を使用できます。 

既にインストールされている場合 **弾力性データベース ジョブ** 、既存のポータルを通じて **弾力性データベース プール**, 、最新の Powershell preview には、既存のインストールをアップグレードするためのスクリプトが含まれています。 最新のバージョンにアップグレードすることお勧め **弾力性データベース ジョブ** PowerShell Api 経由で公開されている新しい機能を活用するためにコンポーネントです。

## 前提条件
* Azure サブスクリプション。 無料評価版サイトを参照してください。 [無料評価版](http://azure.microsoft.com/pricing/free-trial/)します。
* Azure PowerShell Version 0.8.16 以降。 最新バージョン (0.9.5) をインストール、 [Web Platform Installer](http://go.microsoft.com/fwlink/p/?linkid=320376)します。 詳細については、次を参照してください。 [をインストールして、Azure PowerShell を構成する方法](powershell-install-configure.md)します。
* [NuGet コマンド ライン ユーティリティ](https://nuget.org/nuget.exe) 弾力性データベース ジョブのパッケージをインストールするために使用します。 詳細については、http://docs.nuget.org/docs/start-here/installing-nuget を参照してください。

## Elastic Database ジョブ PowerShell パッケージをダウンロードしてインストールする
1. Microsoft Azure PowerShell コマンド ウィンドウを開き、NuGet Command-line Utility (nuget.exe) をダウンロードしたディレクトリに移動します。

2. ダウンロードしてインポート **弾力性データベース ジョブ** 次のコマンドを使用して、現在のディレクトリにパッケージします。

        PS C:\>.\nuget install Microsoft.Azure.SqlDatabase.Jobs -prerelease

     **弾力性データベース ジョブ** という名前のフォルダー内のローカル ディレクトリにファイルが配置される **Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x** 、 *x.x.xxxx.x* バージョン番号が反映されます。 (必要な .dll を含む)、PowerShell コマンドレットが内にある、 **tools\ElasticDatabaseJobs** サブディレクトリとインストール、アップグレードおよびアンインストールするには、PowerShell スクリプトにも配置、 **ツール** サブディレクトリ。

3. 次のように cd tools と入力して、Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x 以下の tools サブディレクトリに移動します。

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

4.  .\InstallElasticDatabaseJobsCmdlets.ps1 スクリプトを実行して、ElasticDatabaseJobs ディレクトリを $home\Documents\WindowsPowerShell\Modules にコピーします。 この処理で、次のように使用するモジュールも自動的にインポートされます。

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobsCmdlets.ps1 
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobsCmdlets.ps1

## PowerShell を使用して Elastic Database ジョブ コンポーネントをインストールする
1.  Microsoft Azure PowerShell コマンド ウィンドウを開き、cd \tools と入力して、Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x フォルダー以下の \tools サブディレクトリに移動します。

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

2.  .\InstallElasticDatabaseJobs.ps1 PowerShell スクリプトを実行し、要求された変数に値を設定します。 このスクリプトで説明するコンポーネントによって作成されます [弾力性データベース ジョブ コンポーネントと価格](sql-database-elastic-jobs-overview/#components-and-pricing) 依存コンポーネントを適切に使用する Azure のクラウド サービスを構成するとします。

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1 
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobs.ps1

入力を求めるこのコマンドを実行すると、ウィンドウが開き、 **ユーザー名** と **パスワード**します。 ここで入力するユーザー名とパスワードは Azure の資格情報ではなく、新しいサーバーの管理者の資格情報となります 

この呼び出し例で指定したパラメーターは、実際の設定に合わせて変更できます。 次に各パラメーターの動作の詳細について説明します。

<table style="width:100%">
  <tr>
    <th>パラメーター</th>
    <th>説明</th>
  </tr>

<tr>
    <td>ResourceGroupName</td>
    <td>新しく作成された Azure コンポーネントを含めるために作成する Azure リソース グループ名を指定します。 このパラメーターの既定値は “__ElasticDatabaseJob” です。 この値は変更しないことをお勧めします。</td>
    </tr>

</tr>

    <tr>
    <td>ResourceGroupLocation</td>
    <td>Provides the Azure location to be used for the newly created Azure components. This parameter defaults to the Central US location.</td>
</tr>

<tr>
    <td>ServiceWorkerCount</td>
    <td>インストールするサービス ワーカーの数を指定します。 このパラメーターの既定値は 1 です。 ワーカー数を多くすると、サービスをスケール アウトし、高可用性を実現できます。 サービスの高可用性が必要なデプロイメントの場合、"2" を使用することをお勧めします。</td>
    </tr>

</tr>
    <tr>
    <td>ServiceVmSize</td>
    <td>クラウド サービス内で使用する VM サイズを指定します。 このパラメーターの既定値は A0 です。 A0/A1/A2/A3 のパラメーター値を指定できます。それぞれ、worker ロールに ExtraSmall/Small/Medium/Large サイズが使用されます。 Fo ワーカー ロールのサイズの詳細についてを参照してください [Elastic Database jobs components and pricing](sql-database-elastic-jobs-overview/#components-and-pricing)します。</td>
</tr>

</tr>
    <tr>
    <td>SqlServerDatabaseSlo</td>
    <td>Standard エディションのサービス レベル目標を指定します。 このパラメーターの既定値は S0 です。 S0/S1/S2/S3 のパラメーター値を指定できます。Azure SQL Database で各 SLO が使用されます。 SQL データベースの Slo の詳細については、次を参照してください。 [Elastic Database jobs components and pricing](sql-database-elastic-jobs-overview/#components-and-pricing)します。</td>
</tr>

</tr>
    <tr>
    <td>SqlServerAdministratorUserName</td>
    <td>新しく作成した Azure SQL Database サーバーの管理ユーザー名を指定します。 指定しない場合、PowerShell の資格情報ウィンドウが開き、資格情報の入力を求められます。</td>
</tr>

</tr>
    <tr>
    <td>SqlServerAdministratorPassword</td>
    <td>新しく作成した Azure SQL Database サーバーの管理パスワードを指定します。 指定しない場合、PowerShell の資格情報ウィンドウが開き、資格情報の入力を求められます。</td>
</tr>
</table>

多数のデータベースに対して同時に実行されるジョブ数が多数あるシステムの場合、-ServiceWorkerCount 2 -ServiceVmSize A2 -SqlServerDatabaseSlo S2 などのパラメーターを指定することをお勧めします。

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\InstallElasticDatabaseJobs.ps1 -ServiceWorkerCount 2 -ServiceVmSize A2 -SqlServerDatabaseSlo S2

## PowerShell を使用して既存の Elastic Database ベース ジョブ コンポーネントのインストールを更新する

**弾力性データベース ジョブ** 拡張性と高可用性のための既存のインストール内で更新することができます。 このプロセスは、将来的に、管理データベースを削除して再作成することなく、サービス コードをアップグレードすること見込んでいます。 また、同じバージョン内でこのプロセスを使用して、サービス VM サイズまたはサーバーのワーカー数を変更できます。

インストールの VM サイズを更新するには、パラメーターを選択した値に更新して、次のスクリプトを実行します。

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\UpdateElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\UpdateElasticDatabaseJobs.ps1 -ServiceVmSize A1 -ServiceWorkerCount 2

<table style="width:100%">
  <tr>
  <th>パラメーター</th>
  <th>説明</th>
</tr>

  <tr>
    <td>ResourceGroupName</td>
    <td>Elastic Database ジョブ コンポーネントを最初にインストールしたときに使用した Azure リソース グループ名を指定します。 このパラメーターの既定値は “__ElasticDatabaseJob” です。 この値を変更することは推奨されないので、このパラメーターを指定する必要はありません。</td>
    </tr>
</tr>

</tr>

  <tr>
    <td>ServiceWorkerCount</td>
    <td>インストールするサービス ワーカーの数を指定します。  このパラメーターの既定値は 1 です。  ワーカー数を多くすると、サービスをスケール アウトし、高可用性を実現できます。  サービスの高可用性が必要なデプロイメントの場合、"2" を使用することをお勧めします。</td>
</tr>

</tr>

    <tr>
    <td>ServiceVmSize</td>
    <td>Provides the VM size for usage within the Cloud Service. This parameter defaults to A0. Parameters values of A0/A1/A2/A3 are accepted which cause the worker role to use an ExtraSmall/Small/Medium/Large size, respectively. Fo more information on worker role sizes, see [Elastic Database jobs components and pricing](sql-database-elastic-jobs-overview/#components-and-pricing).</td>
</tr>

</table>

## ポータルを使用して Elastic Database ジョブ コンポーネントをインストールする

取得したら [弾力性データベース プールを作成](sql-database-elastic-pool-portal.md), 、インストールすることができます **弾力性データベース ジョブ** 弾力性データベース プール内の各データベースに対して管理タスクの実行を有効にするコンポーネントです。 場合と異なりを使用して、 **弾力性データベース ジョブ** PowerShell Api では、ポータルのインターフェイスは、既存のプールに対して実行するだけに制限されています。


**推定所要時間:** 10 分です。

1. 使用して、エラスティック データベース プールのダッシュ ボードの表示から、 [Azure ポータル](https://ms.portal.azure.com/#) , 、クリックして **ジョブの作成**します。
2. インストールする必要があります、最初に、ジョブを作成する場合 **弾力性データベース ジョブ** 分断 **プレビューに関する条件**します。 
3. チェック ボックスをクリックして条項に同意します。
4. サービスのインストール] ビューで、クリックして **ジョブの資格情報**します。

    ![サービスのインストール][1]

5. データベース管理者のユーザー名とパスワードを入力します。 インストールの一環として、新しい Azure SQL Database サーバーが作成されます。 この新しいサーバー内に、管理データベースと呼ばれる新しいデータベースが作成され、Elastic Database ジョブのメタデータの格納に使用されます。 ここで作成されるユーザー名とパスワードは、管理データベースにログインするために使用されます。 プール内のデータベースに対するスクリプト実行には、別の資格情報が使用されます。

    ![ユーザー名とパスワードの作成][2]

6. [OK] ボタンをクリックします。 新しい、数分後にコンポーネントの作成には [リソース グループ](../resource-group-portal.md)します。 下記に示すように、新しいリソース グループがスタート ボードに固定表示されています。 作成されると、弾力性データベース ジョブ (クラウド サービス、SQL Database、Service Bus、ストレージ記憶域) は、すべてグループ内に作成されます。

    ![スタート ボードのリソース グループ][3]

7. 作成またはエラスティック データベース ジョブが提供するときにインストールするときに、ジョブを管理しようとすると **資格情報** 、次のメッセージが表示されます。 

    ![実行中のデプロイメント][4]

アンインストールが必要な場合は、リソース グループを削除します。 参照してください [弾力性データベース ジョブ コンポーネントのアンインストール方法](sql-database-elastic-jobs-uninstall.md)します。

## 次のステップ

スクリプトの実行が参照先のグループ内の各データベースで作成したは、適切な権限を持つ資格情報を確認 [データベースの [グループ内のすべてのデータベースにユーザーを追加する方法](sql-database-elastic-jobs-add-logins-to-dbs.md)します。 
参照してください [の作成と管理、弾力性データベース ジョブ](sql-database-elastic-jobs-create-and-manage.md) 開始します。

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-service-installation/screen-1.png
[2]: ./media/sql-database-elastic-jobs-service-installation/credentials.png
[3]: ./media/sql-database-elastic-jobs-service-installation/start-board.png
[4]: ./media/sql-database-elastic-jobs-service-installation/incomplete.png
 

