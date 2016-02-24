<properties
 pageTitle="リモート監視の事前構成済みソリューションのチュートリアル | Microsoft Azure"
 description="Azure IoT リモート監視の事前構成済みソリューションとそのアーキテクチャの説明です。"
 services=""
 documentationCenter=""
 authors="stevehob"
 manager="timlt"
 editor=""/>

<tags
 ms.service="na"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2015"
 ms.author="stevehob"/>

# リモート監視の事前構成済みソリューションのチュートリアル

## はじめに

IoT Suite リモート監視の事前構成済みソリューションは、遠隔地にある複数のコンピューターを操作するビジネス シナリオ向けの基本的なエンド ツー エンド監視ソリューションです。 このソリューションは、主要な Azure IoT Suite サービスを組み合わせてビジネス シナリオの汎用的な実装を実現します。また、独自のビジネス要件を満たすためにこのタイプの IoT ソリューションの実装を予定している顧客の開始点となります。

## 論理アーキテクチャ

次の図は、事前構成済みソリューションの論理コンポーネントの概要を示したものです。

![](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)


### シミュレートされたデバイス

事前構成済みソリューションにおいて、シミュレートされたデバイスは冷却装置 (建物のエアコンや施設の空気処理ユニットなど) を表します。 シミュレートされたデバイスはそれぞれ、次のテレメトリ メッセージを IoT Hub に送信します。


| メッセージ  | 説明 |
|----------|-------------|
| Startup  | デバイスが起動すると送信、 **デバイス情報** 、デバイスがサポートして、デバイスの現在の構成などのデバイス id、デバイス メタデータの一覧、それ自体に関する情報を含むメッセージがコマンドです。 |


シミュレートされたデバイスは、次のデバイス プロパティをメタデータとして送信します。

| プロパティ               |  目的 |
|------------------------|--------- |
| Device ID              | ソリューションでデバイスが作成されるときに指定されるか、割り当てられる ID。 |
| Manufacturer           | デバイスの製造元 |
| Model Number           | デバイスのモデル番号 |
| Serial Number          | デバイスのシリアル番号 |
| Firmware               | デバイスのファームウェアの現在のバージョン |
| Platform               | デバイスのプラットフォーム アーキテクチャ |
| Processor              | デバイスを実行しているプロセッサ |
| Installed RAM          | デバイスに搭載されている RAM の量 |
| Hub Enabled State      | デバイスの IoT Hub の状態プロパティ |
| Created Time           | ソリューションでデバイスが作成された時刻 |
| Updated Time           | デバイスのプロパティが最後に更新された時刻 |
| Latitude               | 緯度で表したデバイスの位置 |
| Longitude              | 経度で表したデバイスの位置 |

シミュレーターは、シミュレートされたデバイスのこれらのプロパティをサンプル値と共に送信します。  シミュレーターがシミュレートされたデバイスを初期化するたびに、デバイスは事前定義されたメタデータを IoT Hub に投稿します。 これにより、デバイス ポータルで行われたすべてのメタデータの更新が上書きされることに注意してください。


シミュレートされたデバイスは、IoT Hub から送信された次のコマンドを処理できます。

| コマンド                | 説明                                         |
|------------------------|-----------------------------------------------------|
| PingDevice             | 送信、 _ping_ 動作を確認するデバイス   |
| StartTelemetry         | デバイスのテレメトリ送信を開始します。                 |
| StopTelemetry          | デバイスのテレメトリ送信を停止します。             |
| ChangeSetPointTemp     | ランダムなデータが生成される設定ポイントの値を変更します。 |
| DiagnosticTelemetry    | デバイス シミュレーターをトリガーし、追加のテレメトリ値 (externalTemp) を送信します。 |
| ChangeDeviceState      | デバイスの拡張状態プロパティを変更し、デバイスからデバイスの情報メッセージを送信 |


デバイス コマンドの受信確認は IoT Hub を介して提供されます。


### Azure Stream Analytics ジョブ

**ジョブ 1: 製品利用統計情報** は 2 つのコマンドを使用して、受信デバイス テレメトリ ストリームで動作します。 最初のコマンドは、デバイスからのすべてのテレメトリ メッセージを永続的な BLOB ストレージに送信します。 2 番目のコマンドは、5 分間のスライディング ウィンドウで湿度の平均値、最小値、最大値を計算します。 このデータも BLOB ストレージに送信されます。

**ジョブ 2: デバイス情報** 受信メッセージ ストリームからのデバイス情報メッセージをフィルター処理し、Event Hub のエンドポイントに送信します。 デバイスに応答して開始時にデバイス情報メッセージを送信、 **SendDeviceInfo** コマンドです。

**ジョブ 3: ルール** デバイスごとのしきい値と受信の温度と湿度製品利用統計情報の値を評価します。 しきい値の値は、ソリューションに含まれる規則エディターで設定されます。 デバイス/値ペアがタイムスタンプとして、Stream Analytics に読み込まれる blob に格納されている **参照データ**します。 このジョブでは、空以外の値が、デバイスに設定された設定しきい値と比較されます。 超えた場合、' >' の条件、ジョブの出力は、 **アラーム** ことを示し、しきい値を超えましたデバイス、値、およびタイムスタンプ値を提供するイベントです。

### イベント プロセッサ

 **イベント プロセッサ** デバイス情報メッセージとコマンドの応答を処理します。 イベント プロセッサで使用されるものは次のとおりです。

- 現在のデバイス情報を使用してデバイス レジストリ (DocumentDB データベースに格納されています) を更新する場合はデバイス情報メッセージ。
- デバイス コマンドの履歴 (DocumentDB データベースに格納されています) を更新する場合はコマンド応答メッセージ。

## チュートリアルの開始

このセクションでは、ソリューションのコンポーネントと想定されるユース ケースについて説明し、例を示します。

### リモート監視ダッシュボード
Web アプリケーションでは、このページは、PowerBI javascript コントロールを使用して (を参照してください [PowerBI ビジュアル リポジトリ](https://www.github.com/Microsoft/PowerBI-visuals)) を blob ストレージ内の Stream Analytics ジョブから出力されたデータを視覚化します。


### デバイス管理ポータル

この Web アプリを使用すると、次のことができます。

- 一意のデバイス ID を設定して認証キーを生成する新しいデバイスをプロビジョニングする
- 既存のプロパティの表示や新しいプロパティによる更新など、デバイスのプロパティを管理する
- デバイスにコマンドを送信する
- デバイスのコマンド履歴を表示する

### クラウド ソリューションの動作を確認する
移動して、準備されたリソースを表示する、 [Azure ポータル](https://portal.azure.com) を指定したソリューション名、リソース グループに移動します。

![](media/iot-suite-remote-monitoring-sample-walkthrough/azureportal_01.png)

初めてサンプルを実行する場合は、事前に構成された、シミュレートされたデバイスが 4 つあります。

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_01.png)

デバイス管理ポータルを使用して、シミュレートされたデバイスを新しく追加できます。

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_02.png)

デバイスの管理ポータルで新しいデバイスの状態は、最初に、 **保留**:

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_03.png)

アプリケーションは、シミュレートされたデバイスのデプロイが完了したらに変更するデバイスの状態が表示されます **を実行している** 次のスクリーン ショットに示すように、デバイスの管理ポータルで。  **DeviceInfo** Stream Analytics ジョブは、デバイスの管理ポータルに、デバイスからデバイスの状態情報を送信します。

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_04.png)

などのコマンドを送信できますソリューション ポータルを使用して **ChangeSetPointTemp** デバイス。

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_05.png)

状態、デバイスは、ことが実行されるコマンドが正常にレポートをするときに変更されて **成功**:

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_06.png)

ソリューション ポータルを使用すると、モデル番号などの具体的な特性でデバイスを検索できます。

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_07.png)

デバイスを無効にし、無効になったデバイスを削除することができます。

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_08.png)

