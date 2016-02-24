<properties
    pageTitle="Azure SQL Database では現在サーバーのデータベースのトラブルシューティングを使用できません"
    description="Azure SQL Database の接続エラーを特定して解決する手順。"
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="msmets"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/10/2015"
    ms.author="daleche"/>

# 「サーバー上のデータベースは現在使用できません。 再試行してください後で接続します。" その他の接続エラー
"データベース <dbname> サーバーで <servername> は現在利用できません..." Azure SQL Database の最も一般的な一時的な接続エラーです。 一時的な接続エラーは、通常新しいサーバーへのフェールオーバーや、一時的なシステム障害などのプラットフォームの再構成が原因であり、長くは続きません。 別のエラーが発生した場合は、評価、 [エラー メッセージ](sql-database-develop-error-messages.md) の原因の手掛かりは、問題が一時的か永続的でかどうかを判断し、このトピックのガイダンスを使用します。

## 一時的な接続の問題を解決する手順
1.  アプリで再試行ロジックを使用していることを確認します。 参照してください、 [接続の問題](sql-database-connectivity-issues.md) と [ベスト プラクティスと設計のガイドライン](sql-database-connect-central-recommendations.md) 一般的な再試行戦略。 次を参照してください [コード サンプルは](sql-database-develop-quick-start-client-code-samples.md) 固有です。
2.  データベースがリソースの制限に近づくと、一時的な接続の問題に見える場合があります。 参照してください [パフォーマンス問題のトラブルシューティング](sql-database-troubleshoot-performance.md)します。
3.  接続の問題が続く場合は、サポートに連絡してサポート ケースを登録してください。

## 永続的な接続の問題を解決する手順
アプリがまったく接続できない場合、通常は IP とファイアウォールの構成の問題です。 これには、クライアント側 (たとえば、新しい IP アドレスやプロキシ) のネットワークの再構成が含まれます。 接続パラメーター (接続文字列など) の入力が間違っている場合もあります。

1.  セットアップ [のファイアウォール ルールを](sql-database-configure-firewall-settings.md) クライアントの IP アドレスを許可するようにします。
2.  クライアントとインターネットの間のすべてのファイアウォールで、送信接続用のポート 1433 が開いていることを確認します。
3.  接続文字列およびその他の接続設定を確認します。 接続文字列セクションを参照して、 [接続の問題トピック](sql-database-connectivity-issues.md)します。
4.  ダッシュ ボードでサービスの正常性を確認します。 地域的障害があると思われる場合は、次を参照してください。 [障害からの回復](sql-database-disaster-recovery.md) 用の新しいリージョンに復旧する手順です。
5.  接続の問題が続く場合は、サポートに連絡してサポート ケースを登録してください。

