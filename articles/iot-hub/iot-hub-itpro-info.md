<properties
 pageTitle="IT プロフェッショナル向けの Azure IoT Hub の情報 | Microsoft Azure"
 description="IT プロフェッショナルがポート要件やセキュリティ バックグラウンドなど、Azure IoT Hub を使用するときに役立つ情報です。"
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="12/15/2015"
 ms.author="dobett"/>

# IoT Hub へのアクセスの構成と管理

この記事には、ネットワーク インフラストラクチャ上で IoT デバイスが IoT Hub と通信する環境を IT プロフェッショナルが構成する際に役立つ情報が記載されています。

## ネットワーク接続

デバイスは、AMQP または HTTPS のいずれかのプロトコルを使用して Azure の IoT Hub と通信します。 通常、プロトコルの選択は、ソリューションの特定の要件によって左右されます。 次の表に、デバイスが特定のプロトコルを使用できるようにするために開く必要がある送信ポートを示します。

| プロトコル | ポート |
| -------- | ------- |
| HTTPS    | 443     |
| AMQP     | 5671    |

Azure リージョンに IoT Hub を作成すると、そのハブはハブの存続期間中同じ IP アドレスを保持します。 ただし、障害復旧シナリオでは、Microsoft によって IoT Hub が別のスケール ユニットに移動されると、新しい IP アドレスが割り当てられます。

## IoT Hub とセキュリティ

IoT Hub に登録されているデバイスのみが、その IoT Hub と通信できます。 登録されたデバイスを許可する必要があります、 *DeviceConnect* 権限です。 デバイスはデバイスの一意の ID は、各要求でをカプセル化するトークンを含めることによって識別する、ハブ、トークンの有効性をチェックして、デバイスがないブラック リストに掲載される (*DeviceConnect* アクセス権が無効)。

IoT hub の他の管理エンドポイントへのアクセスが一連のアクセス許可の制御も: *iothubowner*, 、*サービス*, 、*registryRead*, 、および *registryReadWrite*します。 IoT Hub に接続しているすべてのクライアント管理のアプリケーションには、適切なアクセス許可を持つトークンが含まれている必要があります。

## 次のステップ

この記事には、開発環境およびテスト環境を構成する IT プロフェッショナルと開発者向けの具体的な情報が含まれています。 Azure IoT Hub サービスについてさらに学習するには、次のリンクを使用してください。

- [Azure IoT Hub とは][lnk-iothub]
-  [セキュリティ] セクションで、Azure の IoT Hub 開発者ガイド][lnk-devguide] トークンおよび IoT Hub でのアクセス許可システムに関する追加情報を提供します。
- [IoT Hub、Azure ポータルを使用して管理][lnk-manage-portal] Azure ポータルを使用して IoT hub を管理する方法について説明します。

[lnk-iothub]: iot-hub-what-is-iot-hub.md
[lnk-devguide]: iot-hub-devguide.md#security
[lnk-manage-portal]: iot-hub-manage-through-portal.md

