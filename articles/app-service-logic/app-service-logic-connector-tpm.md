<properties 
   pageTitle="Logic Apps での BizTalk 取引先管理コネクタの使用 | Microsoft Azure App Service" 
   description="BizTalk 取引先管理コネクタまたは API アプリを作成、構成して、Azure App Service のロジック アプリで使用する方法" 
   services="app-service\logic" 
   documentationCenter=".net,nodejs,java" 
   authors="rajeshramabathiran" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="11/30/2015"
   ms.author="rajram"/>

# BizTalk 取引先管理の使用開始とロジック アプリへの追加
BizTalk 取引先管理 (TPM) サービスにより、パートナーや契約などの企業間の関係を、スキーマや証明書などの関連する成果物と共に定義して保持することができます。 これらの関係は、AS2、EDIFACT、および X12 などの関連する API サービスによって適用できます。

TPM API アプリは、AS2 コネクタ、X12 API アプリ、または EDIFACT API アプリの基本要件です。 BizTalk 取引先管理をビジネス ワークフローに追加し、ロジック アプリ内の企業間ワークフローの一部としてデータを処理できます。 

## 前提条件
- 空の SQL Azure データベース - 新しい TPM API App を作成する前に、空の SQL Azure データベースを作成する必要があります。

## パートナー、契約およびプロファイルを理解する
取引先アグリーメントについては、次のようにクリックします。 [ここ][1]します。

## コネクタでできること
コネクタが作成されたため、ロジック アプリを使用してコネクタをビジネス ワークフローに追加できます。 参照してください [Logic Apps とは?](app-service-logic-what-are-logic-apps.md)します。

>[AZURE.NOTE] Azure Logic Apps と Azure アカウントにサインアップする前に開始する場合に、 [ロジック アプリを実行してください](https://tryappservice.azure.com/?appservice=logic), 、App Service で有効期間の短いスターター ロジック アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

表示、Swagger の REST API リファレンス [コネクタと API Apps のリファレンス](http://go.microsoft.com/fwlink/p/?LinkId=529766)します。

パフォーマンス統計をレビューし、コネクタに対するセキュリティを制御することもできます。 参照してください [の管理し、監視、組み込みの API Apps とコネクタ](app-service-logic-monitor-your-connectors.md)します。

<!--References-->
[1]: app-service-logic-create-a-trading-partner-agreement.md


