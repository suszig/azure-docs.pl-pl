<properties 
    pageTitle="Visual Studio からのデプロイ" 
    description="Visual Studio でプロジェクトを作成してロジック アプリを管理します。" 
    authors="stepsic-microsoft-com" 
    manager="dwrede" 
    editor="" 
    services="app-service\logic" 
    documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/29/2015"
    ms.author="stepsic"/> 


# Visual Studio からのデプロイ

[Azure ポータル](https://portal.azure.com) 設計およびロジック アプリを管理する優れた方法は、代わりに Visual Studio からロジック アプリを展開することもできます。 これには主に 2 つの機能があります。

- アプリケーションのすべての側面を含めることができるように、ソリューションに他のアセットと共にロジック アプリを保存する
- TFS や Git を使用して変更履歴を追跡できるように、ロジック アプリ定義をソース管理にチェックインしたままにする

次の手順を実行するには、Azure SDK 2.7 以上をインストールしておく必要があります。 検索 [最新の SDK for VS](http://azure.microsoft.com/downloads/) ここです。

## プロジェクトの作成

1. 移動して、 **ファイル** メニューをクリック **新規** >  **プロジェクト** (またはに移動できます **追加** し、[ **新しいプロジェクト** 既存のソリューションに追加する)
    ![[ファイル] メニュー](./media/app-service-logic-deploy-from-vs/filemenu.png)

2. ダイアログ ボックスで、**[クラウド]** を見つけて、**[Azure リソース グループ]** を選択します。 型 a **名** ] をクリックし、 **OK**します。
    ![新しいプロジェクトの追加](./media/app-service-logic-deploy-from-vs/addnewproject.png)

3. 必要に応じて、**[ロジック アプリ]** または **[ロジック アプリと API アプリ]** を選択する必要があります。 **[ロジック アプリ]** を選択する場合は、既存の API を指定する必要があります。 選択した場合 **ロジック アプリと API アプリ** し、同時に、新しい空の API アプリを作成することもできます。
    ![Azure テンプレートの選択](./media/app-service-logic-deploy-from-vs/selectazuretemplate.png)

4. **テンプレート**を選択したら、**[OK]** をクリックします。

これで、ロジック アプリ プロジェクトがソリューションに追加されます。 ソリューション エクスプ ローラーで展開を確認する必要があります。
![デプロイ](./media/app-service-logic-deploy-from-vs/deployment.png)

## ロジック アプリの構成

プロジェクトを作成したら、Visual Studio 内でロジック アプリの定義を編集することができます。 ソリューション エクスプローラーで JSON ファイルをクリックします。 アプリケーションのロジックを使用して入力できるプレース ホルダーの定義が表示されます。

定義全体で**パラメーター**を使用することをお勧めします。 これは、開発環境と運用環境の両方にデプロイする場合に役立ちます。 その場合に環境固有のすべての構成を配置する必要があります、 `.param` ファイル、および実際の文字列ではなくパラメーター。

現在、Visual Studio には組み込みの JSON デザイナーがありません。そのため、(JSON を記述するのではなく) グラフィカル インターフェイスを使用する場合は、Azure ポータルを使用する必要があります。

以前の Azure ポータル内のロジック アプリを作成してソース管理にチェックインしたい場合は、これを実現する 3 つの方法があります。
- 移動して **コード ビュー** ポータルと、定義をコピーします。
- ロジック アプリを使用して [REST API](https://msdn.microsoft.com/library/azure/dn948510.aspx) 、定義を取得します。
- 使用 [Azure リソース マネージャーの powershell](../powershell-azure-resource-manager.md),  、具体的には、[`Get-azureresource` コマンド] を定義をダウンロードするには、(https://msdn.microsoft.com/library/dn654579.aspx)。

## ロジック アプリのデプロイ

最後に、アプリを構成したら、いくつかの手順で Visual Studio から直接デプロイすることができます。

1. ソリューション エクスプ ローラーでデプロイを右クリックしに移動 **展開** > **新しいデプロイ]**
    ![新しいデプロイ](./media/app-service-logic-deploy-from-vs/newdeployment.png)

2. Azure サブスクリプションへのサインインを求められます。

3. ロジック アプリをデプロイするリソース グループの詳細] を選択する必要があります。 
    ![リソース グループへのデプロイ](./media/app-service-logic-deploy-from-vs/deploytoresourcegroup.png)

    リソース グループに適したテンプレートとパラメーター ファイルを必ず選択してください (たとえば、運用環境にデプロイする場合、運用中のパラメーター ファイルを選択します)。

4. 配置の状態が [に移動、 **出力** ウィンドウ (を選択する必要があります **Azure プロビジョニング**します。 
    ![出力](./media/app-service-logic-deploy-from-vs/output.png)

今後は、ソース管理でロジック アプリを修正し、Visual Studio を使用して新しいバージョンをデプロイできます。 Azure ポータルで定義を直接変更した場合、次回 Visual Studio からデプロイすると、変更内容がオーバーライドされることに注意してください。

ツーリングを使用して、ソース管理からロジック アプリをデプロイすると、まだ Visual Studio を使用しない場合は、使用することができます、 [API](https://msdn.microsoft.com/library/azure/dn948510.aspx) または [Powershell](../powershell-azure-resource-manager.md) 直接してデプロイを自動化します。




