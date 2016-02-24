<properties 
   pageTitle="IntelliTrace および Visual Studio を使用した発行済みのクラウド サービスのデバッグ | Microsoft Azure"
   description="IntelliTrace および Visual Studio を使用した発行済みのクラウド サービスのデバッグ"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="tlee" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/14/2015"
   ms.author="tarcher" />



# IntelliTrace および Visual Studio を使用した発行済みのクラウド サービスのデバッグ

##概要

IntelliTrace を使用すると、ロール インスタンスを Azure で実行する際に広範なデバッグ情報を記録できます。 問題の原因を調べる必要がある場合は、IntelliTrace ログを使用して、コードが Azure で実行されているかのように Visual Studio から調査することができます。 実際には、IntelliTrace は Azure アプリケーションが Azure のクラウド サービスとして実行されている際にキー コードの実行および環境データを記録し、その記録されたデータを Visual Studio で再生できるようにします。 代わりに、リモート デバッグを使用して、Azure で実行されているクラウド サービスに直接接続することができます。 参照してください [クラウド サービスのデバッグ](http://go.microsoft.com/fwlink/p/?LinkId=623041)します。

>[AZURE.IMPORTANT] IntelliTrace は、デバッグ シナリオのみを目的としていますが、運用環境のデプロイを使用しないで

>[AZURE.NOTE] Visual Studio Enterprise がインストールされている必要がある場合は、IntelliTrace と、Azure アプリケーションのターゲット .NET Framework 4 またはそれ以降のバージョンを使用することができます。 IntelliTrace は、Azure ロールの情報を収集します。 これらのロールの仮想マシンは、常に 64 ビット オペレーティング システムを実行します。

## IntelliTrace の Azure アプリケーションを構成するには

IntelliTrace を Azure アプリケーションで有効にするには、Visual Studio Azure プロジェクトからアプリケーションを作成し、発行する必要があります。 IntelliTrace は Azure に発行する前に Azure アプリケーション向けに構成する必要があります。 IntelliTrace を構成せずにアプリケーションを発行したが、後で構成が必要になった場合は、Visual Studio からもう一度アプリケーションを発行する必要があります。 詳細については、次を参照してください。 [Azure ツールを使用してクラウド サービスの発行](http://go.microsoft.com/fwlink/p/?LinkId=623012)します。

1. Azure アプリケーションを配置する準備ができたらとき、に、プロジェクトのビルド ターゲットが設定されていることを確認 **デバッグ**します。

1. ソリューション エクスプ ローラーで、Azure プロジェクトのショートカット メニューを開き、選択 **発行**します。
 
    [Azure アプリケーションの公開] ウィザードが表示されます。

1. クラウドで発行されている場合は、アプリケーションの IntelliTrace ログを収集するには、選択、 **IntelliTrace を有効に** チェック ボックスをオンします。

    >[AZURE.NOTE] IntelliTrace またはプロファイリング、Azure アプリケーションの公開時に有効にすることができます。 両方を有効にすることはできません。

1. 基本の IntelliTrace 構成をカスタマイズするには、選択、 **設定** ハイパーリンクです。

    次の図に示すように、[IntelliTrace の設定] ダイアログが表示されます。 どのイベントを記録するか、呼び出し情報を収集するかどうか、どのモジュールとプロセスのログを収集するか、どれくらいの領域を記録に割り当てるかを指定できます。 IntelliTrace の詳細については、次を参照してください。 [IntelliTrace によるデバッグ](http://go.microsoft.com/fwlink/?LinkId=214468)します。

    ![VST_IntelliTraceSettings](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC519063.png)

IntelliTrace ログは、IntelliTrace 設定で指定された最大サイズの循環ログ ファイルです(既定のサイズは 250 MB)。 IntelliTrace ログは、仮想マシンのファイル システム内のファイルに収集されます。 ログを要求すると、スナップショットがその時点で取得され、ローカル コンピューターにダウンロードされます。

Azure のアプリケーションが Azure に発行されると、次の図のようにサーバー エクスプ ローラーの Azure の計算ノードから IntelliTrace が有効になっているかどうかを判断することができます。

![VST_DeployComputeNode](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC744134.png)

## ロール インスタンスの IntelliTrace ログのダウンロード

ロール インスタンスの IntelliTrace ログをダウンロードすることができます、 **Cloud Services** 内のノード **サーバー エクスプ ローラー**します。 展開、 **Cloud Services** 、興味のあるインスタンスが見つかるまでノードのこのインスタンスのショートカット メニューを開き **[IntelliTrace ログの**です。 IntelliTrace ログは、ローカル コンピューター上のディレクトリのファイルにダウンロードされます。 IntelliTrace ログを要求するたびに、新しいスナップショットが作成されます。

ログをダウンロードすると、Visual Studio の [Azure のアクティビティ ログ] ウィンドウに操作の進行状況が表示されます。 次の図に示すように、操作の行項目を展開して詳細を表示することができます。

![VST_IntelliTraceDownloadProgress](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC745551.png)

IntelliTrace ログのダウンロード中も、Visual Studio での作業を続行することができます。 ログのダウンロードが完了すると、ログが Visual Studio で自動的に開きます。

>[AZURE.NOTE] IntelliTrace ログには、フレームワークが生成して、後で処理する例外が含まれています。 内部のフレームワーク コードでは、このような例外がロールの起動時の正常な処理として生成されるため、無視しても問題ありません。

## 関連項目

[Cloud Services のデバッグ](https://msdn.microsoft.com/library/ee405479.aspx)


