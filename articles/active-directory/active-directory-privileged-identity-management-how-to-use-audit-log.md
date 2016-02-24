<properties
   pageTitle="Azure Privileged Identity Management: 監査ログを使用する方法"
   description="Azure Privileged Identity Management 拡張機能で監査ログを使用する方法について説明します。"
   services="active-directory"
   documentationCenter=""
   authors="IHenkel"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="na"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/21/2015"
   ms.author="inhenk"/>

# Azure Privileged Identity Management: 監査ログを使用する方法

## 監査ログの使用
Privileged Identity Management の監査ログを使用すると、特定の期間におけるすべてのユーザー割り当てとアクティブ化を確認できます。

## 監査ログへの移動
PIM ダッシュボードで監査履歴をクリックして、監査ログにアクセスできます。

## 監査ログのグラフ
監査ログを使用すると、合計アクティブ化数、1 日あたりの最大アクティブ化数、1 日あたりの平均アクティブ化数を折れ線グラフで表示できます。  監査履歴に複数のロールがある場合は、ロールによってデータをフィルターすることもできます。

並べ替え時、アクション、またはロールを使用して、 **時間**, 、**アクション** または **ロール** ボタン。

## 監査ログの一覧
監査ログの一覧の列は次のとおりです。

- **リクエスター** -ロールのアクティブ化を要求したユーザー。
- **ユーザー** -ユーザーのロールのアクティブ化します。
- **ロール** -ユーザーに割り当てられた役割。
- **アクション** - ロールまたはユーザーで実行するアクション。
- **時間** 動作が発生したときにします。
- **理由** -任意のテキストがアクティブ化時に、[理由] フィールドに入力された場合はここに表示されます。
- **有効期限** - 場合は、ユーザーに、ロールが完全に有効期限の年が 9999 です。

## 監査ログのフィルター

クリックして、監査ログに表示される情報を抽出することも、 **フィルター** ] ボタンをクリックします。   **更新グラフ パラメーター ブレード** が表示されます。

### 日付範囲を変更する
いずれかを選択すると、監査ログの時間範囲を変更、 **今日**, 、**過去 1 週間**, 、**過去 1 か月**, 、または **カスタム** ボタン。
選択すると、 **カスタム** ] ボタンが表示されます、 **から** 日付フィールドと **に** ログの日付の範囲を指定する日付フィールドが必要です。  /MM/DD/YYYY の形式で日付を入力するか、をクリックして、 **カレンダー** アイコン カレンダーから日付を選択します。

### ログに含まれるロールを変更する

オンまたはオフに、 **ロール** する各ロールの横にあるチェック ボックスを含めるか、ログから除外します。

監査ログのすべてのフィルターを設定したら、[更新] をクリックしてログ内のデータをフィルターします。  クリックして、データがすぐに表示されない場合、 **更新** ] ボタンをクリックします。

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
##次のステップ
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

