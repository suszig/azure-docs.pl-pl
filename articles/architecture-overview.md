<properties 
    pageTitle="Microsoft Azure のアプリケーションのアーキテクチャ" 
    description="一般的な設計パターンを対象とするアーキテクチャの概要" 
    services="" 
    documentationCenter="" 
    authors="Rboucher" 
    manager="jwhit" 
    editor="mattshel"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/16/2015" 
    ms.author="robb"/>

#Microsoft Azure のアプリケーションのアーキテクチャ
Microsoft Azure を使用するアプリケーションを構築するためのリソースです。 これには、ソフトウェア システムを視覚的に説明する図を描画するためのツールが含まれます。 



##Azure のアーキテクチャ設計パターン
Microsoft は、お客様独自の設計を作成するのに便利な一連のアーキテクチャ設計パターンを公開しています。 これらのパターンは、簡潔なアーキテクチャのガイドとして組み合わせて利用することで、組織のビジネス ニーズを解決するための Microsoft Azure Platform の最適な活用方法に関するガイダンスとなります。


[概要](../azure-architectures-cpif-overview/) - 
[ハイブリッド ネットワーク](../azure-architectures-cpif-infrastructure-hybrid-networking/) - 
[オフサイトのバッチ処理](../azure-architectures-cpif-foundation-offsite-batch-processing-tier/) -
[複数サイト データ層](../azure-architectures-cpif-foundation-multi-site-data-tier/) -
[グローバル負荷分散 web 層](../azure-architectures-cpif-foundation-global-load-balanced-web-tier/) -
[Azure search 層](../azure-architectures-cpif-foundation-azure-search-tier/)
 
各パターンが含まれています
 
- サービスの説明
- パターンを活用するために必要な Azure サービスの一覧
- アーキテクチャ図
- アーキテクチャの依存関係
- パターンに影響を与える可能性がある設計上の制限または考慮事項
- インターフェイスとエンドポイント
- アンチ パターン
- 可用性と回復性、使用されるサービスの複合 SLA、規模とパフォーマンス、コスト、運用についての考慮事項など、アーキテクチャに関する重要な考慮事項の概要。

![Azure のアーキテクチャ設計パターン](./media/architecture-overview/AzureArchPatterns.jpg)


##設計パターンのポスター
Microsoft Patterns and Practices に書籍が発行 [クラウド設計パターン](http://msdn.microsoft.com/library/dn568099.aspx) は MSDN と PDF ダウンロードの両方に使用します。 すべてのパターンを一覧表示する大型のポスターもあります。 

![パターンとプラクティス クラウド パターンのポスター](./media/architecture-overview/PnPPatternPosterThumb.jpg)



##Microsoft のアーキテクチャの認定コース

Microsoft では、先日マイクロソフト認定資格試験 70-534 をサポートする新しいアーキテクチャ コースをリリースしました。  [EDX.ORG 無償で入手](https://www.edx.org/course/architecting-microsoft-azure-solutions-microsoft-dev205x)します。  新しい [3D 設計図 Visio テンプレート](#3d-blueprint-visio-template)します。 

![Microsoft のアーキテクチャの認定コース](./media/architecture-overview/EDXCourse.png)


##Microsoft のアーキテクチャの設計図

Microsoft は、高レベルのセットを公開 [アーキテクチャ ブルー プリント](http://aka.ms/azblueprints) 特定の型の Microsoft 製品を使用してシステムを構築する方法を表示します。 

各ブループリントには、以下が含まれています。

- フラット **2D Visio** 2003年ベースのファイルをダウンロードして変更できます。 
- カラフルな **3D 描写 PDF** 近年、技術性の低い青写真を紹介するファイル
- **ビデオ** 、3 D バージョンを手順を説明します。 

設計図を使用して、 [クラウドおよびエンタープライズ用シンボル セット](#symbol-and-icon-sets)します。   

![[Microsoft アーキテクチャ ブループリント 3D] ダイアグラム](./media/architecture-overview/BluePrintThumb.jpg)



##3D ブループリント Visio テンプレート

3D のバージョンの [Microsoft アーキテクチャ ブルー プリント](http://aka.ms/azblueprints) Microsoft 以外のツールで最初に作成されました。 一部として、2015 年 8 月 5 日に、新しい Visio 2013 (以降) テンプレートに組み込まれていた、 [EDX.ORG で配布される Microsoft のアーキテクチャ認定コース](#microsoft-architecture-certification-course)します。 

テンプレートは、このコース以外でも利用もできます。 

- [トレーニング ビデオを見る](http://aka.ms/3dBlueprintTemplateVideo) 何ができるとわかるように、まず   
- ダウンロード、 [Microsoft 3d 設計図 Visio テンプレート](http://aka.ms/3DBlueprintTemplate)
- ダウンロード、 [クラウドおよびエンタープライズのシンボル](#drawing-symbol-and-icon-sets) 3D のテンプレートを使用します。 

電子メールにて [CnESymbols@microsoft.com](mailto:CnESymbols@microsoft.com) トレーニング資料またはフィードバックの送信には回答されていない特定の質問についてです。 使いやすさは、テンプレートの主な目標の 1 つです。よかった点や問題点についてお知らせください。

![Microsoft 3D ブループリント Visio テンプレート](./media/architecture-overview/3DBlueprintVisioTemplate.jpg)



##シンボルとアイコン セットの描画 

[Visio とシンボルのトレーニング ビデオを表示](http://aka.ms/CnESymbolsVideo) し [クラウドをダウンロードして、エンタープライズのシンボル設定](http://aka.ms/CnESymbols) Azure、Windows Server、SQL Server などを記述する技術資料を作成するためにします。 これらのシンボルやアイコンは、アーキテクチャ ダイアグラム、トレーニング資料、プレゼンテーション、データシート、インフォグラフィックス、ホワイトペーパーに加え、サード パーティの書籍でも使用できます (Microsoft 製品の使用が想定された書籍の場合)。 ただし、ユーザー インターフェイスで使用することは、想定されていません。

CnE シンボルは、Visio 形式と PNG 形式で提供されます。 セットには、PowerPoint での PNG ファイルの使用方法に関する追加の手順が含まれています。 

シンボル セットは四半期に 1 回発行され、新しいサービスがリリースされるたびに更新されます。 

Microsoft Office と関連技術の追加のシンボルをで使用できる、 [Microsoft Office Visio ステンシル](http://www.microsoft.com/en-us/download/details.aspx?id=35772), CnE セットのようなアーキテクチャ ダイアグラム向けに最適化されてはいませんが、します。   

**に関するフィードバック:** CnE シンボルを使用している場合は、短い 5 つの質問によって記入 [アンケート](http://aka.ms/azuresymbolssurveyv2) 電子メールにてまたは [CnESymbols@microsoft.com](mailto:CnESymbols@microsoft.com) 具体的な質問や議論のためです。 ご意見、ご感想をお寄せください。この機能を継続的に改善できるよう、役立てたいと考えております。 

![クラウドおよびエンタープライズ用シンボル/アイコン セット](./media/architecture-overview/CnESymbols.png)


##アーキテクチャのインフォグラフィック

Microsoft は、ポスター/インフォグラフィックに関連するいくつかのアーキテクチャを公開しています。 含まれる [実際のクラウド アプリケーションの作成](http://azure.microsoft.com/documentation/infographics/building-real-world-cloud-apps/) と [クラウド サービスを使用したスケール](http://azure.microsoft.com/documentation/infographics/cloud-services/) します。 

![Azure Architecture Infographics](./media/architecture-overview/AzureArchInfographicThumb.jpg)

