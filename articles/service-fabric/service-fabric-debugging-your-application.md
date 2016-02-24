<properties
   pageTitle="Visual Studio でアプリケーションをデバッグする | Microsoft Azure"
   description="Visual Studio とローカル開発クラスターを使用してサービスを開発、デバッグし、サービスの信頼性とパフォーマンスを改善する。"
   services="service-fabric"
   documentationCenter=".net"
   authors="jessebenson"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/15/2015"
   ms.author="jesseb"/>

# Visual Studio による Service Fabric アプリケーションのデバッグ

ローカル コンピューターの開発クラスターで Service Fabric アプリケーションをデプロイしデバッグすることにより、時間と費用を節約できます。 Visual Studio では、アプリケーションをローカル クラスターにデプロイして、アプリケーションのすべてのインスタンスにデバッガーを自動的に接続できます。

1. 次の手順に従って、ローカル開発クラスターを開始 [Service Fabric 開発環境を設定する](service-fabric-get-started.md)です。

2. キーを押して **f5 キーを押して** ] をクリックしてまたは **デバッグ** > **[デバッグ開始]**

    ![アプリケーションのデバッグを開始する][デバッグ]

3. コマンドをクリックしてアプリケーションをステップ実行、コードにブレークポイントを設定、 **デバッグ** メニュー。

    > [AZURE.NOTE] Visual Studio は、アプリケーションのすべてのインスタンスにアタッチします。 コードのステップ実行中に、ブレークポイントに複数のプロセスがヒットして、同時セッションになる場合があります。 ヒットしたブレークポイントを無効にし、ブレークポイントをスレッド ID の条件付きにするか、診断イベントを使用します。

4.  **診断イベント** ウィンドウは自動的に開き、診断イベントをリアルタイムで表示します。

    ![イベント診断をリアルタイムで表示する][diagnosticevents]

5. 開くことも、 **診断イベント** サーバー エクスプ ローラー ウィンドウ。   **Azure**, を右クリックして **サービス ファブリック クラスター** > **診断イベントを表示しています.**

    ![診断イベント ウィンドウを開く][viewdiagnosticevents]

6. 診断イベントを表示できますに自動的に生成される **ServiceEventSource.cs** アプリケーション コードから呼び出されます。

    ```csharp
    ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, Service.ServiceTypeName);
    ```

7.  **診断イベント** ウィンドウには、フィルター処理、一時停止、およびリアルタイムのイベント検査がサポートしています。  このフィルターは、イベント メッセージとその内容の単純な文字列検索です。

    ![フィルター処理、一時停止し再開、またはリアルタイムのイベント検査][diagnosticeventsactions]

8. サービスのデバッグは、その他のアプリケーションのデバッグと似ています。 通常 Visual Studio で簡単なデバッグ用のブレークポイントを設定できます。 信頼性の高いコレクションが複数のノードにレプリケートされている場合でも、IEnumerable は実装されているため、デバッグ中に Visual Studio の結果ビューを使用して内部保存されているものを確認できます。 コードの任意の場所にブレークポイントを設定するだけです。

    ![アプリケーションのデバッグを開始する][ブレークポイント]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## 次のステップ

- [Service Fabric サービスをテスト](service-fabric-testability-overview.md)します。
- [Visual Studio で Service Fabric アプリケーションの管理](service-fabric-manage-application-in-visual-studio.md)します。

<!--Image references-->
[startdebugging]: ./media/service-fabric-debugging-your-application/startdebugging.png
[diagnosticevents]: ./media/service-fabric-debugging-your-application/diagnosticevents.png
[viewdiagnosticevents]: ./media/service-fabric-debugging-your-application/viewdiagnosticevents.png
[diagnosticeventsactions]: ./media/service-fabric-debugging-your-application/diagnosticeventsactions.png
[breakpoint]: ./media/service-fabric-debugging-your-application/breakpoint.png

