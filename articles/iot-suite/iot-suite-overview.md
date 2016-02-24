<properties
    pageTitle="Microsoft Azure IoT Suite の概要 | Microsoft Azure"
    description="パッケージ化と構成済みのソリューションを含む、Azure IoT Suite の概要を説明します。"
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
     ms.date="11/27/2015"
     ms.author="dobett"/>

# Azure IoT Suite の概要

Azure IoT サービスは、さまざまな機能を提供しています。 このエンタープライズ クラスのサービスを使用すると、次の操作を実行できます。

- デバイスからデータを収集する
- インモーション データ ストリームを分析する
- 大規模なデータ セットを格納して照会する
- リアルタイム データと履歴データを表示する
- バックオフィス システムと統合する

Azure IoT Suite には、一般的に利用されている Azure サービスとカスタム拡張機能が構成済みソリューションとしてパッケージ化されています。 構成済みソリューションは、一般的な IoT ソリューション パターンの基本実装です。このソリューションを使用して、IoT ソリューションを提供する時間を短縮できます。 使用して、 [IoT ソフトウェア開発キット][lnk-sdks], 、独自の要件を満たしているか、新しいソリューションを開発する場合を例として活用してにこれらの事前に構成されたソリューションを簡単にカスタマイズすることができます。

次のビデオで、Azure IoT Suite の概要について説明します。

> [AZURE.VIDEO azurecon-2015-introducing-the-microsoft-azure-iot-suite]

## Azure IoT Suite の Azure IoT サービス

Azure の IoT スイートに core は、 [Azure IoT Hub][lnk-iot-hub] サービスです。 このサービスは、デバイスからクラウドへ、およびクライアントからデバイスへのメッセージング機能を提供し、クラウドや他の主要な IoT Suite サービスに対するゲートウェイとして機能します。

[Azure Stream Analytics][lnk-asa] モーションでデータ分析を提供します。 IoT Suite は、このサービスを利用して、受信テレメトリを処理し、集計を実行してイベントを検出します。 また、事前構成されたソリューションは、ストリーム分析を使用して、メタデータやデバイスからのコマンド応答などのデータを含む情報メッセージも処理します。

[Azure ストレージ][lnk-azure-storage] と [Azure DocumentDB][lnk-document-db] データ記憶域の機能を提供します。 構成済みソリューションでは、BLOB ストレージを使用してテレメトリを保存し、分析に使用することができます。 構成済みのソリューションでは、デバイス メタデータを管理するのに、DocumentDB の半構造化データの機能でインデックス付きのストレージを使用します。 その結果、ストレージ要件が異なる複数種類のデバイスをソリューションで管理できるようになります。

[Azure Web Apps][lnk-web-apps] と [Microsoft Power BI][lnk-power-bi] データ視覚化機能を提供します。 Power BI は柔軟なので、IoT Suite データを使用する独自の対話型ダッシュボードをすばやく構築できます。

標準的な IoT ソリューションのアーキテクチャの概要については、次を参照してください。 [、モ ノのインターネット (IoT)、Microsoft Azure および][iot-suite-what-is-azure-iot]です。

## 構成済みのソリューション

IoT Suite には構成済みソリューションが含まれているので、Azure IoT Suite で実現できる一般的な IoT シナリオをすぐに始めて、調べることができます。 構成済みソリューションを Azure サブスクリプションにデプロイし、完全なエンドツーエンドの IoT ソリューションを実行することができます。

## 次のステップ

IoT スイートで構成済みのソリューションの詳細については、次を参照してください。 [ソリューションが事前に構成とは、Azure の IoT でしょうか。][lnk-what-are-preconfig]

構成済みのソリューションの 1 つを使用して開始するを参照してください。 [ソリューションが事前に構成、IoT の概要][lnk-preconfig-start]します。

Azure の IoT Hub サービスに関する詳細についてを参照してください、 [IoT Hub ドキュメント][lnk-iot-hub]します。


[lnk-sdks]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-azure-storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-document-db]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-power-bi]: https://powerbi.microsoft.com/
[lnk-web-apps]: https://azure.microsoft.com/documentation/services/app-service/web/
[iot-suite-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-what-are-preconfig]: iot-suite-what-are-preconfigured-solutions.md
[lnk-preconfig-start]: iot-suite-getstarted-preconfigured-solutions/
[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
