<properties
    pageTitle="Batch サービスのクォータと制限 | Microsoft Azure"
    description="Azure Batch サービスを使用する際のクォータ、制限、制約について説明します"
    services="batch"
    documentationCenter=""
    authors="dlepow"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2015"
    ms.author="danlep"/>



# Azure Batch サービスのクォータと制限

この記事では、Azure Batch サービスで使用できるリソースの既定値と上限値について説明します。 このような制限のほとんどは、Azure からユーザーのサブスクリプションまたは Batch アカウントに適用されているクォータです。

実稼働 Batch ワークロードを実行する予定がある場合、状況に応じて既定値を超える 1 つまたは複数のクォータを増やす必要があります。 クォータを増やすには、オンライン カスタマー サポートに申請 (無料) してください。

>[AZURE.NOTE] クォータとは、与信限度を容量が保証されません。 大規模な容量が必要な場合は、Azure サポートにお問い合わせください。

## サブスクリプションのクォータ
リソース|既定の制限|上限
---|---|---
サブスクリプションあたりのリージョンごとの Batch アカウント数|1|50

## Batch アカウントのクォータ
[AZURE.INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

## その他の制限
リソース|上限
---|---
コンピューティング ノードごとのタスク|ノードのコア数 x 4

## Batch クォータの確認

Batch アカウントのクォータを表示、 [Azure ポータル](https://portal.azure.com)します。

1. ポータルで、次のようにクリックします。 **バッチ アカウント** と Batch アカウントの名前。

2. [アカウント] ブレードをクリックして **設定** > **プロパティ**します。

    ![Batch アカウントのクォータ][account_quotas]

3.  **プロパティ** ブレードで、現在、Batch アカウントに適用されるクォータを確認します。

## クォータを増やす

使用して、Azure ポータルで、クォータを要求する次の手順を増やす (の増加を要求することも、 [Azure クラシック ポータル](http://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/))。

1. プレビュー ポータルのダッシュ ボードのクリックして **ヘルプとサポート**します。

2. をクリックして **作成サポート要求 > 基本**します。

3.  **基本** ブレードで、次の操作します。

    a.  **懸案事項の種類**, [ **クォータ**します。

    b. サブスクリプションを選択します。

    c.  **サービス**, [ **Batch サービス**します。

    d.  **サポート プラン**, [ **Azure サポート プラン - 開発者**します。

    クリックして **次**します。

4.  **問題** ブレードで、次の操作します。

    a.  **問題の種類**, [ **バッチ**します。

    b.  **詳細**, クォータの一覧を表示または特定のアカウントおよび新しい制限値に変更するクォータが必要です。

    クリックして **次**します。

5.  **連絡先情報** ブレードで、連絡先の詳細を入力し、クリックして **次**します。

6. クリックして **作成** 新しいサポート要求を送信します。

Azure サポートから連絡があります。 要求を完了するには最大で 2 営業日かかります。

## 関連トピック

* [Azure Batch アカウントの作成と管理](batch-account-create-portal.md)

* [Azure Batch 機能の概要](batch-api-basics.md)

* [Azure サブスクリプションとサービスの制限、クォータ、制約](../azure-subscription-service-limits.md)

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.PNG

