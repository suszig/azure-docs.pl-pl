<properties 
    pageTitle="診断の使用方法 (.NET) | Microsoft Azure" 
    description="Azure で、デバッグ、パフォーマンス測定、監視、トラフィック解析などに診断データを使用する方法を説明します。" 
    services="cloud-services" 
    documentationCenter=".net" 
    authors="rboucher" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/25/2015" 
    ms.author="robb"/>



# Azure のクラウド サービスおよび仮想マシンの診断機能

Azure 診断 1.2 および 1.3 を使用すると、Azure で実行している Worker ロール、Web ロール、または仮想マシンから診断データを収集できます。 このガイドでは、Azure 診断 1.2 および 1.3 の使用方法について説明します。 詳細なガイダンスについては、ログとトレース戦略、問題のトラブルシューティング、診断機能やその他の手法を作成する、次を参照してください。 [Azure アプリケーションの開発に関するトラブルシューティングのベスト プラクティス][]します。

## 概要

Azure 診断 1.2 および 1.3 は、Azure で実行している Worker ロール、Web ロール、または仮想マシンから診断テレメトリを収集できる Azure 拡張機能です。 テレメトリ データは Azure ストレージ アカウントに格納され、デバッグ、トラブルシューティング、パフォーマンス測定、リソース使用状況の監視、トラフィック解析と容量計画、および監査に使用できます。 

Azure 診断 1.2 は、Azure SDK for .NET 2.4 以前で使用します。 Azure 診断 1.3 は、Azure SDK for .NET 2.5 以降で使用します。

診断 1.0 を以前使用したことがある場合は、診断 1.2 および 1.3 と比較して 3 つの大きな違いがあります。

1.  診断 1.2 および 1.3 はクラウド サービスに加えて、仮想マシン上でデプロイできます
2.  診断 1.0 は Azure SDK の一部で、クラウド サービスのデプロイ時にデプロイされます。 診断 1.2 および 1.3 は拡張機能であり、クラウド サービスのデプロイとは別にデプロイされます。
3.  診断 1.2 および 1.3 では、ETW イベントおよび .NET EventSource イベントの収集が可能です。

さらに詳細な比較では、次を参照してください。 [Azure 診断のバージョンを比較する][] この記事の最後にします。

Azure 診断では、次の種類のテレメトリを収集できます。

データ ソース|説明
---|---
IIS ログ|IIS Web サイトに関する情報。
Azure 診断インフラストラクチャ ログ|診断自体に関する情報。
IIS の失敗した要求ログ|IIS サイトまたはアプリケーションへの失敗した要求に関する情報。
Windows イベント ログ|Windows イベント ログ システムに送信された情報。
パフォーマンス カウンター|オペレーティング システムとカスタム パフォーマンス カウンター
クラッシュ ダンプ|アプリケーションがクラッシュした場合のプロセスの状態に関する情報。
カスタム エラー ログ|アプリケーションまたはサービスで作成されたログ。
NET EventSource |.NET EventSource クラスを使用して <a href="http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx">コードで生成されたイベント</a>
マニフェスト ベースの ETW|すべてのプロセスで生成された ETW イベント。

## Worker ロールの診断を有効にする方法

このチュートリアルでは、.NET EventSource クラスを使用してテレメトリ データを生成する Azure Worker ロールの実装方法について説明します。 Azure 診断を使用してテレメトリ データを収集し、これを Azure ストレージ アカウントに格納します。 Worker ロールを作成すると、Visual Studio は Azure SDK for .NET 2.4 以降でソリューションの一部として自動的に診断 1.0 を有効にします。 次の手順では、Worker ロールの作成、ソリューションからの診断、1.0 の無効化、Worker ロールへの診断、1.2 または 1.3 のデプロイに関するプロセスについて説明します。

### 前提条件
この記事では、Azure サブスクリプションがあり、Azure SDK で Visual Studio 2013 を使用していることを前提としています。 サインアップすることができます、Azure サブスクリプションがない、 [無料評価版][]します。 必ず [インストール Azure powershell version 0.8.7 以降][]します。

### 手順 1. worker ロールを作成する
1.  起動 **Visual Studio 2013**します。
2.  新しい **Azure クラウド サービス** プロジェクトから、 **クラウド** を .NET Framework 4.5 を対象とするテンプレートです。  プロジェクト名を「WadExample」と入力し、[OK] をクリックします。
3.  選択 **ワーカー ロール** Ok] をクリックします。 プロジェクトが作成されます。 
4.   **ソリューション エクスプ ローラー**, をダブルクリックして、 **WorkerRole1** プロパティ ファイルです。
5.   **構成** オフをタブ **診断を有効にする** 診断 1.0 (Azure SDK 2.4 以前) を無効にします。
6.  ソリューションを構築してエラーが発生しないことを確認します。

### 手順 2. コードをインストルメント化する
WorkerRole.cs の内容を次のコードに置き換えます。 継承された SampleEventSourceWriter クラス、 [EventSource クラス][], 、4 つのログ記録メソッドを実装する: **SendEnums**, 、**MessageMethod**, 、**SetOther** と **HighFreq**します。 最初のパラメーター、 **WriteEvent** メソッドは、各イベントの ID を定義します。 Run メソッドをそれぞれで実装されているログ記録メソッドを呼び出す無限ループを実装する、 **SampleEventSourceWriter** 10 秒ごとのクラスです。

    using Microsoft.WindowsAzure.ServiceRuntime;
    using System;
    using System.Diagnostics;
    using System.Diagnostics.Tracing;
    using System.Net;
    using System.Threading;

    namespace WorkerRole1
    {
    sealed class SampleEventSourceWriter : EventSource
    {
        public static SampleEventSourceWriter Log = new SampleEventSourceWriter();
        public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); }// Cast enums to int for efficient logging.
        public void MessageMethod(string Message) { if (IsEnabled())  WriteEvent(2, Message); }
        public void SetOther(bool flag, int myInt) { if (IsEnabled())  WriteEvent(3, flag, myInt); }
        public void HighFreq(int value) { if (IsEnabled()) WriteEvent(4, value); }

    }

    enum MyColor
    {
        Red,
        Blue,
        Green
    }

    [Flags]
    enum MyFlags
    {
        Flag1 = 1,
        Flag2 = 2,
        Flag3 = 4
    }

    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
            // This is a sample worker implementation. Replace with your logic.
            Trace.TraceInformation("WorkerRole1 entry point called");

            int value = 0;

            while (true)
            {
                Thread.Sleep(10000);
                Trace.TraceInformation("Working");

                // Emit several events every time we go through the loop
                for (int i = 0; i < 6; i++)
                {
                    SampleEventSourceWriter.Log.SendEnums(MyColor.Blue, MyFlags.Flag2 | MyFlags.Flag3);
                }

                for (int i = 0; i < 3; i++)
                {
                    SampleEventSourceWriter.Log.MessageMethod("This is a message.");
                    SampleEventSourceWriter.Log.SetOther(true, 123456789);
                }

                if (value == int.MaxValue) value = 0;
                SampleEventSourceWriter.Log.HighFreq(value++);
            }
        }

        public override bool OnStart()
        {
            // Set the maximum number of concurrent connections 
            ServicePointManager.DefaultConnectionLimit = 12;

            // For information on handling configuration changes
            // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.

            return base.OnStart();
        }
    }
    }


### 手順 3. worker ロールをデプロイする
1.  選択して、worker ロールを Visual Studio 内から Azure に展開、 **WadExample** プロジェクト、ソリューション エクスプ ローラーで、 **発行** から、 **ビルド** メニュー。
2.  サブスクリプションを選択します。
3.   **Microsoft Azure 発行設定** ダイアログの [ **[新規作成]**します。
4.   **クラウド サービスの作成とストレージ アカウント** ダイアログを入力、 **名前** (「WadExample」など) し、リージョンまたはアフィニティ グループを選択します。
5.  設定、 **環境** に **ステージング**します。
6.  その他の変更 **設定** 、適切な対応済み] をクリックして **発行**します。
7.  展開が完了したら、クラウド サービスが Azure の旧ポータルで確認、 **を実行している** 状態です。

### 手順 4. 診断構成ファイルを作成して拡張機能をインストールする
1.  次の PowerShell コマンドを実行して、パブリック構成ファイルのスキーマ定義をダウンロードします。
2.  
        (Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd' 

2.  XML ファイルを追加、 **WorkerRole1** プロジェクトを右クリックして、 **WorkerRole1** プロジェクトし、選択 **追加** ]-> [ **新しい項目]** ]-> [ **Visual c# アイテム** ]-> [ **データ** ]-> [ **XML ファイル**します。 ファイルに「WadExample.xml」という名前を付けます。

    ![CloudServices_diag_add_xml](./media/cloud-services-dotnet-diagnostics/AddXmlFile.png)

3.  構成ファイルに WadConfig.xsd を関連付けます。 WadExample.xml エディター ウィンドウがアクティブになっていることを確認します。 キーを押して **f4 キーを押して** を開くには、 **プロパティ** ウィンドウです。 をクリックして、 **スキーマ** プロパティに、 **プロパティ** ウィンドウです。 クリックして、 **...** で、 **スキーマ** プロパティです。 クリックして、 **追加…** ] ボタンをクリックし、XSD ファイルを保存した場所に移動し、WadConfig.xsd ファイルを選択します。 Click **OK**.
4.  WadExample.xml 構成ファイルの内容を次の XML に置き換え、ファイルを保存します。 この構成ファイルは、収集するいくつかのパフォーマンス カウンターを定義します。1 つは CPU 使用率、1 つはメモリ使用率です。 次に、SampleEventSourceWriter クラスのメソッドに対応する 4 つのイベントを定義します。

```
        <?xml version="1.0" encoding="utf-8"?>
        <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
            <WadCfg>
                <DiagnosticMonitorConfiguration overallQuotaInMB="25000">
                <PerformanceCounters scheduledTransferPeriod="PT1M">
                    <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />
                    <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT1M" unit="bytes"/>
                    </PerformanceCounters>
                    <EtwProviders>
                        <EtwEventSourceProviderConfiguration provider="SampleEventSourceWriter" scheduledTransferPeriod="PT5M">
                            <Event id="1" eventDestination="EnumsTable"/>
                            <Event id="2" eventDestination="MessageTable"/>
                            <Event id="3" eventDestination="SetOtherTable"/>
                            <Event id="4" eventDestination="HighFreqTable"/>
                            <DefaultEvents eventDestination="DefaultTable" />
                        </EtwEventSourceProviderConfiguration>
                    </EtwProviders>
                </DiagnosticMonitorConfiguration>
            </WadCfg>
        </PublicConfig>
```

### 手順 5. worker ロールに診断をインストールする
Web ロールまたは Worker ロールの診断を管理する PowerShell コマンドレットは、Set-AzureServiceDiagnosticsExtension、Get-AzureServiceDiagnosticsExtension、Remove-AzureServiceDiagnosticsExtension です。

1.  Azure PowerShell を開きます。
2.  ワーカー ロールで診断をインストールするスクリプトを実行 (置換 *StorageAccountKey* を wadexample ストレージ アカウントのストレージ アカウント キー)。

        $storage_name = "wadexample"
        $key = "<StorageAccountKey>"
        $config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExample\WorkerRole1\WadExample.xml"
        $service_name="wadexample"
        $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key 
        Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Staging -Role WorkerRole1


### 手順 6. テレメトリ データを確認する
Visual Studio で **サーバー エクスプ ローラー** wadexample ストレージ アカウントに移動します。 クラウド サービスの実行は約 5 分後に、テーブルが表示されます **WADEnumsTable**, 、**WADHighFreqTable**, 、**WADMessageTable**, 、**WADPerformanceCountersTable** と **WADSetOtherTable**します。 いずれかのテーブルをダブルクリックして、収集した利用統計情報を表示します。
    ![CloudServices_diag_tables](./media/cloud-services-dotnet-diagnostics/WadExampleTables.png)

## 仮想マシンの診断を有効にする方法

このチュートリアルでは、開発コンピューターから Azure の仮想マシンに診断をリモートでインストールする方法について説明します。 その Azure の仮想マシンで実行され、.NET を使用してテレメトリ データを送出するアプリケーションを実装する方法についても説明 [EventSource クラス][]します。 Azure 診断を使用してテレメトリを収集し、これを Azure ストレージ アカウントに格納します。

### 前提条件
このチュートリアルでは、Azure サブスクリプションがあり、Azure SDK で Visual Studio 2013 を使用していることを前提としています。 サインアップすることができます、Azure サブスクリプションがない、 [無料評価版][]します。 必ず [インストール Azure powershell version 0.8.7 以降][]します。

### 手順 1. 仮想マシンを作成する
1.  開発コンピューター上で Visual Studio 2013 を起動します。
2.  Visual Studio で **サーバー エクスプ ローラー** 展開 **Azure**, を右クリックして **仮想マシン** を選択し、 **バーチャル マシンの作成**します。
3.  Azure サブスクリプションを選択して、 **サブスクリプションを選択** クリックしてダイアログ **次**します。
4.  選択 **Windows Server 2012 R2 Datacenter、2014 年 11 月** で、 **仮想マシン イメージの選択** クリックしてダイアログ **次**します。
5.   **仮想マシン基本設定**, 、仮想マシン名を「wadexample」に設定します。 管理者のユーザー名とパスワードを設定し、クリックして **次**します。
6.   **クラウド サービスの設定** ダイアログは、"wadexampleVM"という名前の新しいクラウド サービスを作成します。 "Wadexample"という名前の新しいストレージ アカウントを作成し、クリックして **次**します。
7.  クリックして **作成**します。

### 手順 2. アプリケーションを作成する
1.  開発コンピューター上で Visual Studio 2013 を起動します。
2.  .NET Framework 4.5 をターゲットとする Visual C# の新しいコンソール アプリケーションを作成します。 プロジェクト名として「WadExampleVM」と入力します。
    ![CloudServices_diag_new_project](./media/cloud-services-dotnet-diagnostics/NewProject.png)
3.  Program.cs の内容を次のコードに置き換えます。 クラス **SampleEventSourceWriter** 4 つのログ記録メソッドを実装する: **SendEnums**, 、**MessageMethod**, 、**SetOther** と **HighFreq**します。 WriteEvent メソッドの最初のパラメーターは各イベントの ID を定義しています。 Run メソッドをそれぞれで実装されているログ記録メソッドを呼び出す無限ループを実装する、 **SampleEventSourceWriter** 10 秒ごとのクラスです。

        using System;
        using System.Diagnostics;
        using System.Diagnostics.Tracing;
        using System.Threading;

        namespace WadExampleVM
        {
        sealed class SampleEventSourceWriter : EventSource
        {
            public static SampleEventSourceWriter Log = new SampleEventSourceWriter();
            public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); }// Cast enums to int for efficient logging.
            public void MessageMethod(string Message) { if (IsEnabled())  WriteEvent(2, Message); }
            public void SetOther(bool flag, int myInt) { if (IsEnabled())  WriteEvent(3, flag, myInt); }
            public void HighFreq(int value) { if (IsEnabled()) WriteEvent(4, value); }

        }

        enum MyColor
        {
            Red,
            Blue,
            Green
        }

        [Flags]
        enum MyFlags
        {
            Flag1 = 1,
            Flag2 = 2,
            Flag3 = 4
        }

        class Program
        {
        static void Main(string[] args)
        {
            Trace.TraceInformation("My application entry point called");
            
            int value = 0;

            while (true)
            {
                Thread.Sleep(10000);
                Trace.TraceInformation("Working");

                // Emit several events every time we go through the loop
                for (int i = 0; i < 6; i++)
                {
                    SampleEventSourceWriter.Log.SendEnums(MyColor.Blue, MyFlags.Flag2 | MyFlags.Flag3);
                }

                for (int i = 0; i < 3; i++)
                {
                    SampleEventSourceWriter.Log.MessageMethod("This is a message.");
                    SampleEventSourceWriter.Log.SetOther(true, 123456789);
                }

                if (value == int.MaxValue) value = 0;
                SampleEventSourceWriter.Log.HighFreq(value++);
            }

        }
        }
        }


4.  ファイルを保存し、選択 **ソリューションのビルド** から、 **ビルド** メニューしてコードをビルドします。


### 手順 3. アプリケーションをデプロイする
1.  右クリックし、 **WadExampleVM** プロジェクト **ソリューション エクスプ ローラー** 選択 **ファイル エクスプ ローラーでフォルダーを開く**します。
2.  移動し、 *bin \debug* フォルダーとすべてのファイル (WadExampleVM.*) をコピー
3.   **サーバー エクスプ ローラー** 仮想マシンを右クリックし、選択 **リモート デスクトップを使用して接続**します。
4.  VM に接続されたら、WadExampleVM という名前のフォルダーを作成し、コピーしたアプリケーション ファイルをそのフォルダーに貼り付けます。
5.  アプリケーション ファイルの WadExampleVM.exe を起動します。 空白のコンソール ウィンドウが表示されます。

### 手順 4. 診断構成を作成して拡張機能をインストールする
1.  次の PowerShell コマンドを実行して、パブリック構成ファイルのスキーマ定義を開発コンピューターにダウンロードします。

        (Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd' 

2.  既に開いているプロジェクトか、プロジェクトを開かずに Visual Studio インスタンスで、Visual Studio の新しい XML ファイルを開きます。 Visual Studio で、次のように選択します。 **追加** ]-> [ **新しい項目]** ]-> [ **Visual c# アイテム** ]-> [ **データ** ]-> [ **XML ファイル**します。 ファイルに「WadExample.xml」という名前を付けます。
3.  構成ファイルに WadConfig.xsd を関連付けます。 WadExample.xml エディター ウィンドウがアクティブになっていることを確認します。 キーを押して **f4 キーを押して** を開くには、 **プロパティ** ウィンドウです。 をクリックして、 **スキーマ** プロパティに、 **プロパティ** ウィンドウです。 クリックして、 **...** で、 **スキーマ** プロパティです。 クリックして、 **追加…** ] ボタンをクリックし、XSD ファイルを保存した場所に移動し、WadConfig.xsd ファイルを選択します。 Click **OK**.
4.  WadExample.xml 構成ファイルの内容を次の XML に置き換え、ファイルを保存します。 この構成ファイルは、収集するいくつかのパフォーマンス カウンターを定義します。1 つは CPU 使用率、1 つはメモリ使用率です。 次に、SampleEventSourceWriter クラスのメソッドに対応する 4 つのイベントを定義します。

```
        <?xml version="1.0" encoding="utf-8"?>
        <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
            <WadCfg>
                <DiagnosticMonitorConfiguration overallQuotaInMB="25000">
                <PerformanceCounters scheduledTransferPeriod="PT1M">
                    <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />
                    <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT1M" unit="bytes"/>
                    </PerformanceCounters>
                    <EtwProviders>
                        <EtwEventSourceProviderConfiguration provider="SampleEventSourceWriter" scheduledTransferPeriod="PT5M">
                            <Event id="1" eventDestination="EnumsTable"/>
                            <Event id="2" eventDestination="MessageTable"/>
                            <Event id="3" eventDestination="SetOtherTable"/>
                            <Event id="4" eventDestination="HighFreqTable"/>
                            <DefaultEvents eventDestination="DefaultTable" />
                        </EtwEventSourceProviderConfiguration>
                    </EtwProviders>
                </DiagnosticMonitorConfiguration>
            </WadCfg>
        </PublicConfig>
```

### 手順 5. Azure の仮想マシンに診断をリモートでインストールする
VM の診断を管理する PowerShell コマンドレットは、Set-AzureVMDiagnosticsExtension、Get-AzureVMDiagnosticsExtension、Remove-AzureVMDiagnosticsExtension です。

1.  開発コンピューター上で Azure PowerShell を開きます。
2.  スクリプトは、VM に診断をリモートでインストールを実行 (置換 *StorageAccountKey* を wadexamplevm ストレージ アカウントのストレージ アカウント キー)。

        $storage_name = "wadexamplevm"
        $key = "<StorageAccountKey>"
        $config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExampleVM\WadExampleVM\WadExample.xml"
        $service_name="wadexamplevm"
        $vm_name="WadExample"
        $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key 
        $VM1 = Get-AzureVM -ServiceName $service_name -Name $vm_name
        $VM2 = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $config_path -Version "1.*" -VM $VM1 -StorageContext $storageContext
        $VM3 = Update-AzureVM -ServiceName $service_name -Name $vm_name -VM $VM2.VM


### 手順 6. テレメトリ データを確認する
Visual Studio で **サーバー エクスプ ローラー** wadexample ストレージ アカウントに移動します。 VM の実行は約 5 分後、テーブルが表示されます **WADEnumsTable**, 、**WADHighFreqTable**, 、**WADMessageTable**, 、**WADPerformanceCountersTable** と **WADSetOtherTable**します。 いずれかのテーブルをダブルクリックして、収集した利用統計情報を表示します。
    ![CloudServices_diag_wadexamplevm_tables](./media/cloud-services-dotnet-diagnostics/WadExampleVMTables.png)

## 構成ファイル スキーマ

診断構成ファイルは、診断モニターを開始するときに診断構成設定の初期化に使用される値を定義します。 サンプル構成ファイルとそのスキーマの詳細については、: [Azure 診断 1.2 構成スキーマ][]します。

## トラブルシューティング

### Azure 診断が起動しない
診断は、ゲスト エージェント プラグインと監視エージェントの 2 つのコンポーネントで構成されます。 ゲスト エージェント プラグインのログ ファイルは次のファイルにあります。 

*%SystemDrive%\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\ < DiagnosticsVersion >*\CommandExecution.log

プラグインによって次のエラー コードが返されます。

終了コード|説明
---|---
0|成功。
-1|一般的なエラー。
-2|rcf ファイルを読み込めませんでした。<p>これは、ゲスト エージェント プラグインの起動ツールが VM 上で不正に手動で起動された場合にのみ発生する内部エラーです。
-3|診断構成ファイルを読み込めませんでした。<p><p>解決方法: これは、構成ファイルがスキーマ検証を渡さないことが原因です。 スキーマに準拠する構成ファイルを使用してください。
-4|監視エージェント診断の別のインスタンスが既にローカル リソース ディレクトリを使用しています<p><p>。解決方法: 別の値を指定する **LocalResourceDirectory**します。
-6|ゲスト エージェント プラグインの起動ツールが無効なコマンド·ラインで診断を起動しようとしました。<p><p>これは、ゲスト エージェント プラグインの起動ツールが VM 上で不正に手動で起動された場合にのみ発生する内部エラーです。
-10|診断がハンドルされていない例外で終了しました。
-11|ゲスト エージェントが、監視エージェントの起動と監視を処理するプロセスを作成できませんでした。<p><p>解決方法: 新しいプロセスを起動するのに十分なシステム リソースが使用できることを確認します。<p>
-101|診断プラグインを呼び出すときの引数が無効です。<p><p>これは、ゲスト エージェント プラグインの起動ツールが VM 上で不正に手動で起動された場合にのみ発生する内部エラーです。
-102|プラグイン プロセスの初期化でエラーが発生しました。<p><p>解決方法: 新しいプロセスを起動するのに十分なシステム リソースが使用できることを確認します。
-103|プラグイン プロセスの初期化でエラーが発生しました。 具体的にはロガー オブジェクトを作成できません。<p><p>解決方法: 新しいプロセスを起動するのに十分なシステム リソースが使用できることを確認します。
-104|ゲスト エージェントが指定した rcf ファイルを読み込むことができません。<p><p>これは、ゲスト エージェント プラグインの起動ツールが VM 上で不正に手動で起動された場合にのみ発生する内部エラーです。
-105|診断プラグインが診断構成ファイルを開くことができません。<p><p>これは、診断プラグインが VM 上で不正に手動で起動された場合にのみ発生する内部エラーです。
-106|診断構成ファイルを読み込めませんでした。<p><p>解決方法: これは、構成ファイルがスキーマ検証を渡さないことが原因です。 そのため、スキーマに準拠する構成ファイルを使用してください。 フォルダーに診断拡張機能に渡される XML が見つかります *%SystemDrive%\WindowsAzure\Config* VM にします。 適切な XML ファイルを開き、検索 **Microsoft.Azure.Diagnostics**, 、次に、 **xmlCfg** フィールドです。 データは base64 エンコードされる必要がありますので [デコード](http://www.bing.com/search?q=base64+decoder) 診断で読み込まれた XML を参照します。<p>
-107|監視エージェントへのリソース ディレクトリのパスが無効です。<p><p>これは、監視エージェントが VM 上で不正に手動で起動された場合にのみ発生する内部エラーです。</p>
-108    |診断構成ファイルを監視エージェント構成ファイルに変換できません。<p><p>これは、診断プラグインが無効な構成ファイルを使用して手動で起動された場合にのみ発生する内部エラーです。
-110|一般的な診断構成エラー。<p><p>これは、診断プラグインが無効な構成ファイルを使用して手動で起動された場合にのみ発生する内部エラーです。
-111|監視エージェントを開始できません。<p><p>解決方法: 十分なシステム リソースを使用できることを確認します。
-112|一般的なエラー


### 診断データがストレージに記録されない
イベント データが見つからない最も一般的な原因は、不正に定義されたストレージ アカウント情報です。 

解決方法: 診断構成ファイルを修正し、診断を再インストールします。
イベント データは、ストレージ アカウントにアップロードされる前にフォルダーに格納されます。 詳細については上記で **LocalResourceDirectory**します。

このフォルダーにファイルがない場合、監視エージェントは起動できません。 これは通常、無効な構成ファイルが原因であり、CommandExecution.log で報告される必要があります。 監視エージェントが正常にイベント·データを収集している場合は、構成ファイルで定義されたイベントごとに .tsf ファイルが表示されます。

監視エージェントは、MaEventTable.tsf ファイルに発生したすべてのエラーを記録します。 このファイルの内容を確認するには、次のコマンドを実行します。

        %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.[IaaS | PaaS]Diagnostics\1.3.0.0\Monitor\x64\table2csv maeventtable.tsf

このツールでは maeventtable.csv というファイルが生成されます。このログを開いてエラーを確認できます。


## よく寄せられる質問
よく寄せられる質問とその回答は次のとおりです。

**Q。**Visual Studio ソリューションをどのようにして Azure 診断 1.0 から Azure 診断 1.1 にアップグレードしますか。

**A.**Visual Studio ソリューションを診断 1.0 から診断 1.1 (以降) にアップグレードするには、次の手動のプロセスを行います。
- Visual Studio ソリューションの診断を無効にし、診断 1.0 がロールと共に展開されないようにします。
- コードがトレース リスナーを使用している場合は、.NET EventSource を使用するようにコードを変更する必要があります。 診断 1.1 以降はトレース リスナーをサポートしません。
- 展開プロセスを変更して診断 1.1 拡張機能をインストールします。

**Q。**診断 1.1 拡張機能を既にロールまたは VM 上にインストールしている場合は、どのようにして診断 1.2 または 1.3 にアップグレードしますか。

**A.**指定した場合は"– Version"1*""。診断 1.1 をインストールしたときに、次回ロールまたは VM が再起動を正規表現に一致する最新バージョンに自動的に更新が"1*"。場合は"– バージョン「1.1」"する診断 1.1 のインストールを更新する、新しいバージョンするセット コマンドレットを再実行しをインストールするバージョンを指定します。

**Q。**テーブルにはどのようにして名前が付けられますか。

**A.**テーブルは次のようにして名前が付けられます。

        if (String.IsNullOrEmpty(eventDestination)) {
            if (e == "DefaultEvents")
                tableName = "WADDefault" + MD5(provider);
            else
                tableName = "WADEvent" + MD5(provider) + eventId;
        }
        else
            tableName = "WAD" + eventDestination;

たとえば次のようになります。

        <EtwEventSourceProviderConfiguration provider=”prov1”>
          <Event id=”1” />
          <Event id=”2” eventDestination=”dest1” />
          <DefaultEvents />
        </EtwEventSourceProviderConfiguration>
        <EtwEventSourceProviderConfiguration provider=”prov2”>
          <DefaultEvents eventDestination=”dest2” />
        </EtwEventSourceProviderConfiguration>

次の 4 つのテーブルが生成されます。

イベント|テーブル名
---|---
provider=”prov1” &lt;Event id=”1” /&gt;|WADEvent+MD5(“prov1”)+”1”
provider=”prov1” &lt;Event id=”2” eventDestination=”dest1” /&gt;|WADdest1
provider=”prov1” &lt;DefaultEvents /&gt;|WADDefault+MD5(“prov1”)
provider=”prov2” &lt;DefaultEvents eventDestination=”dest2” /&gt;|WADdest2

## Azure 診断のバージョンの比較

次の表は、Azure 診断のバージョン 1.0 および 1.1/1.2/1.3 でサポートされる機能を比較します。

ロールの種類の Supported|診断 1.0|診断 1.1/1.2/1.3
---|---
Web role|| あり |[はい]
ワーカー role|| あり |[はい]
IaaS|| なし[はい]

構成とデプロイメント|診断 1.0|診断 1.1/1.2/1.3
---|---|---
Visual Studio との統合 - Azure Web/Worker 開発機能への統合。|あり|いいえ
PowerShell スクリプト - 診断のロールへのインストールと構成を管理するスクリプト。|あり|あり

データ ソース|既定のコレクション|形式|説明|診断 1.0|診断 1.1/1.2|診断 1.3
---|---|---|---|---|---|---
System.Diagnostics.Trace ログ|あり|テーブル|コードからトレース リスナーに送信されるトレース メッセージを記録します (トレース リスナーを web.config ファイルまたは app.config ファイルに追加する必要があります)。 ログ データは、scheduledTransferPeriod の転送間隔でストレージ テーブル WADLogsTable に転送されます。|あり|なし (EventSource を使用)|あり
IIS ログ|あり|BLOB|IIS サイトに関する情報を記録します。 ログ データは、scheduledTransferPeriod の転送間隔で、指定したコンテナーに転送されます。|あり|はい|あり
Azure 診断インフラストラクチャ ログ|あり|テーブル|診断インフラストラクチャ、RemoteAccess モジュール、RemoteForwarder モジュールに関する情報を記録します。 ログ データは、scheduledTransferPeriod の転送間隔でストレージ テーブル WADDiagnosticInfrastructureLogsTable に転送されます。|あり|はい|あり
IIS の失敗した要求ログ|いいえ|BLOB|IIS サイトまたはアプリケーションへの失敗した要求に関する情報を記録します。 Web.config の system.WebServer の下でトレース オプションを設定することでも有効にする必要があります。 ログ データは、scheduledTransferPeriod の転送間隔で、指定したコンテナーに転送されます。|あり|はい|あり
Windows イベント ログ|いいえ|テーブル|オペレーティング システム、アプリケーション、またはドライバーが適切に動作しているかどうかを示す情報を記録します。 パフォーマンス カウンターは明示的に指定する必要があります。 これらが追加されると、パフォーマンス カウンター データは scheduledTransferPeriod の転送間隔でストレージ テーブル WADPerformanceCountersTable に転送されます。|あり|はい|あり
パフォーマンス カウンター|いいえ|テーブル|オペレーティング システム、アプリケーション、またはドライバーが適切に動作しているかどうかを示す情報を記録します。 パフォーマンス カウンターは明示的に指定する必要があります。 これらが追加されると、パフォーマンス カウンター データは scheduledTransferPeriod の転送間隔でストレージ テーブル WADPerformanceCountersTable に転送されます。|あり|はい|あり
クラッシュ ダンプ|いいえ|BLOB|システム クラッシュ時のオペレーティング システムの状態に関する情報を記録します。 小さいクラッシュ ダンプがローカルで収集されます。 完全なダンプを有効にできます。 ログ データは、scheduledTransferPeriod の転送間隔で、指定したコンテナーに転送されます。 ほとんどの例外は ASP.NET によって処理されるため、これは通常 Worker ロールまたは VM でのみ役立ちます。|あり|はい|あり
カスタム エラー ログ|いいえ|BLOB|ローカル ストレージ リソースを使用することで、カスタム データを記録し、指定したコンテナーにすぐに転送できます。|あり|はい|あり
EventSource|いいえ|テーブル|.NET EventSource クラスを使用してコードで生成されたログ イベント。|いいえ|はい|あり
マニフェスト ベースの ETW|いいえ|テーブル|すべてのプロセスで生成された ETW イベント。|いいえ|はい|あり


## その他のリソース

- [Troubleshooting Best Practices for Developing Azure Applications (Azure アプリケーション開発時のトラブルシューティングのベスト プラクティス)][]
- [Azure 診断を使用したログ データの収集][]
- [Azure アプリケーションのデバッグ][]
- [Azure クラウド サービスおよび仮想マシン用の診断の構成][]

  

[Overview]: #overview
[How to Enable Diagnostics in a Worker Role]: #worker-role
[How to Enable Diagnostics in a Virtual Machine]: #virtual-machine
[Sample Configuration File and Schema]: #configuration-file-schema
[Troubleshooting]: #troubleshooting
[Frequently Asked Questions]: #faq
[Comparing Azure Diagnostics Versions]: #comparing
[Additional Resources]: #additional
[EventSource Class]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx
  
[Configuring Diagnostics for Azure Cloud Services and Virtual Machines]: http://msdn.microsoft.com/library/windowsazure/dn186185.aspx
[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Troubleshooting Best Practices for Developing Azure Applications]: http://msdn.microsoft.com/library/windowsazure/hh771389.aspx
[Free Trial]: http://azure.microsoft.com/pricing/free-trial/
[Install and configure Azure PowerShell version 0.8.7 or later]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
[Azure Diagnostics 1.2 Configuration Schema]: http://msdn.microsoft.com/library/azure/dn782207.aspx
[Set-AzureServiceDiagnosticsExtension]: http://msdn.microsoft.com/library/dn495270.aspx
[Get-AzureServiceDiagnosticsExtension]: http://msdn.microsoft.com/library/dn495145.aspx
[Remove-AzureServiceDiagnosticsExtension]: http://msdn.microsoft.com/library/dn495168.aspx
 

