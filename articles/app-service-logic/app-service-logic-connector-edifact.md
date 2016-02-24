<properties 
   pageTitle="Logic Apps での BizTalk Edifact コネクタの使用 | Microsoft Azure App Service" 
   description="BizTalk Edifact コネクタまたは API アプリを作成、構成して、Azure App Service のロジック アプリで使用する方法" 
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

# BizTalk Edifact コネクタの使用開始とロジック アプリへの追加  
Edifact サービスを使用して、Edifact プロトコルに従って企業間通信のメッセージを送受信します。 Edifact は、一般に ASC Edifact (Accredited Standards Committee Edifact) とも呼ばれており、さまざまな業界で広く使用されています。

この BizTalk Edifact コネクタをビジネス ワークフローに追加し、ロジック アプリ内のそのワークフローの一部としてデータを処理できます。 

## 前提条件
- TPM API App: Edifact コネクタを作成する前に作成する必要が、[BizTalk 取引先管理コネクタ] [1] です。
- SQL Azure データベース: B2B API Apps それぞれに、専用の Azure SQL Database が必要です。
- Azure Service Bus: これはオプションであり、バッチ処理の場合にのみ使用されます。

## Edifact コネクタを使用する
Edifact コネクタを使用するにはまず、AS2 コネクタ API アプリのインスタンスを作成する必要があります。 これは、ロジック アプリの作成時にインラインで作成するか、または Azure Marketplace から AS2 コネクタ API アプリを選択することによって作成できます。

## Edifact コネクタを構成する
取引先は、B2B (企業間) コミュニケーションに含まれるエンティティです。 2 つのパートナーがある関係を確立した場合、この関係は "契約" と呼ばれます。 契約は、2 つのパートナーが達成したいと考えているコミュニケーションに基づいて定義され、プロトコルまたはトランスポートに固有のものです。

取引先アグリーメントの作成に必要な手順が記載されているここで [2]。

## Logic Apps デザイナー画面で Edifact コネクタを使用する
トリガーまたはアクションとして、Edifact コネクタを使用できます。

### トリガー
- Azure Logic Apps フロー デザイナーを起動します。
- 右側のペインから、[Edifact コネクタをクリックします。  
![トリガーの設定][3]
- -> をクリックします。  
![トリガーのオプション][4]
- Edifact コネクタでは 1 つのトリガーが表示されます。 選択 *バッチをリリース*:  
![リリース バッチ入力][5]
- このトリガーには、入力がありません。 -> をクリックします。  
![リリース バッチの構成][6]
- 出力の一部として、コネクタは、メッセージがバッチ処理されているかどうかの情報だけでなく Edifact ペイロードと契約 ID も返します。

### アクション
- 右側のペインから、[Edifact コネクタをクリックします。  
![アクションの設定][7]
- -> をクリックします。  
![アクションの一覧][8]
- Edifact コネクタは、多くのアクションをサポートしています。 選択 *エンコード*:  
![入力のエンコード][9]
- アクションの入力を指定し、それを構成します。  
![エンコード設定][10]

    Parameter|種類の種類パラメーターの説明
---|---|---
Content|string|XML メッセージ
アグリーメント ID|integer|契約 ID
Message|boolean| をバッチ処理バッチ処理されたメッセージ
データ要素 Separator|string|データ要素区切り記号
コンポーネント Separator|string|コンポーネントの区切り記号
セグメント Terminator|string|セグメント終端記号
10 進数のポイント Indicator|string|小数点のインジケーター
繰り返し Separator|string|繰り返し区切り記号
エスケープ Character|string|エスケープ文字
置換 Character|string|置換文字
セグメント終端記号 Suffix|string|セグメント終端記号のサフィックス

アクションが正常に完了すると、EDIFACT ペイロードを含むオブジェクトが返されます。

## コネクタでできること
これでコネクタが作成されたため、ロジック アプリを使用してこのコネクタをビジネス フローに追加することができます。 参照してください [Logic Apps とは?](app-service-logic-what-are-logic-apps.md)します。

>[AZURE.NOTE] Azure Logic Apps と Azure アカウントにサインアップする前に開始する場合に、 [ロジック アプリを実行してください](https://tryappservice.azure.com/?appservice=logic), 、App Service で有効期間の短いスターター ロジック アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

表示、Swagger の REST API リファレンス [コネクタと API Apps のリファレンス](http://go.microsoft.com/fwlink/p/?LinkId=529766)します。

パフォーマンス統計をレビューし、コネクタに対するセキュリティを制御することもできます。 参照してください [管理と監視 API アプリとコネクタの](../app-service-api/app-service-api-manage-in-portal.md)です。


<!--References -->
[1]: app-service-logic-connector-tpm.md
[2]: app-service-logic-create-a-trading-partner-agreement.md
[3]: ./media/app-service-logic-connector-edifact/TriggerSettings.PNG
[4]: ./media/app-service-logic-connector-edifact/ListOfTriggers.PNG
[5]: ./media/app-service-logic-connector-edifact/ReleaseBatchTriggerInput.PNG
[6]: ./media/app-service-logic-connector-edifact/ReleaseBatchTriggerConfigured.PNG
[7]: ./media/app-service-logic-connector-edifact/ActionSettings.PNG
[8]: ./media/app-service-logic-connector-edifact/ListOfActions.PNG
[9]: ./media/app-service-logic-connector-edifact/EncodeInput.PNG
[10]: ./media/app-service-logic-connector-edifact/EncodeConfigured.PNG

