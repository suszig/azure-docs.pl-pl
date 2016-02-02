<properties
    pageTitle="大規模なデプロイ"
    description="Azure Toolkit for Eclipse を使用して大規模なデプロイを行う方法について説明します。"
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="11/30/2015" 
    ms.author="robmcm"/>




# 大規模なデプロイ

デプロイの規模が大きすぎて既定の approot フォルダーに格納できない場合は、JDK とアプリケーション サーバーのデプロイ ルート フォルダーとしてローカル ストレージ リソースを使用できます。

## 大規模なデプロイのデプロイ ルート フォルダーとしてローカル ストレージ リソースを使用するには

1. 新しいローカル ストレージ リソースを作成します。 リソースの名前は問いません。 ストレージ リソースはロール レベルで定義されます。 新しいローカル ストレージ リソースを作成するためのローカル ストレージ構成ダイアログにアクセスする最も簡単な方法は、次の手順に従うことです。**プロジェクト エクスプ ローラー** ビューでロールを右クリックします (ロールが表示されていない場合は、Azure プロジェクト ノードを展開します)。**[Azure]** をクリックし、**[Local Storage]** をクリックします。 **[Local Storage]** ダイアログで、**[Add]** をクリックして新しいローカル ストレージ リソースを作成します。
1. 2048 MB 以上の目的のサイズを設定します (このサイズより小さくすると、approot のように同一ファイル サイズ問題が発生する可能性があります)。
1. **[Clean the contents when the role instance is recycled]** がオンになっていることを確認します。これで、ロール インスタンスがリサイクルされるときに、デプロイのスタートアップ ロジックがリソース内の既存のファイルと競合することを防止できます。
1. **[Environment variable storing the resource's directory path after deployment]** の値が文字列 **DEPLOYROOT** に設定されていることを確認します。 ローカル ストレージ リソースのダイアログには、次のようになります。
    ![][ic667943]

ローカル リソースの*名前*として **DEPLOYROOT** を使用し、自動的に生成される環境変数 (この場合は **DEPLOYROOT_PATH** に設定されます) を変更しないという操作でも、アプリケーションは問題なく動作します。

ローカル ストレージ リソースの作成に関する追加情報が掲載されて [ローカル ストレージのプロパティの][]します。

## 関連項目

[Azure Toolkit for Eclipse の][]

[Eclipse ので、Azure の Hello World アプリケーションを作成します。][]

[Azure Toolkit for Eclipse のをインストールします。][]

Java で Azure を使用する方法の詳細については、次を参照してください。、 [Azure Java デベロッパー センターの []][]します。






[azure java developer center]: http://go.microsoft.com/fwlink/?LinkID=699547 
[azure toolkit for eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529 
[creating a hello world application for azure in eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533 
[installing the azure toolkit for eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546 
[local storage properties]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties 
[ic667943]: ./media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png 

