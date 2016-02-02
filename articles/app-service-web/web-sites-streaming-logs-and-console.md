<properties 
    pageTitle="ストリーミング ログとコンソール" 
    description="ストリーミング ログとコンソールの概要" 
    authors="btardif" 
    manager="wpickett" 
    editor="" 
    services="app-service\web" 
    documentationCenter=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="11/12/2015" 
    ms.author="byvinyal"/>


# ストリーミング ログとコンソール

### ストリーミング ログ

Microsoft Azure ポータルを提供できるようにトレース イベントを表示する、統合されたストリーミング ログ ビューアー 
Azure App Service web アプリでリアルタイムにします。

この設定には、次のいくつかの簡単な手順が必要です。

- コードにトレースを記述する
- Azure ポータル内からアプリケーション診断を有効にする
- Web アプリ ブレードの [ストリーミング ログ] パーツをクリックする

### 方法: コードにトレースを記述する

コードにトレースを記述するのは簡単です。 C# では、次のコードを記述するだけです。

`````````````````````````
Trace.TraceInformation("My trace statement");
`````````````````````````

`````````````````````````
Trace.TraceWarning("My warning statement");
`````````````````````````

`````````````````````````
Trace.TraceError("My error statement");
`````````````````````````

Trace クラスは System.Diagnostics 名前空間にあります。

node.js アプリケーションでは、次のコードを記述すると同じ結果を得られます。

`````````````````````````
console.log("My trace statement").
`````````````````````````

### 方法: ストリーミング ログを有効にして表示する

![][browsesitesscreenshot]
診断は Web アプリごとに有効になります。 内から、 [ポータル](https://portal.azure.com) ] をクリックします
 **(1) を参照** 左側のメニュー バーのボタンをクリックし、クリックして **Web Apps (2)** を取得する、 **リスト (3)** の 
 すべての web アプリです。

構成する Web アプリの名前をクリックして移動します。

![][diagnosticslogs]
クリックし、 **設定 (1)** > **診断ログ (2)** 電源をオンに **に** 
**アプリケーション ログ記録 (Filesystem)(3)** 、 **レベル** オプションでは、重要度を変更できます。 
取り込むトレースのレベルです。 これを設定する必要があります **Verbose** にだけしようとしている場合 
トレース ステートメントのすべて記録されることが、機能についてよく知ってを取得します。

ブレードの上部にある **[保存]** をクリックします。これで、ログを表示できる状態になりました。

**注:** 中で高い方、 **重大度レベル** ログおよびトレースの多くを他のリソースが消費されます。 
次のように表示されます。 これに設定されている適切なレベルのこの機能を使用しているかどうかを確認します。 
トラフィックの多い/実稼働サイトです。

![][streaminglogsscreenshot]
ポータルのクリック内からストリーミング ログを表示する **ツール (1)** > **ログ Stream(2)**します。 場合、アプリ 
アクティブに書き込まトレース ステートメントは、ほぼリアルタイムで結果ウィンドウには表示する必要があります。

## コンソール

Azure ポータルには、Web アプリ環境にアクセスするためのコンソールが用意されています。 Web を調べることができます。 
アプリケーションのファイル システムと powershell/cmd スクリプトを実行します。 設定された同じアクセス許可が適用します。 
web アプリ コードを実行するコンソール コマンドを実行するときにします。 保護されたアクセスすることはできません。 
ディレクトリまたは高度な権限を必要とするスクリプトを実行します。

![][consolescreenshot]
コンソールにアクセスするには、前のセクションで説明しているように Web アプリを参照します。 
**[ツール]**、**[コンソール]** の順にクリックすると、コンソールが開きます。

コンソールに詳しくなるには、次のような基本的なコマンドを試してください。



`````````````````````````
dir
`````````````````````````
`````````````````````````
cd
`````````````````````````





[diagnosticslogs]: ./media/web-sites-streaming-logs-and-console/diagnostic-logs.png 
[browsesitesscreenshot]: ./media/web-sites-streaming-logs-and-console/browse-sites.png 
[streaminglogsscreenshot]: ./media/web-sites-streaming-logs-and-console/streaming-logs.png 
[consolescreenshot]: ./media/web-sites-streaming-logs-and-console/console.png 

