<properties 
    pageTitle="新しい Application Insights リソースを作成します。" 
    description="新しいライブ アプリケーションを対象にした Application Insights 監視をセットアップします。 Web ベースのアプローチです。" 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/06/2015" 
    ms.author="awills"/>

# 新しい Application Insights リソースを作成します。



[AZURE.INCLUDE [app-insights-selector-get-started](../../includes/app-insights-selector-get-started.md)]

Visual Studio Application Insights は、Microsoft Azure でアプリケーションに関するデータを表示 *リソース*します。 新しいリソースの作成の一部であるため [新しいアプリケーションを監視する Application Insights を設定する][start]です。 多くの場合、これは IDE によって自動的に行うことができ、可能な場合には常にそうするよう勧められています。 しかし、リソースを手動で作成する場合もあります。

リソースを作成した後、インストルメンテーション キーを取得し、それを使用してアプリケーション内の SDK を構成します。 これは、リソースにテレメトリを送信します。

## Microsoft Azure へのサインアップ

取得していない場合、 [Microsoft アカウントは、1 つを今すぐ入手](http://live.com)します。 (Outlook.com、OneDrive、Windows Phone、XBox Live などのサービスを利用している場合、Microsoft アカウントを持っています。)

サブスクリプションが必要もあります [Microsoft Azure](http://azure.com)します。 チームまたは組織で Azure サブスクリプションを取得している場合、所有者は Windows Live ID を使用してあなたを追加できます。

または、新しいサブスクリプションを作成することもできます。 無料試用版では、Azure 内のすべてを試すことができます。 評価期間が過ぎた後、無料サービスでは課金されないため、従量制サブスクリプションが適切であると感じるかもしれません。 

Application Insights にログインし、サブスクリプションへのアクセスがある場合 [http://portal.azure.com](http://portal.azure.com), 、ログインに自分の Live ID を使用します。


## Application Insights リソースの作成
  

 [Portal.azure.com](https://portal.azure.com), 、Application Insights のリソースを追加します。

![[新規]、[Application Insights] の順にクリックする](./media/app-insights-create-new-resource/01-new.png)


* **アプリケーションの種類** 概要ブレードとで設定できるプロパティに表示されるものに影響を与えます [メトリックス エクスプ ローラー][metrics]します。 自分のアプリの種類が表示されない場合、Web ページの Web の種類のいずれかと、その他のデバイスの電話の種類のいずれかを選択します。
* **リソース グループ** プロパティを管理するための便利な機能は、アクセス制御などです。 その他の Azure リソースが既に作成されている場合、新しいリソースを同じグループに入れることもできます。
* **サブスクリプション** は、Azure での支払いアカウントです。
* **場所** は、データの保存場所です。 現在、これは変更できません。
* **スタート画面に追加** Azure ホーム ページ上のリソースに対するクイック アクセス タイルを配置します。 推奨。

アプリが作成されると、新しいブレードが開きます。 そのブレードには、アプリに関するパフォーマンスと使用状況データが表示されます。 

次回 Azure にログインするときにそこへ戻るには、スタート画面 (ホーム画面) 上のアプリのクイック スタート タイルを探してください。 あるいは、[参照] ボタンをクリックして探します。


## インストルメンテーション キーのコピー

インストルメンテーション キーは作成したリソースを識別します。 これは、SDK に渡すために必要です。

![Essentials、インストルメンテーション キーの順にクリックし、Ctrl キーを押しながら C キーを押します。](./media/app-insights-create-new-resource/02-props.png)

## アプリケーションでの SDK のインストール

アプリで Application Insights SDK をインストールします。 この手順は、アプリケーションの種類に大きく依存します。 

インストルメンテーション キーを使用して構成する [アプリケーションにインストールする SDK][start]します。

SDK には、コードを記述せずにテレメトリを送信する標準的なモジュールが含まれています。 ユーザーの操作を追跡したり、詳細については、問題を診断 [API を使用して][api] 、独自のテレメトリを送信します。


## <a name="monitor"></a>製品利用統計情報を参照してください。

クイック スタート ブレードを閉じ、Azure ポータルのアプリケーション ブレードに戻ります。

[検索タイルを表示する] をクリックして [診断検索][diagnostic], 、最初のイベントが表示されます。 

大量のデータが予想される場合は、数秒後に [最新の情報に更新] をクリックします。

## リソースの自動作成

記述することができます、 [PowerShell スクリプト](app-insights-powershell-script-create-resource.md) リソースを自動的に作成します。




<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[diagnostic]: app-insights-diagnostic-search.md
[metrics]: app-insights-metrics-explorer.md
[start]: app-insights-overview.md

 
