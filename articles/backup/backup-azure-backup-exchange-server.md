<properties
    pageTitle="System Center 2012 R2 DPM を使用して Exchange サーバーを Azure Backup にバックアップする | Microsoft Azure"
    description="System Center 2012 R2 DPM を使用して Exchange サーバーを Azure Backup にバックアップする方法について説明する"
    services="backup"
    documentationCenter=""
    authors="Jim-Parker"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/08/2015"
    ms.author="jimpark;delhan"/>


# System Center 2012 R2 DPM を使用して Exchange サーバーを Azure Backup にバックアップする
この記事では、Azure Backup を Microsoft Exchange server をバックアップする System Center 2012 R2 Data Protection Manager (DPM) サーバーを構成する方法について説明します。  

## 更新プログラム
Azure Backup に DPM サーバーを正常に登録するには、System Center 2012 R2 DPM 用の最新の更新プログラムのロールアップと、Azure Backup エージェントの最新バージョンをインストールする必要があります。 最新の更新プログラムのロールアップを取得、 [Microsoft カタログ](http://catalog.update.microsoft.com/v7/site/Search.aspx?q=System%20Center%202012%20R2%20Data%20protection%20manager)します。

>[AZURE.NOTE] この記事の例については、Azure Backup エージェントのバージョン 2.0.8719.0 がインストールされているし、System Center 2012 R2 DPM の更新プログラム ロールアップ 6 がインストールされています。

## 前提条件
続行する前に、以下のことを確認すべて、 [の前提条件](backup-azure-dpm-introduction.md#prerequisites) Microsoft Azure Backup を使用して保護するワークロードが満たされています。 該当する前提条件を以下に示します。

- Azure サイトに対するバックアップ コンテナーが作成済みである。
- エージェントとコンテナーの資格情報が DPM サーバーにダウンロード済みである。
- DPM サーバーにエージェントがインストールされている。
- コンテナーの資格情報を使用して DPM サーバーを登録済みである。

## DPM 保護エージェント  
Exchange サーバーに DPM 保護エージェントをインストールするには、次の手順に従います。

1. ファイアウォールが正しく構成されていることを確認します。 参照してください [エージェントに対するファイアウォールの例外を構成する](https://technet.microsoft.com/library/Hh758204.aspx)です。

2. クリックして、Exchange サーバーにエージェントをインストール **管理 > エージェント > インストール** DPM 管理者コンソールでします。 参照してください [DPM 保護エージェントをインストール](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) の詳細な手順を実行します。

## Exchange サーバーの保護グループを作成する

1. DPM 管理者コンソールで [ **保護**, 、クリックして **新規** を開くには、ツール リボンで、 **新しい保護グループの作成** ウィザード。

2.  **へようこそ]** ウィザードのクリックの画面 **次**します。

3.  **保護グループの種類を選択して** 画面で選択 **サーバー** ] をクリック **次**します。

4. Exchange サーバー データベースを保護し、をクリックするを選択 **次**します。

    >[AZURE.NOTE] Exchange 2013 を保護する場合、 [Exchange 2013 の前提条件](https://technet.microsoft.com/library/dn751029.aspx)します。

    次の例では、Exchange 2010 データベースが選択されています。

    ![グループ メンバーの選択](./media/backup-azure-backup-exchange-server/select-group-members.png)

5. データ保護方法を選択します。

    保護グループに名前を付けて、次のオプションの両方を選択します。

    - ディスクを使用した短期的な保護を利用する
    - オンライン保護を利用する

6. クリックして **次**します。

7. 選択、 **データの整合性をチェックするには、[Eseutil の実行** を Exchange Server データベースの整合性をチェックする場合は、オプションです。

    実行によって生成された I/O トラフィックを避けるため、DPM サーバー上のバックアップの整合性チェックを実行はこのオプションを選択した後、 **eseutil** コマンドを Exchange サーバーにします。

    >[AZURE.NOTE] このオプションを使用するには、DPM サーバー上の C:\Program files \microsoft System Center 2012 R2\DPM\DPM\bin ディレクトリに、Ese.dll ファイルと Eseutil.exe ファイルをコピーする必要があります。 それ以外の場合、次のエラーがトリガーされます。  
    ![eseutil エラー](./media/backup-azure-backup-exchange-server/eseutil-error.png)

8. クリックして **次**します。

9. データベースを選択 **コピー バックアップ**, 、クリックして **次**します。

    >[AZURE.NOTE] データベースの少なくとも 1 つの DAG コピーの「完全バックアップ」を選択しないと、ログは切り捨てられません。

10. 目標を構成する **短期的なバックアップ**, 、クリックして **次**します。

11. 使用可能なディスク領域を確認し、クリックして **次**します。

12. 時刻を DPM サーバーは、初期レプリケーションの作成し、順にクリックして選択 **次**します。

13. 整合性チェックのオプションを選択し、クリックして **次**します。

14. クリックして、Azure にバックアップするデータベースを選択して **次**します。 次に例を示します。

    ![オンライン保護データの指定](./media/backup-azure-backup-exchange-server/specify-online-protection-data.png)

15. スケジュールを定義 **Azure Backup**, 、クリックして **次**します。 次に例を示します。

    ![オンライン バックアップ スケジュールの指定](./media/backup-azure-backup-exchange-server/specify-online-backup-schedule.png)

    >[AZURE.NOTE] オンライン回復ポイントは高速に基づく完全な回復ポイント。 そのため、オンライン回復ポイントは、高速完全回復ポイントに指定した時刻より後にスケジュールする必要があります。

16. 保有ポリシーを構成する **Azure Backup**, 、クリックして **次**します。

17. オンライン レプリケーション オプションを選択し、クリックして **次**します。

    データベースの規模が大きい場合は、ネットワーク経由で最初のバックアップを作成するのに時間がかかる可能性があります。 この問題を回避するには、オフライン バックアップを作成します。  

    ![オンライン保持ポリシーの指定](./media/backup-azure-backup-exchange-server/specify-online-retention-policy.png)

18. クリックして、設定を確認して **グループの作成**します。

19. クリックして **閉じる**します。

## Exchange データベースを回復する

1. Exchange データベースを回復するにはクリックして **回復** DPM 管理者コンソールでします。

2. 回復する Exchange データベースを特定します。

3. オンライン回復ポイントを選択して、 *復旧時間* ボックスの一覧です。

4. クリックして **回復** を開始する、 **回復ウィザード**します。

オンライン回復ポイントでは、5 種類の回復があります。

- **Exchange Server の元の場所に回復する:** 、データの元の Exchange サーバーに回復します。
- **Exchange サーバー上の別のデータベースに回復する:** データは、別の Exchange サーバー上の別のデータベースに回復します。
- **回復データベースに回復する:** データが Exchange の回復データベース (RDB) を復旧します。
- **ネットワーク フォルダーにコピーする:** データをネットワーク フォルダーに回復されます。
- **テープにコピーする:** 場合は、テープ ライブラリまたはアタッチを解除し、DPM サーバーで構成されているスタンドアロンのテープ ドライブがある場合は、回復ポイントは、空きテープにコピーされます。

    ![オンライン レプリケーションの選択](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## 次のステップ

- [Azure Backup FAQ](backup-azure-backup-faq.md)

