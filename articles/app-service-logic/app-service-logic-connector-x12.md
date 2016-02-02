<properties 
   pageTitle="Logic Apps での BizTalk X12 コネクタの使用 | Microsoft Azure App Service" 
   description="BizTalk X12 コネクタまたは API アプリを作成、構成して、Azure App Service のロジック アプリで使用する方法" 
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


# BizTalk X12 API アプリの使用開始とロジック アプリへの追加

BizTalk X12 サービスを使用して、X12 プロトコルに従って企業間通信のメッセージを送受信します。 X12 は、一般に ASC X12 (Accredited Standards Committee X12) とも呼ばれており、さまざまな業界で広く使用されています。

BizTalk X12 API アプリをビジネス ワークフローに追加し、ロジック アプリ内の企業間ワークフローの一部としてデータを処理できます。


## 前提条件

- TPM API App: x12 を作成する前に、コネクタを作成する必要がある、 [BizTalk 取引先管理コネクタ ][1]します。
- SQL Azure データベース: B2B API Apps それぞれに、専用の Azure SQL Database が必要です。
- Azure Service Bus: これはオプションであり、バッチ処理の場合にのみ使用されます。

## BizTalk X12 コネクタを使用する

BizTalk X12 コネクタを使用するには、まず、BizTalk X12 コネクタ API アプリのインスタンスを作成する必要があります。 これは、ロジック アプリの作成時にインラインで作成するか、または Azure Marketplace から BizTalk X12 コネクタ API アプリを選択することによって作成できます。

## BizTalk X12 コネクタを構成する

取引先は、B2B (企業間) コミュニケーションに含まれるエンティティです。 2 つのパートナーがある関係を確立した場合、この関係は "契約" と呼ばれます。 契約は、2 つのパートナーが達成したいと考えているコミュニケーションに基づいて定義され、プロトコルまたはトランスポートに固有のものです。

取引先アグリーメントを作成する手順については [ここで ][2]します。

## Logic Apps デザイナー画面で X12 コネクタを使用する

トリガーまたはアクションとして、X12 コネクタを使用できます。

### トリガー

- Azure Logic Apps フロー デザイナーを起動します。
- [X12] をクリックしてコネクタが右側のペインから。  
![トリガー設定][3]
- -> をクリックします。  
![トリガー オプション][4]
- BizTalk X12 コネクタでは 1 つのトリガーが表示されます。 選択 *バッチをリリース*:  
![バッチ入力のリリース][5]
- このトリガーには、入力がありません。 -> をクリックします。  
![リリース バッチの構成][6]
- 出力の一部として、コネクタは、メッセージがバッチ処理されているかどうかの情報だけでなく X12 ペイロードと契約 ID も返します。

### アクション

- [X12] をクリックしてコネクタが右側のペインから。  
![アクションの設定][7]
- -> をクリックします。  
![アクションの一覧][8]
- X12 コネクタは、多くのアクションをサポートしています。 選択 *エンコード*:  
![入力のエンコード][9]
- アクションの入力を指定し、それを構成します。  
![エンコードの構成][10]

 パラメーター| 型| パラメーターの説明
---|---|---
 コンテンツ| 文字列| XML メッセージ
 契約 ID| 文字列| 契約 ID
 パッチ処理されたメッセージ| 文字列| バッチ処理されたメッセージである

このアクションは、X12 ペイロードを含むオブジェクトを返します。

## コネクタでできること

これでコネクタが作成されたため、このコネクタを Logic App を使用してビジネス フローに追加することができます。 を参照してください [Logic Apps とは?](app-service-logic-what-are-logic-apps.md).
>[AZURE.NOTE] Azure Logic Apps と Azure アカウントにサインアップする前に開始する場合に、 [ロジック アプリを実行してください](https://tryappservice.azure.com/?appservice=logic), 、App Service で有効期間の短いスターター ロジック アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

表示、Swagger の REST API リファレンス [コネクタと API Apps のリファレンス](http://go.microsoft.com/fwlink/p/?LinkId=529766)します。

パフォーマンス統計をレビューし、コネクタに対するセキュリティを制御することもできます。 参照してください [管理と監視 API アプリとコネクタの](../app-service-api/app-service-api-manage-in-portal.md)します。




[1]: app-service-logic-connector-tpm.md 
[2]: app-service-logic-create-a-trading-partner-agreement.md 
[3]: ./media/app-service-logic-connector-x12/TriggerSettings.PNG 
[4]: ./media/app-service-logic-connector-x12/ListOfTriggers.PNG 
[5]: ./media/app-service-logic-connector-x12/ReleaseBatchTriggerInput.PNG 
[6]: ./media/app-service-logic-connector-x12/ReleaseBatchTriggerConfigured.PNG 
[7]: ./media/app-service-logic-connector-x12/ActionSettings.PNG 
[8]: ./media/app-service-logic-connector-x12/ListOfActions.PNG 
[9]: ./media/app-service-logic-connector-x12/EncodeInput.PNG 
[10]: ./media/app-service-logic-connector-x12/EncodeConfigured.PNG 
[11]: ./media/app-service-logic-connector-x12/TriggerSettings.PNG 

