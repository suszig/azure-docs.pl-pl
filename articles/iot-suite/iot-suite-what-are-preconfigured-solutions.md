<properties
 pageTitle="Azure IoT の構成済みソリューション | Microsoft Azure"
 description="Azure IoT の構成済みソリューションとそのアーキテクチャ (追加リソースのリンクを含む) の説明。"
 services=""
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="na"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="11/30/2015"
 ms.author="dobett"/>

# Azure IoT Suite の構成済みソリューションとは

Azure IoT Suite の構成済みソリューションとは、Azure サブスクリプションを使用して Microsoft Azure にデプロイできる一般的な IoT ソリューション パターンの実装です。 構成済みソリューションは次の用途で使用できます。

- 独自の IoT ソリューションの開始点として。
- IoT ソリューションの設計と開発で一般的なパターンについて学習するため。

構成済みの各ソリューションには、一般的な IoT シナリオが実装されており、シミュレートされたデバイスを使用してテレメトリを生成することができる、完全なエンドツーエンドの実装です。

Azure に構成済みソリューションをデプロイして実行するだけでなく、完全なソース コードをダウンロードして、特定の IoT 要件を満たすようにソリューションをカスタマイズして拡張することができます。

利用可能な構成済みソリューション:

- [リモート監視][lnk-remote-monitoring]
- [予測的なメンテナンス][lnk-predictive-maintenance]

次の表は、これらの構成済みソリューションが特定の IoT 機能にどのようにマップされるかを示しています。

| 解決策 | データの取り込み | デバイス ID | コマンドと制御 | ルールとアクション | 予測分析 |
|------------------------|-----|-----|-----|-----|-----|
| リモートの監視      | あり | はい | はい | あり | -   |
| 予測のメンテナンス | あり | はい | はい | はい | あり |

## リモート監視の構成済みソリューションの概要

ここでは、リモート監視の構成済みソリューションの主な要素について説明します。 リモート監視は、最も単純な構成済みソリューションです。他の構成済みソリューションが共有する一般的な設計要素が使用されています。

次の図は、リモート監視ソリューションの主な要素を示しています。 以降のセクションでは、これらの要素について詳しく説明します。

![リモート監視の構成済みソリューションのアーキテクチャ][img-remote-monitoring-arch]

## デバイス

リモート監視の構成済みソリューションをデプロイすると、実際の冷却デバイスをシミュレートするソフトウェア デバイス シミュレーターのインスタンスがデプロイメントに含まれます。 シミュレートされたデバイスから IoT Hub エンドポイントに対して、温度と湿度のテレメトリが送信されます。 また、シミュレートされたデバイスは、ソリューション ポータルから IoT Hub 経由で送信された次のコマンドに応答します。

- デバイスの ping
- テレメトリの開始
- テレメトリの停止
- 設定点温度の変更
- 診断テレメトリ
- デバイス状態の変更

## IoT Hub

IoT Hub は、1 つのエンドポイントの冷却デバイスからテレメトリを受信します。 IoT hub は、各デバイスを取得できます、コマンドなど、デバイスの特定のエンドポイントも保持、 **Ping デバイス** に送信されるコマンドです。

IoT Hub は、受信したテレメトリを、コンシューマー グループ エンドポイント経由で利用できるようにします。

IoT Hub インスタンスは、この構成済みのソリューションで、 *クラウド ゲートウェイ* 一般的な [IoT ソリューションのアーキテクチャ][lnk-what-is-azure-iot]します。

## Azure Stream Analytics

構成済みのソリューションは、3 つ [Azure Stream Analytics][lnk-asa] 涼しいデバイスから製品利用統計情報のストリームをフィルター処理する (ASA) ジョブ。

- ジョブ 1 から Azure BLOB ストレージに対して、コールド ストレージのすべてのテレメトリが送信されます。
- ジョブ 2 は、テレメトリ ストリームをフィルターして、デバイスからのコマンド応答メッセージとデバイス状態の更新メッセージを特定し、そのメッセージを Azure Event Hub エンドポイントに送信します。
- ジョブ 3 は、テレメトリ ストリームをフィルターして、アラームをトリガーする値を絞り込みます。 ある値によってアラームがトリガーされると、ソリューション ポータルのダッシュボード ビューに表示されるアラーム履歴テーブルに、通知が表示されます。

この構成済みのソリューションでは、ASA ジョブの一部を形成。、 *IoT ソリューション バックエンド* 一般的な [IoT ソリューションのアーキテクチャ][lnk-what-is-azure-iot]します。

## イベント プロセッサ

 [EventPocessorHost][lnk-event-processor] で実行されるインスタンス、 [web ジョブ][lnk-web-job], 、ASA ジョブ 2 で識別されるコマンド応答し、デバイス ステータス メッセージを処理およびでは、この情報を格納、 [Azure DocumentDB][lnk-document-db] データベースです。

この構成済みのソリューションで、イベント プロセッサの一部を形成、 *IoT ソリューション バックエンド* 一般的な [IoT ソリューションのアーキテクチャ][lnk-what-is-azure-iot]します。

## ソリューション ポータル

ソリューション ポータルは、構成済みソリューションの一部としてクラウドにデプロイされている Web ベースの UI です。 ソリューション ポータルでは、次の操作を実行できます。

- ダッシュボードにテレメトリとアラームの履歴を表示します。
- 新しいデバイスをプロビジョニングします。
- デバイスを管理し、監視します。
- 特定のデバイスにコマンドを送信します。
- ルールとアクションを管理します。

> [AZURE.NOTE] また、ソリューション portal は IoT Hub を維持 [デバイスの id のレジストリ][lnk-identity-registry] ソリューションの DocumentDB データベース内の豊富なデバイスの状態情報のストアと同期します。

この構成済みのソリューションで、ソリューションのポータルの一部を形成、 *IoT ソリューション バックエンド* の一部と、 *処理とビジネスの接続* 一般的な [IoT ソリューションのアーキテクチャ][lnk-what-is-azure-iot]します。

## 次のステップ

IoT の構成済みソリューションの詳細については、次のリソースをご覧ください。

- [Azure IoT の構成済みソリューションの概要][lnk-suite-overview]
- [IoT 事前構成済みソリューションの使用][lnk-preconf-get-started]

[img-remote-monitoring-arch]: ./media/iot-suite-what-are-preconfigured-solutions/remote-monitoring-arch1.png
[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-processor]: event-hubs-programming-guide.md#event-processor-host
[lnk-web-job]: web-sites-create-web-jobs.md
[lnk-document-db]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-identity-registry]: iot-hub-devguide.md#device-identity-registry
[lnk-suite-overview]: iot-suite-overview.md
[lnk-preconf-get-started]: iot-suite-getstarted-preconfigured-solutions.md
[lnk-predictive-maintenance]: iot-suite-predictive-overview.md
