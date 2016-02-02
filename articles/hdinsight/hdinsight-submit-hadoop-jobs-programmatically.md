<properties
    pageTitle="HDInsight での Hadoop ジョブの送信 | Microsoft Azure"
    description="Hadoop ジョブを Azure HDInsight Hadoop へ送信する方法について説明します。"
    editor="cgronlun"
    manager="paulettm"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/30/2015"
    ms.author="jgao"/>


# HDInsight での Hadoop ジョブの送信

Azure PowerShell を使用して MapReduce、と Hive ジョブを送信する方法と、HDInsight .NET SDK を使用して、MapReduce、Hadoop ストリーミング、Hive ジョブを送信する方法について説明します。
> [AZURE.NOTE] この記事の手順は、Windows クライアントから実行する必要があります。 Linux、OS X、または Unix クライアントを使用して、HDInsight で MapReduce、Hive、または Pig を操作する方法の詳細については、次の記事を参照し、**SSH** または **Curl** のリンクを選択してください。
>
> - [HDInsight での Hive の使用](hdinsight-use-hive.md)
> - [HDInsight での Pig の使用](hdinsight-use-pig.md)
> - [HDInsight での MapReduce の使用](hdinsight-use-mapreduce.md)

## 前提条件

この記事を読み始める前に、次の項目を用意する必要があります。

- **Azure HDInsight クラスター**。 手順については、次を参照してください。 [[hdinsight の入門] HDInsight の概要][hdinsight-get-started] または [HDInsight ][hdinsight-provision]します。
- **Azure PowerShell を実行できるワークステーション**。 参照してください [Azure PowerShell 1.0 をインストールし、大きい](hdinsight-administer-use-powershell.md#install-azure-powershell-10-and-greater)します。

## PowerShell を使用して MapReduce ジョブを送信する

参照してください [Windows ベースの HDInsight で Hadoop MapReduce の実行のサンプル](hdinsight-run-samples.md)します。

## PowerShell を使用して Hive ジョブを送信します。

を参照してください [Hive クエリの実行 PowerShell を使用して](hdinsight-hadoop-use-hive-powershell.md)

## Visual Studio を使用して Hive ジョブを送信する

参照してください [使い方 HDInsight Hadoop Tools for Visual Studio の ][hdinsight-visual-studio-tools]します。

## PowerShell を使用して Sqoop ジョブを送信する

参照してください [[hdinsight での sqoop の使用] を HDInsight での Sqoop の使用][hdinsight-use-sqoop]します。

## HDInsight .NET SDK を使用して Hive/Pig/Sqoop ジョブを送信する

HDInsight .NET SDK は、.NET から HDInsight クラスターを簡単に操作できる .NET クライアント ライブラリを提供します。

**ジョブを送信するには**

1. Visual Studio で、C# コンソール アプリケーションを作成します。
2. NuGet パッケージ マネージャー コンソールから、次のコマンドを実行します。

        Install-Package Microsoft.Azure.Common.Authentication -pre
        Install-Package Microsoft.Azure.Management.HDInsight -Pre
        Install-Package Microsoft.Azure.Management.HDInsight.Job -Pre

2. 次のコードを使用します。

     using System;
     using System.Collections.Generic;
     using System.Linq;
     using System.Security;
    
     using Microsoft.Azure;
     using Microsoft.Azure.Common.Authentication;
     using Microsoft.Azure.Common.Authentication.Factories;
     using Microsoft.Azure.Common.Authentication.Models;
     using Microsoft.Azure.Management.HDInsight;
     using Microsoft.Azure.Management.HDInsight.Job;
     using Microsoft.Azure.Management.HDInsight.Job.Models;
     using Hyak.Common;
    
     namespace SubmitHDInsightJobDotNet
     {
         class Program
         {
             private static HDInsightManagementClient _hdiManagementClient;
             private static HDInsightJobManagementClient _hdiJobManagementClient;
    
             private static Guid SubscriptionId = new Guid("<Your Subscription ID>");
             private const string ResourceGroupName = "<Your Resource Group Name>";
    
             private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
             private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
             private const string ExistingClusterUsername = "admin";
             private const string ExistingClusterPassword = "**********";
    
             static void Main(string[] args)
             {
                 System.Console.WriteLine("Running");
    
                 var tokenCreds = GetTokenCloudCredentials();
                 var subCloudCredentials = GetSubscriptionCloudCredentials(tokenCreds, SubscriptionId);
    
                 _hdiManagementClient = new HDInsightManagementClient(subCloudCredentials);
    
                 var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                 _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
    
                 SubmitHiveJob();
                 SubmitPigJob();
                 SubmitSqoopJob();
             }
    
             public static TokenCloudCredentials GetTokenCloudCredentials(string username = null, SecureString password = null)
             {
                 var authFactory = new AuthenticationFactory();
    
                 var account = new AzureAccount { Type = AzureAccount.AccountType.User };
    
                 if (username != null && password != null)
                     account.Id = username;
    
                 var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
    
                 var accessToken =
                     authFactory.Authenticate(account, env, AuthenticationFactory.CommonAdTenant, password, ShowDialog.Auto)
                         .AccessToken;
    
                 return new TokenCloudCredentials(accessToken);
             }
    
             public static SubscriptionCloudCredentials GetSubscriptionCloudCredentials(TokenCloudCredentials creds, Guid subId)
             {
                 return new TokenCloudCredentials(subId.ToString(), creds.Token);
             }
    
             private static void SubmitPigJob()
             {
                 var parameters = new PigJobSubmissionParameters
                 {
                     UserName = ExistingClusterUsername,
                     Query = @"LOGS = LOAD 'wasb:///example/data/sample.log';
                         LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
                         FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
                         GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
                         FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
                         RESULT = order FREQUENCIES by COUNT desc;
                         DUMP RESULT;"
                 };
    
                 System.Console.WriteLine("Submitting the Pig job to the cluster...");
                 var response = _hdiJobManagementClient.JobManagement.SubmitPigJob(parameters);
                 System.Console.WriteLine("Validating that the response is as expected...");
                 System.Console.WriteLine("Response status code is " + response.StatusCode);
                 System.Console.WriteLine("Validating the response object...");
                 System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
             }
    
             private static void SubmitHiveJob()
             {
                 Dictionary<string, string> defines = new Dictionary<string, string> { { "hive.execution.engine", "ravi" }, { "hive.exec.reducers.max", "1" } };
                 List<string> args = new List<string> { { "argA" }, { "argB" } };
                 var parameters = new HiveJobSubmissionParameters
                 {
                     UserName = ExistingClusterUsername,
                     Query = "SHOW TABLES",
                     Defines = ConvertDefinesToString(defines),
                     Arguments = ConvertArgsToString(args)
                 };
    
                 System.Console.WriteLine("Submitting the Hive job to the cluster...");
                 var response = _hdiJobManagementClient.JobManagement.SubmitHiveJob(parameters);
                 System.Console.WriteLine("Validating that the response is as expected...");
                 System.Console.WriteLine("Response status code is " + response.StatusCode);
                 System.Console.WriteLine("Validating the response object...");
                 System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
             }
    
             private static void SubmitSqoopJob()
             {
                 var sqlDatabaseServerName = "<SQLDatabaseServerName>";
                 var sqlDatabaseLogin = "<SQLDatabaseLogin>";
                 var sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>";
                 var sqlDatabaseDatabaseName = "hdisqoop";
    
                 var tableName = "log4jlogs";
                 var exportDir = "/tutorials/usesqoop/data";
    
                 // Connection string for using Azure SQL Database.
                 // Comment if using SQL Server
                 var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ".database.windows.net;user=" + sqlDatabaseLogin + "@" + sqlDatabaseServerName + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
                 // Connection string for using SQL Server.
                 // Uncomment if using SQL Server
                 //var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ";user=" + sqlDatabaseLogin + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
    
                 var parameters = new SqoopJobSubmissionParameters
                 {
                     UserName = ExistingClusterUsername,
                     Command = "export --connect " + connectionString + " --table " + tableName + "_mobile --export-dir " + exportDir + "_mobile --fields-terminated-by \\t -m 1"
                 };
    
                 System.Console.WriteLine("Submitting the Sqoop job to the cluster...");
                 var response = _hdiJobManagementClient.JobManagement.SubmitSqoopJob(parameters);
                 System.Console.WriteLine("Validating that the response is as expected...");
                 System.Console.WriteLine("Response status code is " + response.StatusCode);
                 System.Console.WriteLine("Validating the response object...");
                 System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
             }
    
             private static string ConvertDefinesToString(Dictionary<string, string> defines)
             {
                 if (defines.Count == 0)
                 {
                     return null;
                 }
    
                 return string.Join("&define=", defines.Select(x => x.Key + "%3D" + x.Value).ToArray());
             }
             private static string ConvertArgsToString(List<string> args)
             {
                 if (args.Count == 0)
                 {
                     return null;
                 }
    
                 return string.Join("&arg=", args.ToArray());
             }
         }
     }

5. **F5** キーを押してアプリケーションを実行します。

## Visual Studio の HDInsight ツールを使用してジョブを送信する

Visual Studio の HDInsight ツールを使用して、Hive クエリと Pig スクリプトを実行できます。 参照してください [HDInsight 用の Visual Studio Hadoop tools の使用開始](hdinsight-hadoop-visual-studio-tools-get-started.md)します。

## 次のステップ

この記事では、HDInsight クラスターを作成する方法をいくつか説明しました。 詳細については、次の記事を参照してください。

* [[Hdinsight の入門] Azure HDInsight の概要します。][hdinsight-get-started]
* [[Hdinsight プロビジョニング] HDInsight で Hadoop クラスターを作成します。][hdinsight-provision]
* [PowerShell ][hdinsight-admin-powershell]
* [HDInsight コマンドレット リファレンス ドキュメント ][hdinsight-powershell-reference]
* [[Hdinsight を使用して hive] HDInsight での Hive を使用します。][hdinsight-use-hive]
* [[Hdinsight での pig の使用] を HDInsight での Pig を使用します。][hdinsight-use-pig]



[azure-certificate]: http://msdn.microsoft.com/library/windowsazure/gg551722.aspx 
[azure-management-portal]: https://portal.azure.com/ 
[hdinsight-visual-studio-tools]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md 
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md 
[hdinsight-provision]: hdinsight-provision-clusters.md 
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md 
[hdinsight-use-hive]: hdinsight-use-hive.md 
[hdinsight-use-pig]: hdinsight-use-pig.md 
[hdinsight-get-started]: ../hdinsight-get-started.md 
[hdinsight-storage]: ../hdinsight-use-blob-storage.md 
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md 
[hdinsight-develop-streaming-jobs]: hdinsight-hadoop-develop-deploy-streaming-jobs.md 
[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx 
[powershell-install-configure]: ../install-configure-powershell.md 
[image-hdi-gettingstarted-runmrjob]: ./media/hdinsight-submit-hadoop-jobs-programmatically/HDI.GettingStarted.RunMRJob.png 
[image-hdi-gettingstarted-mrjoboutput]: ./media/hdinsight-submit-hadoop-jobs-programmatically/HDI.GettingStarted.MRJobOutput.png 
[apache-hive]: http://hive.apache.org/ 

