<properties 
    pageTitle="弾力性データベース ジョブ ツールのアンインストール方法" 
    description="弾力性データベース ジョブ ツールのアンインストール方法" 
    services="sql-database" 
    documentationCenter="" 
    manager="jeffreyg" 
    authors="sidneyh" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/01/2015" 
    ms.author="ddove; sidneyh"/>

#エラスティック データベース ジョブ コンポーネントのアンインストール
**弾力性データベース ジョブ** ポータルまたは PowerShell を使用してコンポーネントをアンインストールすることができます。

##Azure ポータルを使用してエラスティック データベース ジョブ コンポーネントをアンインストールする

1. 開いている、 [Azure ポータル](https://ms.portal.azure.com/)します。
2. 含むサブスクリプションに移動 **弾力性データベース ジョブ** コンポーネント、つまり、弾力性データベース ジョブ コンポーネントがインストールされて、サブスクリプションです。
3. クリックして **参照** ] をクリック **リソース グループ**します。
4. "__ElasticDatabaseJob" という名前のリソース グループを選択します。
5. リソース グループを削除します。

##PowerShell を使用して、弾力性データベース ジョブ コンポーネントをアンインストールします。

1.  Microsoft Azure PowerShell コマンド ウィンドウを開き、cd tools と入力して、Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x フォルダー以下の tools サブディレクトリに移動します。

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

2.  .\UninstallElasticDatabaseJobs.ps1 PowerShell スクリプトを実行します。

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\UninstallElasticDatabaseJobs.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\UninstallElasticDatabaseJobs.ps1

または、単純に、コンポーネントのインストール時に使用された既定値があるという想定で、次のスクリプトを実行します。

        $ResourceGroupName = "__ElasticDatabaseJob"
        Switch-AzureMode AzureResourceManager
        
        $resourceGroup = Get-AzureResourceGroup -Name $ResourceGroupName
        if(!$resourceGroup)
        {
            Write-Host "The Azure Resource Group: $ResourceGroupName has already been deleted.  Elastic database job components are uninstalled."
            return
        }
        
        Write-Host "Removing the Azure Resource Group: $ResourceGroupName.  This may take a few minutes.”
        Remove-AzureResourceGroup -Name $ResourceGroupName -Force
        Write-Host "Completed removing the Azure Resource Group: $ResourceGroupName.  Elastic database job compoennts are now uninstalled."

## 次のステップ

弾力性データベース ジョブを再インストールするには、次を参照してください [弾力性データベース ジョブ サービスのインストール。](sql-database-elastic-jobs-service-installation.md)

弾力性データベース ジョブの概要については、次を参照してください。 [弾力性データベース ジョブの概要](sql-database-elastic-jobs-overview.md)します。

<!--Image references-->
[1]: ./media/sql-database-elastic-job-uninstall/
 

