<properties
 pageTitle="Azure Scheduler とは | Microsoft Azure"
 description="Azure Scheduler では、クラウドで実行するアクションを宣言的に記述することができます。その後スケジュールを設定し、アクションを自動的に実行します。"
 services="scheduler"
 documentationCenter=".NET"
 authors="krisragh"
 manager="dwrede"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="get-started-article"
 ms.date="12/04/2015"
 ms.author="krisragh"/>


# Azure Scheduler とは

Azure Scheduler では、クラウドで実行するアクションを宣言的に記述することができます。 その後スケジュールを設定し、アクションを自動的に実行します。 スケジューラはこの処理を使用して [Azure ポータル](scheduler-get-started-portal.md), 、コード、 [REST API](https://msdn.microsoft.com/library/dn528946), 、または Azure PowerShell。

Scheduler には、スケジュールされた作業の作成、保守、呼び出しの機能があります。 Scheduler が、ワークロードをホストしたり、コードを実行したりすることはありません。 Scheduler は他の場所 (Azure、オンプレミス、または別のプロバイダー) でホストされるコードを_呼び出す_だけです。 HTTP、HTTPS、ストレージ キューを使用して呼び出します。

スケジューラ スケジュール [ジョブ](scheduler-concepts-terms.md), 、いずれかの確認でき、確定的にかつ確実に実行するワークロードをスケジュールすることは、ジョブの実行結果の履歴を保持します。 Azure WebJobs (Azure App Service の Web Apps 機能の一部) およびその他の Azure スケジュール機能では、Scheduler をバック グラウンドで使用します。  [Scheduler REST API](https://msdn.microsoft.com/library/dn528946) こうしたアクションにおける通信の管理を支援します。 そのため、スケジューラがサポートしている [複雑なスケジュールと高度な繰り返し](scheduler-advanced-complexity.md) 簡単にします。

Scheduler の使用に適しているシナリオがいくつかあります。 次に例を示します。

+ _Recurring アプリケーション アクション: _ 定期的に Twitter からフィードへのデータを収集します。
+ _Daily メンテナンス: ログのバックアップ、およびその他のメンテナンス タスクの実行の _ 日々 排除します。 たとえば、管理者は、今後 9 か月間、毎日午前 1 時にデータベースをバックアップすることができます。

Scheduler では、作成、更新、削除、表示、およびジョブを管理することができ、 [、ジョブ コレクション](scheduler-concepts-terms.md) スクリプトを使用してプログラムを使用して、およびポータルで。

## 関連項目

 [Azure Scheduler の概念、用語、およびエンティティ階層](scheduler-concepts-terms.md)

 [Azure ポータルでのスケジューラの使用の概要します。](scheduler-get-started-portal.md)

 [Azure Scheduler のプランと課金](scheduler-plans-billing.md)

 [複雑なスケジュールと Azure Scheduler で高度な繰り返しを構築する方法](scheduler-advanced-complexity.md)

 [Azure Scheduler REST API リファレンス](https://msdn.microsoft.com/library/dn528946)

 [Azure のスケジューラの PowerShell コマンドレット リファレンス](scheduler-powershell-reference.md)

 [Azure Scheduler の高可用性と信頼性](scheduler-high-availability-reliability.md)

 [Azure Scheduler の制限、規定値、およびエラー コード](scheduler-limits-defaults-errors.md)

 [Azure のスケジューラ送信認証](scheduler-outbound-authentication.md)





