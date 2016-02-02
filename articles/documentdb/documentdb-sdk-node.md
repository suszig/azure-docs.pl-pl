<properties 
    pageTitle="DocumentDB Node.js SDK | Microsoft Azure" 
    description="リリース日、提供終了日、DocumentDB Node.js SDK の各バージョン間の変更など、Node.js SDK に関するあらゆる詳細を提供します。" 
    services="documentdb" 
    documentationCenter="nodejs" 
    authors="ryancrawcour" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="11/16/2015" 
    ms.author="ryancraw"/>


# DocumentDB SDK

> [AZURE.SELECTOR]
- [.NET SDK](documentdb-sdk-dotnet.md)
- [Node.js SDK](documentdb-sdk-node.md)
- [Java SDK](documentdb-sdk-java.md)
- [Python SDK](documentdb-sdk-python.md)


## DocumentDB Node.js SDK

<table>
<tr><td>**Download**</td><td>[NPM](https://www.npmjs.com/package/documentdb)</td></tr>
<tr><td>**Contribute**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-node/tree/master/source)</td></tr>
<tr><td>**ドキュメント**</td><td>[Node.js SDK リファレンス ドキュメント](http://azure.github.io/azure-documentdb-node/)</td></tr>
<tr><td>**サンプル**</td><td>[Node.js コード サンプル](https://github.com/Azure/azure-documentdb-node/tree/master/samples)</td></tr>
<tr><td>**開始**</td><td>[Node.js SDK を使ってみる](documentdb-nodejs-get-started.md)</td></tr>
<tr><td>**Current Supported Platform**</td><td>[Node.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/)<br/>[Node.js v0.12](https://nodejs.org/en/blog/release/v0.12.0/)<br/>[Node.js v4.2.0](https://nodejs.org/en/blog/release/v4.2.0/)</td></tr>
</table></br>

## リリース ノート

### <a name="1.4.0"/>1.4.0</a>

- Upsert を実装します。 documentClient の新しい upsertXXX メソッド

### <a name="1.3.0"/>1.3.0</a>

- 他の SDK とバージョン番号をそろえるため、このリリースはスキップされました

### <a name="1.2.2"/>1.2.2</a>

- Q promises ラッパーを新しいリポジトリに分割します
- 更新し、npm レジストリのファイルをパッケージ化します

### <a name="1.2.1"/>1.2.1</a>

- ID ベースのルーティングを実装する
- 問題が解決 [番号 49](https://github.com/Azure/azure-documentdb-node/issues/49) -メソッド current() と競合して現在のプロパティ

### <a name="1.2.0"/>1.2.0</a>

- 地理空間インデックスのサポートを追加しました。
- すべてのリソースの id プロパティを検証します。 リソースの ID には ?、/、#、&#47;&#47; 文字を使えず、終わりの文字をスペースにできません。
- ResourceResponse に新しいヘッダーの「インデックス変換の進行状況」を追加します。

### <a name="1.1.0"/>1.1.0</a>

- V2 インデックス作成ポリシーを実装する

### <a name="1.0.3"/>1.0.3</a>

- コアと約束 SDK 内の実装の問題 [#40] (https://github.com/Azure/azure-documentdb-node/issues/40) - eslint および grunt 構成

### <a name="1.0.2"/>1.0.2</a>

- 問題 [#45](https://github.com/Azure/azure-documentdb-node/issues/45) -Promise ラッパーではエラーのため、ヘッダーは含まれません。

### <a name="1.0.1"/>1.0.1</a>

- readConflicts、readConflictAsync、queryConflicts を追加し、競合に関して問い合わせる機能を実装しました
- API ドキュメントを更新しました
- 問題 [#41](https://github.com/Azure/azure-documentdb-node/issues/41) -client.createDocumentAsync エラー

### <a name="1.0.0"/>1.0.0</a>

- GA SDK

## リリース日と提供終了日

Microsoft は、新しい/サポートされるバージョンに速やかに移行する目的で、SDK の提供終了を少なくともその **12 か月**前に通知します。

新機能および機能と最適化が現在の SDK にのみ追加、そのためには、アップグレードすること勧め常に、最新の SDK バージョンにできるだけ早くできるだけします。

提供終了の SDK で DocumentDB に要求した場合、サービスにより却下されます。
> [AZURE.WARNING]
バージョン **1.0.0** 以前のすべてのバージョンの Azure DocumentDB SDK for Node.js は **2016 年 2 月 29 日**で提供が終了します。 

<br/>

| バージョン| リリース日| 提供終了日
| ---     | ---          | ---
| [1.4.0](#1.4.0)| 2015 年 10 月 6 日| ---
| [1.3.0](#1.3.0)| 2015 年 10 月 6 日| ---
| [1.2.2](#1.2.2)| 2015 年 9 月 10 日| ---
| [1.2.1](#1.2.1)| 2015 年 8 月 15 日| ---
| [1.2.0](#1.2.0)| 2015 年 8 月 5 日| ---
| [1.1.0](#1.1.0)| 2015 年 7 月 9 日| ---
| [1.0.3](#1.0.3)| 2015 年 6 月 4 日| ---
| [1.0.2](#1.0.2)| 2015 年 5 月 23 日| ---
| [1.0.1](#1.0.1)| 2015 年 5 月 15 日| ---
| [1.0.0](#1.0.0)| 2015 年 4 月 8 日| ---
| 0.9.4-prelease| 2015 年 4 月 6 日| 2016 年 2 月 29 日
| 0.9.3-prelease| 2015 年 1 月 14 日| 2016 年 2 月 29 日
| 0.9.2-prelease| 2014 年 12 月 18 日| 2016 年 2 月 29 日
| 0.9.1-prelease| 2014 年 8 月 22 日| 2016 年 2 月 29 日
| 0.9.0-prelease| 2014 年 8 月 21 日| 2016 年 2 月 29 日


## FAQ

[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## 関連項目

DocumentDB の詳細については、次を参照してください。 [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) サービス ページです。





