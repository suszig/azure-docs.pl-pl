<properties
   pageTitle="Azure Privileged Identity Management: セキュリティ レビューを開始する方法"
   description="Azure Privileged Identity Management 拡張機能で特権 ID のセキュリティ プレビューを作成する方法について説明します。"
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

# Azure Privileged Identity Management: セキュリティ レビューを開始する方法

## セキュリティ レビューの開始
最終的には、Azure ポータルの他の場所でセキュリティ レビューを実行できるようになります。  このドキュメント内のセキュリティ レビューを開始するための手順を説明する、 **Privileged Identity 情報管理 (PIM)** インターフェイスです。

管理者が認識していないユーザーがいる可能性、またはユーザーがジョブやプロジェクトを変更して新しい仕事では特権アクセスが不要になっている可能性があります。  このような「古い」ロールの割り当てに関連するリスクを軽減するためには、管理者はセキュリティ レビューを開始することによってユーザーに与えられているロールを確認できます。

## セキュリティ レビューを開始するパス
> [AZURE.NOTE] まだ作成していない PIM のダッシュ ボード、Azure ポータルで場合、」の手順を参照してください  [Azure Privileged Identity Management の概要。](active-directory-privileged-identity-management-getting-started.md)

Azure PIM ダッシュボードからは次の方法でレビューを開始できます。

- ダッシュボード > [セキュリティ レビュー] > [レビュー] > [レビュー] ボタン
- ダッシュボード > [ロール] > [レビュー] ボタン
- ダッシュボード > ロール一覧でレビューするロールをクリック > [レビュー] ボタン

## セキュリティ レビューを開始する

クリックすると、 **確認** ] ボタンを **役割のレビューを開始する** と **を確認するロールを選択** ブレードが表示されます、 **を確認するロールを選択** 項目が選択されます。

### レビューするロールを選択する

1. [役割の一覧から選択のロール、 **を確認するロールを選択** ブレードです。  一度に 1 つのロールのみ選択できます。   **を確認するロールを選択** ブレードに置き換え、 **ブレードで [レビュー担当者**します。  レビュー担当者を選択するときは 2 つのオプションがあります。
  - 自分で - 他の管理者の関与なしでセキュリティ レビューの動作を確認する場合、この機能を使用します。
  - ロールのメンバーによる自己レビュー - ユーザーに自分のロール割り当てを自分で確認させる場合、この機能を使用します。
2. どちらかを選択して、レビューの詳細に関する作業を始めます。  **既定値を変更** ブレードが表示されます。

### 自己レビュー

1. レビューの名前を入力して、レビューの名前、 **名前** フィールドです。  レビューには内容がわかる追跡しやすい一意の名前を付けることをお勧めします。
2. [確認のための開始日を入力、 **開始日** フィールドです。
3. レビューの終了日を入力して、 **終了日** フィールドです。  レビューの終了日を設定するときは次の点を考慮する必要があります。
  - レビュー対象のユーザー数。
  - ユーザーが拡張機能を追加してレビューを実行できるようになるまでの時間。
4. [Ok] ボタンをクリックして、 **既定値を変更** ブレードです。 ブレードが閉じます。
5. [Ok] ボタンをクリックして、 **ロールのレビューの開始** ブレードです。  ブレードが閉じます。 クリックして、ダッシュ ボードの更新通知は、ポータルのメイン メニューに表示されます、 **更新** に表示されます] ボタンをクリックし、セキュリティの確認、 **セキュリティ レビュー** セクションです。
6. ロールの各ユーザーに、拡張機能を追加して自分の管理アクセスをレビューする必要があることを通知します。  次のステップを参照してください。
6. [管理アクセスをレビューする](active-directory-privileged-identity-management-how-to-perform-security-review.md)

### 自分でレビューする

レビュー担当者として [自分] オプションを選択した場合は、セキュリティ レビューに進みます。 確認を完了の詳細については、次を参照してください [Azure Privileged Identity Management: セキュリティ確認を実行する方法。](active-directory-privileged-identity-management-how-to-perform-security-review.md)

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## PIM の内容一覧
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


