<properties 
   pageTitle="StorSimple Manager サービス管理 | Microsoft Azure"
   description="Azure クラシック ポータルで StorSimple Manager サービスを使用して、StorSimple デバイスを管理する方法を説明します。"
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carolz"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/02/2015"
   ms.author="alkohli" />


# StorSimple Manager サービスを使用した StorSimple デバイスの管理

## 概要

この記事では、接続方法、用意されている各種オプション、この UI を通じて実行できる特定のワークフローへのリンクを含め、StorSimple Manager サービス インターフェイスについて説明します。 このガイドは、StorSimple 物理デバイスと仮想デバイスの両方に適用されます。

この記事を読むと、次のことを習得できます。

- StorSimple Manager サービスへの接続
- StorSimple Manager UI のナビゲーション
- StorSimple Manager サービスを使用した StorSimple デバイスの管理


## StorSimple Manager サービスへの接続

StorSimple Manager サービスは Microsoft Azure で実行され、複数の StorSimple デバイスに接続します。 これらのデバイスを管理するには、中央の Microsoft Azure クラシック ポータルをブラウザーで実行して使用します。 StorSimple Manager サービスに接続するには、次の手順を実行します。

#### サービスに接続するには

1. に移動 [http://azure.microsoft.com/](http://azure.microsoft.com/)

1. Microsoft アカウントの資格情報を使用して、ウィンドウの右上にある Microsoft Azure クラシック ポータルにログオンします。

1. 左側のナビゲーション ウィンドウで下へスクロールして、StorSimple Manager サービスにアクセスします。


## StorSimple Manager サービス UI のナビゲーション

StorSimple Manager サービス UI のナビゲーション階層を次の表に示します。

- **StorSimple Manager** のランディング ページからは、サービス内のすべてのデバイスに適用可能な UI サービス レベルのページに移動できます。

- **[デバイス]** ページからは、特定のデバイスに適用可能なデバイス レベルの UI ページに移動できます。

- **[ボリューム コンテナー]** ページからは、デバイスに関連付けられているすべてのボリュームを表示するボリューム ページに移動できます。


#### StorSimple Manager サービスのナビゲーション階層

| ランディング ページ| サービス レベルのページ| デバイス レベルのページ| デバイス レベルのページ|
|---|---|---|---|
| StorSimple Manager サービス| サービスのダッシュボード| デバイス ダッシュボード| |
| | デバイス] → [| 監視|
| | バックアップ カタログ| ボリューム containers→| ボリューム|
| | 構成 (サービス)| バックアップ ポリシー| |
| | ジョブ| (デバイス) の構成します。|
| | Alerts| メンテナンス|

![ビデオ](./media/storsimple-manager-service-administration/Video_icon.png) **ビデオ**

StorSimple Manager サービスのユーザー インターフェイスの手順を説明するビデオを視聴するにはクリックして [ここ](http://azure.microsoft.com/documentation/videos/storsimple-manager-service-overview/)します。

## StorSimple Manager サービスを使用した StorSimple デバイスの管理

次の表に、StorSimple Manager サービス UI 内で実行できるすべての一般的な管理タスクと複雑なワークフローの概要を示します。 これらのタスクは、タスクが開始される UI ページに基づいてまとめられています。

各ワークフローの詳細については、表内の適切な手順をクリックしてください。

#### StorSimple Manager のワークフロー

| 目的の操作| 移動先の UI ページ| 実行する手順|
|---|---|---|
| サービスを作成する</br>サービスの削除</br>サービス登録キーの取得</br>再生成するサービス登録キー| StorSimple Manager サービス| [StorSimple Manager サービスを展開します。](storsimple-manage-service.md)
| サービス データ暗号化キーを変更する</br>操作ログの表示| StorSimple Manager サービス → [ダッシュボード]| [StorSimple Manager サービス ダッシュ ボードの使用](storsimple-service-dashboard.md)|
| デバイスを非アクティブ化</br>デバイスの削除| StorSimple Manager サービス → [デバイス]| [非アクティブ化またはデバイスの削除](storsimple-deactivate-and-delete-device.md)|
| 災害復旧とデバイスのフェールオーバーについて</br>物理デバイスへのフェールオーバー</br>仮想デバイスへのフェールオーバー</br>ビジネス継続性障害復旧 (BCDR)| StorSimple Manager サービス → [デバイス]| [StorSimple デバイスのフェールオーバーと災害復旧](storsimple-device-failover-disaster-recovery.md)|
| バックアップ ボリュームを一覧表示</br>バックアップ セットの選択</br>バックアップ セットの削除| StorSimple Manager サービス → [バックアップ カタログ]| [バックアップを管理します。](storsimple-manage-backup-catalog.md)|
| ボリュームを複製する| StorSimple Manager サービス → [バックアップ カタログ]| [ボリュームを複製します。](storsimple-clone-volume.md)|
| バックアップ セットを復元する| StorSimple Manager サービス → [バックアップ カタログ]| [バックアップ セットを復元します。](storsimple-restore-from-backup-set.md)|
| ストレージ アカウントについて</br>ストレージ アカウントの追加</br>ストレージ アカウントの編集</br>ストレージ アカウントを削除</br>ストレージ アカウントのキー ローテーション| StorSimple Manager サービス → [構成]| [ストレージ アカウントを管理します。](storsimple-manage-storage-accounts.md)|
| 帯域幅テンプレートに関する</br>帯域幅テンプレートを追加する</br>帯域幅テンプレートを編集する</br>帯域幅テンプレートを削除する</br>既定の帯域幅テンプレートを使用して</br>指定時刻に開始する終日帯域幅テンプレートの作成| StorSimple Manager サービス → [構成]| [帯域幅テンプレートを管理します。](storsimple-manage-bandwidth-templates.md)|
| アクセス制御レコードについて</br>アクセス制御レコードを作成する</br>アクセス制御レコードの編集</br>アクセス制御レコードの削除| StorSimple Manager サービス → [構成]| [アクセス制御レコードを管理します。](storsimple-manage-acrs.md)|
| ジョブの詳細を表示</br>ジョブを取り消す| StorSimple Manager サービス → [ジョブ]| [ジョブを管理します。](storsimple-manage-jobs.md)
| 警告通知を受け取る</br>アラートを管理する</br>アラートを確認します。| StorSimple Manager サービス → [アラート]| [表示および StorSimple のアラートの管理](storsimple-manage-alerts.md)
| 接続されたイニシエーターの表示</br>デバイスのシリアル番号を検索</br>ターゲット IQN の検索| StorSimple Manager サービス → [デバイス] → [ダッシュボード]| [StorSimple デバイスのダッシュ ボードの使用](storsimple-device-dashboard.md)|
| 監視グラフを作成する| StorSimple Manager サービス → [デバイス] → [監視]| [StorSimple デバイスを監視します。](storsimple-monitor-device.md)|
| ボリューム コンテナーを追加</br>ボリューム コンテナーを変更する</br>ボリューム コンテナーを削除| StorSimple Manager サービス → [デバイス] → [ボリューム コンテナー]| [ボリューム コンテナーを管理します。](storsimple-manage-volume-containers.md)|
| ボリュームの追加</br>ボリュームを変更する</br>ボリュームをオフライン</br>ボリュームを削除する</br>ボリュームを監視します。| StorSimple Manager サービス → [デバイス] → [ボリューム コンテナー] → [ボリューム]| [ボリュームを管理します。](storsimple-manage-volumes.md)|
| デバイス設定の変更</br>時刻の設定を変更</br>変更 DNS.md 設定</br>ネットワーク インターフェイスを構成します。| StorSimple Manager サービス → [デバイス] → [構成]| [StorSimple デバイスのデバイス構成を変更します。](storsimple-modify-device-config.md)|
| Web プロキシ設定を表示する| StorSimple Manager サービス → [デバイス] → [構成]| [デバイスの web プロキシを構成します。](storsimple-configure-web-proxy.md)|
| デバイス管理者のパスワードの変更</br>変更 StorSimple Snapshot Manager のパスワード| StorSimple Manager サービス → [デバイス] → [構成]| [StorSimple のパスワードを変更します。](storsimple-change-passwords.md)|
| リモート管理の構成| StorSimple Manager サービス → [デバイス] → [構成]| [StorSimple デバイスにリモート接続します。](storsimple-remote-connect.md)|
| アラート設定の構成| StorSimple Manager サービス → [デバイス] → [構成]| [表示および StorSimple のアラートの管理](storsimple-manage-alerts.md)|
| StorSimple デバイスの CHAP の構成| StorSimple Manager サービス → [デバイス] → [構成]| [StorSimple デバイスの CHAP を構成します。](storsimple-configure-chap.md)|
| バックアップ ポリシーの追加</br>追加のスケジュールを変更または</br>バックアップ ポリシーの削除</br>手動バックアップをとる</br>複数のボリュームおよびスケジュールに関するカスタム バックアップ ポリシーの作成| StorSimple Manager サービス → [デバイス] → [バックアップ ポリシー]| [バックアップ ポリシーを管理します。](storsimple-manage-backup-policies.md)|
| デバイス コント ローラーを停止</br>デバイス コント ローラーを再起動</br>デバイス コント ローラーをシャット ダウン</br>デバイスを出荷時設定にリセットする</br>(上記は、オンプレミス デバイスのみ)| StorSimple Manager サービス → [デバイス] → [メンテナンス]| [StorSimple デバイス コント ローラーを管理します。](storsimple-manage-device-controller.md)|
| StorSimple のハードウェア コンポーネントについて</br>ハードウェアの状態を監視</br>(上記は、オンプレミス デバイスのみ)| StorSimple Manager サービス → [デバイス] → [メンテナンス]| [ハードウェア コンポーネントの監視](storsimple-monitor-hardware-status.md)|
| サポート パッケージを作成する| StorSimple Manager サービス → [デバイス] → [メンテナンス]| [作成し、サポート パッケージの管理](storsimple-create-manage-support-package.md)|
| Install software updates| StorSimple Manager サービス → [デバイス] → [メンテナンス]| [デバイスを更新します。](storsimple-update-device.md)|


## 次のステップ

StorSimple デバイスの日常的な操作または StorSimple デバイスのハードウェア コンポーネントのいずれかで問題が発生した場合は、次のトピックを参照してください。

- [運用デバイスをトラブルシューティングします。](storsimple-troubleshoot-operational-device.md)
- [StorSimple 監視インジケーター Led を使用します。](storsimple-monitoring-indicators.md)

問題を解決できず、サービス要求の作成が必要な場合は、次のトピックを参照してください。

-  [Microsoft サポートにお問い合わせ](storsimple-contact-microsoft-support.md)





