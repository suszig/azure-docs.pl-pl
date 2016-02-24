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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->

# 大規模なデプロイ #

デプロイの規模が大きすぎて既定の approot フォルダーに格納できない場合は、JDK とアプリケーション サーバーのデプロイ ルート フォルダーとしてローカル ストレージ リソースを使用できます。

## 大規模なデプロイのデプロイ ルート フォルダーとしてローカル ストレージ リソースを使用するには ##

1. 新しいローカル ストレージ リソースを作成します。 リソースの名前は問いません。 ストレージ リソースはロール レベルで定義されます。 元となる新しいローカル ストレージ リソースを作成することが、ローカル記憶域の構成] ダイアログにアクセスする最も簡単な方法は、次の手順を使用して、: でロールを右クリックして、 **プロジェクト エクスプ ローラー** ビュー (ロールが表示されない場合、Azure プロジェクト ノードを展開し、) をクリックして **Azure**, 、順にクリック **ローカル記憶域**します。 内で、 **ローカル記憶域** ダイアログ ボックスで、をクリックして **追加** 新しいローカル ストレージ リソースを作成します。
1. 2048 MB 以上の目的のサイズを設定します (このサイズより小さくすると、approot のように同一ファイル サイズ問題が発生する可能性があります)。
1. いることを確認 **ロール インスタンスがリサイクルされるときに、内容をクリーンアップ** がチェックされてこれによって、デプロイの起動ロジックがロール インスタンスがリサイクルされるときに既存のファイルとリソースの競合を実行していることを防止します。
1. いることを確認、 **展開後にリソースのディレクトリ パスを格納する環境変数** 値が文字列に設定されている **DEPLOYROOT**します。 ローカル ストレージ リソースのダイアログには、次のようになります。
    ![][ic667943]

またはを使用する場合 **DEPLOYROOT** として、 *名前* して、ローカル リソースの名を変更しないで、自動的に生成された環境変数 (に設定されます **DEPLOYROOT_PATH** 場合) も、アプリケーションに対して機能します。

ローカル ストレージ リソースの作成に関する追加情報が掲載されて [ローカル ストレージ プロパティ][]します。

## 関連項目 ##

[Azure Toolkit for Eclipse][]

[Azure 向け Hello World アプリケーションを Eclipse で作成する][]

[Azure Toolkit for Eclipse のインストール][] 

Java で Azure を使用する方法の詳細については、次を参照してください。、 [Azure Java デベロッパー センター][]します。

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Local storage properties]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: ./media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png

