<properties
   pageTitle="Marketplace 用のデータ サービスを作成するための技術的前提条件 | Microsoft Azure"
   description="Azure Marketplace でデプロイおよび販売するデータ サービスを作成するための要件について説明します。"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager=""
   editor=""/>

<tags
   ms.service="marketplace-publishing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="11/30/2015"
   ms.author="hascipio; avikova" />


# Azure Marketplace 用のデータ サービス プランを作成するための技術的前提条件

始める前にプロセスをよく読み、各ステップをどこで、なぜ実行するのかを理解してください。 可能な限り、会社の情報と他のデータを準備し、必要なツールをダウンロードし、技術コンポーネントを作成してから、プラン作成プロセスを開始する必要があります。

プロセスを開始する前に、次の項目を準備してください。

## データ サービス プランの公開に使用するテクノロジの決定

発行者は、Azure Marketplace でデータ サービスを発行する際に、複数のテクノロジの中から決定できます。 サポートされている主要なテクノロジは以下で説明します。 データ サービスの発行に使用されるテクノロジに関係なく、Azure Marketplace サービスによって公開される **OData フィード**を通してエンド ユーザーはデータを消費します。 に関する詳細情報が見つかります OData サービスで [http://www.odata.org/](http://www.odata.org/)

## SQL Azure データベース

SQL Azure でデータセットを準備することは発行者の責任です。 Azure にサブスクライブし、適切なサイズのデータベースをプロビジョニングし、データを SQL Azure DB にアップロードする必要があります。 また、発行者はデータを常に最新に保つ責任もあります。 詳細については、で検索できる Azure サービスにサブスクライブする [https://azure.microsoft.com/services/sql-database/](https://azure.microsoft.com/services/sql-database/)


データを SQL Azure に移動する際、Azure Marketplace はテーブルとビューを公開できます。 発行者はどのテーブル、ビュー、および列をエンド ユーザーに公開するか指定できます。 さらに、コンテンツ プロバイダーも、どの列をエンド ユーザがクエリでき、どれをペイロードに返されるだけにするかを指定できます。 これにより、データベースのどのデータを公開するかについて、高い柔軟性を提供できます。 クエリ可能な列は、1 つまたは複数のデータベースのインデックスに支持されている必要があります。

## REST ベースの Web サービス

サポートされているプロトコル: **HTTPS のみ**

既存の REST ベース サービスは、Azure Marketplace を通じて公開できます。 データセットが常に OData フィードとしてエンドユーザーに公開されているため、Azure Marketplace サービスは OData ベースのサービスにサービスをマップできる必要があります。 そのためには、REST ベースのエンドポイントがすべてのパラメーターを HTTP パラメーターとして公開する必要があります。

ペイロードは、ATOM 応答にマップできる形式である必要があります。 そのため、サービスからの応答は XML 形式である必要があり、ペイロード値 (レコード セットなど) を含む 1 つの反復要素のみを含めることができます。 Azure Marketplace サービスは繰り返しノードを ATOM のエントリ ノードへ、エントリ ノード内ではペイロード値をプロパティ ノードにマップします。

API キー、認証トークンなど) などの認証情報 HTTP パラメーターとして、または HTTP ヘッダー (キー値のペア) – に基本認証はサポートされても提供する必要があります。 有効なキーが提供され、Azure Marketplace を介すすべての要求がそのキーによって作成される必要があります。 Azure Marketplace レイヤーではユーザーの監視および課金を行います。

サービスによって返されたエラーは HTTP 状態コードにマップされる必要があります。 サービスがエラーを含む XML を返した場合、これらのエラーは Azure Marketplace サービスによって HTTP 状態コードにマップされます。

## SOAP ベースの Web サービス

プロトコル: **HTTPS のみ**

要件は REST ベース サービス セクションのものと同じです。 唯一の違いは、Azure Marketplace を介して要求が作成されるごとに発行者のサービスに対して通知される XML 本文でもパラメーターが提供される点にあります。 これは、ユーザーがフロント エンドで提供する HTTP パラメーターは、要求と共にコンテンツ プロバイダーの Web サービスに通知される XML ドキュメントの XML 要素に変換されることを意味します。

## OData ベースの Web サービス

プロトコル: **HTTPS のみ**

データは OData サービスとして Azure Marketplace に公開できます。 システムはサービスを通過し、サービスのルートを Azure Marketplace サービスのルートに置換します。これはすべての後続の呼び出しが Azure Marketplace を介して確実に行われるようにするためです。

OData サービスは、バックエンドのデータベースとの競合が不要なだけではありません。 OData はサービスを実行するあらゆる種類のストレージまたはビジネス ロジックをサポートします。


## 次のステップ

これの前提条件を確認し、必要なタスクを完了すると、進むで詳述するようデータ サービス プランを作成すると、 [データ サービスの発行ガイド](marketplace-publishing-data-service-creation.md)します。

またはの各公開の段階のプロセス全体およびそれぞれの記事を確認したい場合は、記事を参照してください [作業の開始: オファーを Azure Marketplace に発行する方法](marketplace-publishing-getting-started.md)します。


[link-acct]: marketplace-publishing-accounts-creation-registration.md 

