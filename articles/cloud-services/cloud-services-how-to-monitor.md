<properties 
    pageTitle="クラウド サービスの監視方法 | Microsoft Azure" 
    description="Azure クラシック ポータルを使用して、クラウド サービスを監視する方法について説明します。" 
    services="cloud-services" 
    documentationCenter="" 
    authors="rboucher" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="robb"/>


#クラウド サービスの監視方法

[AZURE.INCLUDE [disclaimer](../../includes/disclaimer.md)]

Azure クラシック ポータルで、クラウド サービスの主要パフォーマンス メトリックを監視できます。 監視レベルは、サービス ロールごとに最小および詳細に設定でき、監視画面をカスタマイズできます。 詳細監視データはストレージ アカウントに保存され、ポータル外からもアクセスできます。 

Azure クラシック ポータルの監視画面は自由に構成できます。 メトリックの一覧で監視するメトリックを選択でき、 **モニター** とチャートのメトリックをプロットするメトリックを選択できます、 **モニター** ページとダッシュ ボード。 

##概念##

既定では、新しいクラウド サービス用に最小監視が用意されており、ロール インスタンス (仮想マシン) のホスト オペレーティング システムから収集したパフォーマンス カウンターが使用されます。 最小メトリックは、CPU 使用率、受信データ、送信データ、ディスク読み取りのスループット、およびディスク書き込みのスループットに限定されます。 詳細監視を構成することで、仮想マシン (ロール インスタンス) 内のパフォーマンス データに基づいた追加のメトリックが使用できます。 詳細メトリックを使用すると、アプリケーションの操作中に発生する問題を詳しく分析できます。

既定では、ロール インスタンスから得られるパフォーマンス カウンター データは、3 分間隔でサンプリングされてロール インスタンスから転送されます。 詳細監視を有効にすると、生のパフォーマンス カウンター データが、5 分、1 時間、および 12 時間間隔で、ロール インスタンスごとに、および各ロールのロール インスタンス全体で集計されます。 集計されたデータは、10 日後に消去されます。

詳細監視を有効にすると、集計された監視データはストレージ アカウントのテーブルに保存されます。 ロールの詳細監視を有効にするには、ストレージ アカウントにリンクする診断接続文字列を構成する必要があります。 異なるロールには異なるストレージ アカウントを使用できます。

詳細監視を有効にすると、データ ストレージ、データ転送、およびストレージ トランザクションに関連したストレージ コストが増加することに注意してください。 最小監視の場合、ストレージ アカウントは不要です。 最小監視レベルで公開されるメトリック用のデータは、監視レベルを詳細に設定した場合でも、ストレージ アカウントには保存されません。


##How to: クラウド サービスの監視の構成##

以下の手順を使用して、Azure クラシック ポータルで詳細監視または最小監視を構成します。 

###開始する前に###

- 監視データを保存するストレージ アカウントを作成します。 異なるロールには異なるストレージ アカウントを使用できます。 詳細については、次を参照してください。 ヘルプを **ストレージ アカウント**, 、または参照してください [をストレージ アカウントを作成する方法](/manage/services/storage/how-to-create-a-storage-account/)します。

- クラウド サービス ロールの Azure 診断を有効にします。 参照してください [クラウド サービスの診断を構成する](https://msdn.microsoft.com/library/azure/dn186185.aspx#BK_EnableBefore)です。

診断接続文字列がロール構成にあることを確認します。 Azure 診断を有効にして、ロール構成に診断接続文字列を含めるまで、詳細監視を有効にすることはできません。   

> [AZURE.NOTE] Azure SDK 2.5 を対象とするプロジェクトでは、診断接続文字列は自動的にはプロジェクトのテンプレートに含めないでした。 これらのプロジェクトでは、ロール構成に診断接続文字列を手動で追加する必要があります。

**ロール構成に診断接続文字列を手動で追加するには**

1. Visual Studio でクラウド サービス プロジェクトを開きます。
2. ダブルクリックして、 **ロール** を開くには、ロール デザイナーを選択、 **設定** ] タブ
3. という名前の設定を探します **Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString**します。 
4. この設定が存在しない場合にクリックして、 **設定の追加** をクリックして、構成に追加する新しい設定の種類を変更 **ConnectionString**
5. 接続文字列の値を設定します] をクリックして、 **...** ] ボタンをクリックします。 表示されるダイアログ ボックスで、ストレージ アカウントを選択できます。

    ![Visual Studio の設定](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioDiagnosticsConnectionString.png)

###監視レベルを詳細または最小に変更するには###

1.  [Azure クラシック ポータル](https://manage.windowsazure.com/), を開き、 **構成** 、クラウド サービス デプロイのページです。

2.  **レベル**, 、クリックして **Verbose** または **最小限**します。 

3. クリックして **保存**します。

詳細監視を有効にした後、1 時間以内に Azure クラシック ポータルに監視データが表示され始めます。

生のパフォーマンス カウンター データと集計した監視データは、ストレージ アカウントのテーブルに保存され、テーブル名はロールのデプロイ ID で修飾されます。 

##How to: クラウド サービスのメトリックに関するアラートの受信##

クラウド サービスの監視メトリックに基づいてアラートを受け取ることができます。  **管理サービス** ページは、Azure の従来のポータルで、規則を作成、選択したメトリックが指定した値に達したときに警告をトリガーします。 アラートがトリガーされたときに電子メールが送信されるように指定することもできます。 詳細については、次を参照してください。 [方法: アラートの通知を受信し、Azure でアラート ルールの管理](http://go.microsoft.com/fwlink/?LinkId=309356)します。

##How to: メトリック テーブルへのメトリックの追加##

1.  [Azure クラシック ポータル](http://manage.windowsazure.com/), を開き、 **モニター** クラウド サービスのページです。

    既定で、メトリック テーブルには使用可能なメトリックのサブセットが表示されます。 図はクラウド サービス用の既定の詳細メトリックを示しています。これは "メモリ/利用可能な MB" パフォーマンス カウンターに限定されており、データはロール レベルで集計されます。 使用 **メトリックの追加** Azure クラシック ポータルで監視する追加の集計およびロール レベル メトリックを選択します。

    ![詳細表示](./media/cloud-services-how-to-monitor/CloudServices_DefaultVerboseDisplay.png)
 
2. メトリック テーブルにメトリックを追加するには、以下の手順を実行します。

    a. クリックして **メトリックの追加** を開くには **メトリックの選択**, 、下図のようです。
    最初に利用可能なメトリックはデプロイされ、利用可能なオプションが表示されています。 メトリックごとに、一番上のオプションはすべてのロールの集計監視データを表示します。 さらに、データを表示する個々のロールを選択することもできます。

    ![メトリックの追加](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics.png)


    b. To select metrics to display:

    - Click the down arrow by the metric to expand the monitoring options.
    - Select the check box for each monitoring option you want to display.

    You can display up to 50 metrics in the metrics table.

    > [AZURE.TIP] In verbose monitoring, the metrics list can contain dozens of metrics. To display a scrollbar, hover over the right side of the dialog box. To filter the list, click the search icon, and enter text in the search box, as shown below.
 
    ![Add metrics search](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics_Search.png)

3. メトリックの選択が終了したら [OK] (チェックマーク) をクリックします。

    下図のように、選択したメトリックがメトリック テーブルに追加されます。

    ![監視メトリック](./media/cloud-services-how-to-monitor/CloudServices_Monitor_UpdatedMetrics.png)

 
4. メトリック テーブルからメトリックを削除する、メトリックをクリックして選択し、をクリックし、 **メトリックの削除]**します。 (表示のみ **メトリックの削除]** 、メトリックを選択する必要がある場合)。

###メトリック テーブルにカスタム メトリックを追加するには###
 **Verbose** ポータルに監視可能な既定のメトリックの一覧を指定するレベルを監視します。 これらに加えて、ポータルを使用してアプリケーションで定義されているカスタム メトリックまたはパフォーマンス カウンターを監視できます。

次の手順では、オンにしたと仮定 **Verbose** レベルを監視して、アプリケーションを収集してカスタム パフォーマンス カウンターの転送を構成します。 

ポータルでカスタム パフォーマンス カウンターを表示するには、wad-control-container の構成を更新する必要があります。
 
1.  診断ストレージ アカウントで wad-control-container BLOB を開きます。 これを行うには、Visual Studio または他の記憶域エクスプローラーを使用できます。

    ![Visual Studio のサーバー エクスプローラー](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioBlobExplorer.png)
2. パターンを使用して、blob のパスを移動 **DeploymentId/RoleName/RoleInstance** ロール インスタンスの構成を検索します。 

    ![Visual Studio のストレージ エクスプローラー](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioStorage.png)
3. ロール インスタンスの構成ファイルをダウンロードし、更新してカスタム パフォーマンス カウンターを追加します。 モニターにたとえば *ディスクへの書き込みバイト数/秒* の *C ドライブ* の下に次の追加 **performancecounters \subscriptions** ノード

    ```xml
    <PerformanceCounterConfiguration>
    <CounterSpecifier>\LogicalDisk(C:)\Disk Write Bytes/sec</CounterSpecifier>
    <SampleRateInSeconds>180</SampleRateInSeconds>
    </PerformanceCounterConfiguration>
    ```
4. 変更を保存し、同じ場所に構成ファイルをアップロードして、BLOB 内の既存のファイルを上書きします。
5. Azure クラシック ポータルの構成で詳細モードに切り替えます。 既に詳細モードにしてあった場合は、いったん最小に切り替えてから詳細に戻す必要があります。
6. カスタム パフォーマンス カウンターがで使用できるよう、 **メトリックの追加** ] ダイアログ ボックス。 

##How to: メトリック チャートのカスタマイズ##

1. メトリック テーブルで、メトリック チャートにプロットするメトリックを最大 6 個選択します。 メトリックを選択するには、メトリックの左側にあるチェック ボックスをオンにします。 メトリック チャートからメトリックを削除するには、メトリック テーブルのチェック ボックスをオフにします。

    メトリック テーブルでメトリックを選択すると、メトリックがメトリック チャートに追加されます。 幅の狭い画面で、 **残り n 項目** ドロップダウン リストに表示におさまりきらなかったメトリックのヘッダーが含まれています。

 
2. 相対値 (最終値は各メトリックにだけ) と絶対値 (Y 軸を表示) の表示を切り替えるには、チャートの上部で [相対] または [絶対] をクリックします。

    ![相対または絶対](./media/cloud-services-how-to-monitor/CloudServices_Monitor_RelativeAbsolute.png)

3. メトリック チャートに表示する期間を変更するには、チャートの上部で 1 時間、24 時間、または 7 日を選択します。

    ![監視の表示期間](./media/cloud-services-how-to-monitor/CloudServices_Monitor_DisplayPeriod.png)

    ダッシュボードのメトリック チャートでは、メトリックのプロット方法が異なります。 メトリックの標準セットが使用でき、メトリックのヘッダーを選択することでメトリックを追加または削除できます。

###ダッシュボードでメトリック チャートをカスタマイズするには###

1. クラウド サービスのダッシュボードを開きます。

2. チャートのメトリックを追加または削除します。

    - 新しいメトリックをプロットするには、チャートのヘッダー部でメトリックのチェック ボックスをオンにします。 幅の狭い画面で、下矢印をクリックして ***n*[概要] タブメトリック** メトリックをプロットするグラフのヘッダー領域を表示できません。

    - チャートにプロットされているメトリックを削除するには、そのヘッダーにあるチェック ボックスをオフにします。

3. 切り替える **相対** と **絶対** が表示されます。

4. 表示するデータの期間を 1 時間、24 時間、または 7 日に設定します。

##方法: Azure クラシック ポータルの外部で詳細監視データにアクセスする##

詳細監視データは、各ロールに指定したストレージ アカウントのテーブルに保存されます。 クラウド サービス デプロイごとに、ロールに対して 6 つのテーブルが作成されます。 間隔 (5 分、1 時間、および 12 時間) のそれぞれにテーブルが 2 つ作成されます。 片方のテーブルはロール レベルの集計を保存し、もう片方のテーブルはロール インスタンスごとの集計を保存します。 

テーブル名は次の形式です。

    WAD*deploymentID*PT*aggregation_interval*[R|RI]Table

各値の説明:

- *deploymentID* 、クラウド サービス デプロイに割り当てられている guid を指定します

- *aggregation_interval* = 5 M、1 H、または 12h

- ロール レベルの集計 = R

- ロール インスタンスの集計 = RI

たとえば、次のテーブルには 1 時間間隔で集計された詳細監視データが保存されます。

    WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRTable (hourly aggregations for the role)

    WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRITable (hourly aggregations for role instances)
 

