<properties 
   pageTitle="StorSimple Snapshot Manager 管理 | Microsoft Azure"
   description="StorSimple Snapshot Manager ソリューションの管理タスクとワークフローの概要とこれらに関する詳細情報へのリンクを示します。"
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carolz"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/28/2015"
   ms.author="v-sharos" />


# StorSimple Snapshot Manager を使用した StorSimple ソリューションの管理

## 概要

StorSimple Snapshot Manager は、Microsoft Azure StorSimple 環境でのデータ保護とバックアップ管理を簡素化する、Microsoft 管理コンソール (MMC) スナップインです。 StorSimple Snapshot Manager では、データ センター内やクラウド内の Microsoft Azure StorSimple データを 1 つの統合ストレージ ソリューションとして管理できるため、バックアップ プロセスが簡素化され、コストが削減されます。

StorSimple Snapshot Manager の中央管理コンソールを使用すると、ローカルおよびクラウドのデータについて、一貫性のある、特定時点のバックアップ コピーを作成することができます。 たとえば、このコンソールを使用して次の操作を実行できます。

- ボリュームを構成、バックアップ、削除する。

- ボリューム グループを構成してバックアップ データのアプリケーション整合性を確保する。

- 事前に指定されたスケジュールでデータがバックアップされるようにバックアップ ポリシーを管理する。

- クラウドに保存して障害復旧に使用できる、データの独立したコピーを作成する。

この記事では、StorSimple Snapshot Manager の概要と、これを使用してシステム管理タスクとワークフローの完了する方法を説明するためのチュートリアルへのリンクを示します。

- 詳細については、StorSimple Snapshot Manager のコンポーネントとアーキテクチャ、を参照してください [StorSimple Snapshot Manager は何ですか?](storsimple-what-is-snapshot-manager.md)。

- StorSimple Snapshot Manager をダウンロードするには [StorSimple Snapshot Manager のダウンロード ページ](https://www.microsoft.com/download/details.aspx?id=44220)します。

- StorSimple Snapshot Manager の展開の手順を参照してください [StorSimple Snapshot Manager の展開](storsimple-snapshot-manager-deployment.md)します。

## StorSimple Snapshot Manager のタスクとワークフロー

StorSimple Snapshot Manager を使用すると、現在のバックアップ ジョブ、スケジュールされているバックアップ ジョブ、および完了したバックアップ ジョブを監視し、管理できます。 さらに、StorSimple Snapshot Manager では、最大 64 個の完了したバックアップのカタログが提供されます。 このカタログを使用して、ボリュームまたは個々のファイルを検索し、復元できます。

| 目的の操作| 使用するチュートリアル|
|:---------------------------|:----------------------|
| StorSimple Snapshot Manager について知りたい。| [StorSimple Snapshot Manager とは(storsimple のようなは-スナップショット-manager.md)|
| StorSimple Snapshot Manager のインストール<br>StorSimple Snapshot Manager を再インストール<br>StorSimple Snapshot Manager の削除| [StorSimple Snapshot Manager をデプロイします。](storsimple-snapshot-manager-deployment.md)|
| 使用する StorSimple Snapshot Manager メニューと機能:<ul><li>メニュー バー</li><li>ツール バー</li><li>スコープ ペイン</li><li>結果ペイン</li><li>操作ウィンドウ</li><li>キーボードのナビゲーションおよびショートカット</li></ul>| [StorSimple Snapshot Manager ユーザー インターフェイス](storsimple-use-snapshot-manager.md)|
| StorSimple Snapshot Manager に含まれている一般的な MMC 機能の使用:<ul><li>ビュー</li><li>ここから新しいウィンドウ</li><li>更新</li><li>リストのエクスポート</li><li>ヘルプ</li></ul>| [StorSimple Snapshot Manager での MMC メニュー操作を使用します。](storsimple-snapshot-manager-mmc-menu.md)
| デバイスの交換を追加または<br>デバイス接続<br>インポートされたボリューム グループの確認<br>デバイスが接続されている更新<br>デバイスの認証<br>デバイスの詳細を表示<br>デバイス構成の削除<br>デバイス パスワードの変更<br>障害が発生したデバイスの交換<br>| [StorSimple Snapshot Manager を使用して接続し、StorSimple デバイスの管理](storsimple-snapshot-manager-manage-devices.md)|
| ボリュームのマウント<br>ボリュームに関する情報を表示<br>ボリュームを削除する<br>ボリュームの再スキャン<br>ベーシック ボリュームのバックアップを作成と構成<br>とダイナミック ミラー ボリュームのバックアップの構成| [StorSimple Snapshot Manager を使用して表示してボリュームの管理](storsimple-snapshot-manager-manage-volumes.md)|
| ボリューム グループの表示<br>ボリューム グループの作成<br>ボリューム グループのバックアップ<br>ボリューム グループの編集<br>ボリューム グループの削除| [StorSimple Snapshot Manager を使用して作成してボリューム グループを管理](storsimple-snapshot-manager-manage-volume-groups.md)|
| バックアップ ポリシーの作成 <br>バックアップ ポリシーの編集<br>バックアップ ポリシーの削除| [StorSimple Snapshot Manager を使用して作成、バックアップ ポリシーの管理](storsimple-snapshot-manager-manage-backup-policies.md)|
| 表示およびスケジュールされたバックアップ ジョブを管理<br>表示し、最新のバックアップ ジョブを管理<br>ビューおよび現在バックアップ ジョブの実行を管理| [StorSimple Snapshot Manager を使用して、バックアップ ジョブを表示および管理](storsimple-snapshot-manager-manage-backup-jobs.md)|
| ボリュームの復元<br>ボリュームまたはボリューム グループの複製<br>バックアップの削除<br>ファイルの復元<br>StorSimple Snapshot Manager データベースを復元します。| [StorSimple Snapshot Manager を使用したバックアップ カタログを管理するには](storsimple-snapshot-manager-manage-backup-catalog.md)|

## 次のステップ

[StorSimple Snapshot Manager のダウンロード](https://www.microsoft.com/download/details.aspx?id=44220)します。




