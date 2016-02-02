<properties 
   pageTitle="Azure Search サービスでの使用状況と統計の監視 | Microsoft Azure | ホスト型クラウド検索サービス" 
   description="Microsoft Azure のホスト型クラウド検索サービスである Azure Search のリソース使用量とインデックス サイズを追跡記録します。" 
   services="search" 
   documentationCenter="" 
   authors="HeidiSteen" 
   manager="mblythe" 
   editor=""
   tags="azure-portal"/>

<tags
   ms.service="search"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required" 
   ms.date="11/04/2015"
   ms.author="heidist"/>


# Azure Search サービスでの使用状況と統計の監視

インデックスとドキュメントのサイズの増加率を追跡することで、サービスに設定した上限に達する前に、容量を事前に調整することができます。

リソースの使用状況を監視するには、数値と統計が簡単に表示、 [Azure Classic Portal](https://portal.azure.com), 、取得することも、情報プログラムを使用してカスタム サービス管理ツールを構築する場合は、です。 この記事では、この両方の手法について手順を説明します。

## ポータルでの数値とメトリックの表示

1. サインイン、 [Azure クラシック ポータル](https://portal.azure.com)します。

2. Azure Search サービスのサービス ダッシュボードを開きます。 [ホーム] ページに、サービスのタイルが表示されます。またはジャンプ バーの [参照] で、サービスを参照することもできます。 参照してください [サービスを作成する](search-create-service-portal.md) 詳しい説明を参照します。

[使用] セクションには、使用可能なリソースのうち、現在使用中の割合を示すメーターがあります。

  ![][1]

共有サービスのレプリカとパーティションは、それぞれ最大で 1 つであることに注意してください。 また、サポート可能であるのは、合計 10,000 のドキュメントまたは 50 MB のデータまで (いずれかの先に達した方まで) です。

## REST API を使用したインデックス統計情報の取得

Azure Search REST API または .NET SDK を使用することにより、プログラムからサービス メトリックにアクセスできます。 使用している場合は、 [インデクサー](https://msdn.microsoft.com/library/azure/dn946891.aspx) 追加 API は必要な数値を取得できる利用可能な Azure SQL Database または DocumentDB からインデックスを読み込めません。

  + [インデックス統計を取得します。](https://msdn.microsoft.com/library/azure/dn798942.aspx)
  + [ドキュメントのカウント](https://msdn.microsoft.com/library/azure/dn798924.aspx)
  + [インデクサー状態を取得します。](https://msdn.microsoft.com/library/azure/dn946884.aspx)

## 次のステップ

レビュー [制限および制約](search-limits-quotas-capacity.md) パーティションとレプリカを既存の容量が不十分な場合に必要がありますの組み合わせを決定します。

参照してください [Microsoft Azure で検索サービスを管理する](search-manage.md) サービスの管理の詳細についてです。



[1]: ./media/search-monitor-usage/AzureSearch-Monitor1.PNG 

