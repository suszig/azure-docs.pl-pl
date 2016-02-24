<properties
   pageTitle="SQL Database 脅威の検出の概要"
   description="Azure ポータルでの SQL Database 脅威の検出の使用方法"
   services="sql-database"
   documentationCenter=""
   authors="ronitr"
   manager="jeffreyg"
   editor="v-romcal"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/29/2015"
   ms.author="ronmat; ronitr"/>


# SQL Database 脅威の検出の概要

## 概要

脅威の検出は、データベースに対する潜在的なセキュリティ脅威を示す異常なデータベース アクティビティを検出します。  脅威の検出はプレビュー状態であり、Azure SQL Database の V12 バージョンでサポートされます。

脅威の検出で提供される新しいセキュリティ階層は、異常なアクティビティに対するセキュリティ アラートを提供することによって、発生する可能性のある脅威をユーザーが検出して対応できるようにします。  ユーザーを使用して疑わしいイベントを調べることができます [Azure SQL Database の監査](sql-database-auditing-get-started.md) にアクセスしたりを侵害したり、データベース内のデータを利用しようとした結果を決定します。
脅威の検出を使用するとデータベースに対する潜在的な脅威に簡単に対処でき、セキュリティの専門家である必要や、高度なセキュリティ監視システムを管理する必要はありません。

たとえば、脅威の検出は、SQL インジェクションの可能性を示す特定の異常なデータベース アクティビティを検出します。 SQL インジェクションはインターネットでの一般的な Web アプリケーションのセキュリティの問題の 1 つであり、データ駆動型アプリケーションの攻撃に使用されます。 攻撃者は、アプリケーションの脆弱性を利用してアプリケーションの入力フィールドに悪意のある SQL ステートメントを挿入し、データベースのデータを侵害または変更します。


## データベースに対する脅威の検出の設定

1. Azure ポータルを起動 [https://portal.azure.com](https://portal.azure.com)します。

2. 監視する SQL Database の構成ブレードに移動します。 [設定] ブレードで、選択 **監査と脅威の検出**します。

    ![ナビゲーション ウィンドウ][1]

3.  **監査と脅威の検出** 構成ブレードに **ON** 監査、脅威の検出の設定が表示されるされます。

    ![ナビゲーション ウィンドウ][2]

4. 有効にする **ON** 脅威の検出。

5. 異常なデータベース アクティビティが検出されたときにセキュリティ アラートを受け取る電子メールのリストを構成します。

6. クリックして **保存** で、 **監査と脅威の検出** を追加または更新の監査を保存し、脅威の検出ポリシーの構成ブレードです。

    ![ナビゲーション ウィンドウ][3]


## 疑わしいイベントが検出されたときの異常なデータベース アクティビティの調査

1. 異常なデータベース アクティビティが検出されると、電子メールで通知を受け取ります。 <br/>
電子メールは異常な活動、データベース名、サーバー名およびイベントの時刻の性質を含む疑わしいセキュリティ イベントに関する情報を提供します。 さらに、考えられる原因に関する情報を提供し、推奨される対処法を調査し、データベースへの潜在的な脅威を軽減します。<br/>

    ![ナビゲーション ウィンドウ][4]

2. 電子メールで] をクリックして、 **Azure SQL の監査ログ** リンク、Azure ポータルを起動し、疑わしいイベントの時刻に関連する監査レコードを表示します。

    ![ナビゲーション ウィンドウ][5]

3. 監査レコードをクリックすると、SQL ステートメント、エラーの理由、クライアントの IP など、疑わしいデータベース アクティビティについての詳細が表示されます。

    ![ナビゲーション ウィンドウ][6]

4. レコードの監査] ブレードで [  **を Excel で開く** を開くには、事前に構成された excel テンプレートをインポートし、疑わしいイベントの前後に監査ログのより深い分析を実行します。<br/>
**注:** Excel 2010 またはそれ以降、電源のクエリと **高速結合** 設定が必要

    ![ナビゲーション ウィンドウ][7]

5. 構成する、 **高速結合** 設定 - で、 **POWER QUERY** リボン タブで、 **オプション** オプション] ダイアログを表示します。 [プライバシー] セクションを選択し、2 番目のオプション [プライバシー レベルを無視し、可能であればパフォーマンスを向上させる] をオンにします。

    ![ナビゲーション ウィンドウ][8]

6. SQL 監査ログをロードするには、[設定] タブのパラメーターが正しく設定されていることを確認してから、データのリボンを選択し、[すべて更新] ボタンをクリックします。

    ![ナビゲーション ウィンドウ][9]

7. 結果が表示されます、 **SQL 監査ログ** シートを検出された異常なアクティビティのより深い分析を実行し、アプリケーションのセキュリティ イベントの影響を軽減することができます。


<!--Image references-->
[1]: ./media/sql-database-threat-detection-get-started/1_td_click_on_settings.png
[2]: ./media/sql-database-threat-detection-get-started/2_td_turn_on_auditing.png
[3]: ./media/sql-database-threat-detection-get-started/3_td_turn_on_threat_detection.png
[4]: ./media/sql-database-threat-detection-get-started/4_td_email.png
[5]: ./media/sql-database-threat-detection-get-started/5_td_audit_records.png
[6]: ./media/sql-database-threat-detection-get-started/6_td_audit_record_details.png
[7]: ./media/sql-database-threat-detection-get-started/7_td_audit_records_open_excel.png
[8]: ./media/sql-database-threat-detection-get-started/8_td_excel_fast_combine.png
[9]: ./media/sql-database-threat-detection-get-started/9_td_excel_parameters.png

