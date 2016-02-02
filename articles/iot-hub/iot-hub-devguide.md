<properties
 pageTitle="IoT Hub の開発者ガイド トピック | Microsoft Azure"
 description="IoT Hub のエンドポイント、セキュリティ、デバイス ID レジストリ、メッセージングなどについて説明する、Azure IoT Hub の開発者ガイドです。"
 services="iot-hub"
 documentationCenter=".net"
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/29/2015"
 ms.author="elioda"/>


# Azure IoT Hub 開発者ガイド

Azure IoT Hub は、何百万もの IoT デバイスとアプリケーション バックエンドの間に信頼性のある保護された双方向通信を確立できる、完全に管理されたサービスです。

Azure IoT Hub により、次のことを実現できます。

* デバイスごとのセキュリティ資格情報とアクセス制御を使用した、通信のセキュリティ保護。
* 非常にスケール性が高く信頼性に優れた、デバイスとクラウド間の双方向メッセージング。
* 最も一般的な言語とプラットフォームのデバイスのライブラリを使用した、簡単なデバイスの接続。

このドキュメントで扱う領域を次に示します。

- [Endpoints](#endpoints). このセクションでは、各 IoT Hub がランタイムと管理の操作のために公開する、さまざまなエンドポイントについて説明します。
- [デバイス identity レジストリ](#device-identity-registry)します。 このセクションでは、各 IoT Hub のデバイス ID レジストリに格納されている情報と、その情報へのアクセス方法と変更方法について説明します。
- [Security](#security). このセクションでは、IoT Hub の機能へのアクセスを許可するために使用する、デバイスとクラウド コンポーネントの両方のセキュリティ モデルについて説明します。
- [Messaging](#messaging). このセクションでは、IoT Hub によって公開される、(デバイスとクラウド間の双方向の) メッセージング機能について説明します。
- [クォータおよび調整](#throttling)します。 このセクションでは、IoT Hub の使用に適用されるクォータの概要ついて説明します。

## エンドポイント <a id="endpoints"></a>

Azure IoT Hub はマルチテナント サービスで、さまざまなアクターの機能を公開します。 次の図は、IoT Hub によって公開されているさまざまなエンドポイントを示しています。

![IoT Hub エンドポイント][img-endpoints]

エンドポイントの説明を次に示します。

* **リソース プロバイダー**:、IoT Hub リソース プロバイダーを公開、 [Azure リソース マネージャー ][lnk-arm] IoT hub を作成、IoT hub のプロパティを更新および IoT hub を削除する Azure サブスクリプションの所有者ができるインターフェイス。 IoT Hub のプロパティは、デバイス レベルのアクセス制御ではなくハブ レベルのセキュリティ ポリシー ( 参照してください [アクセス制御](#accesscontrol)) とクラウドとデバイスとクラウドへのデバイスのメッセージングの機能のオプションです。 リソース プロバイダーすることもできます [デバイスの id をエクスポート](#importexport)します。
* **デバイス ID の管理**: 各 IoT Hub により、デバイス ID の管理 (作成、取得、更新、削除) を行うための、一連の HTTP REST エンドポイントが公開されています。 デバイス ID は、デバイスの認証とアクセス制御に使用されます。 参照してください [デバイス identity レジストリ](#device-identity-registry) の詳細。
* **デバイスのエンドポイント**: デバイス ID レジストリでプロビジョニングされたデバイスごとに、そのデバイスとの通信に使用する一連のエンドポイントが IoT Hub から公開されています。 これらのエンドポイントは両方の HTTP で公開されていると [AMQP ][lnk-amqp]:
    - *D2C メッセージの送信*。 このエンドポイントを使用して、D2C メッセージを送信します。 詳細については、次を参照してください。 [メッセージングをクラウドへのデバイス](#d2c)します。
    - *C2D メッセージの受信*。 デバイスは、このエンドポイントを使用して、そのデバイスが宛先となっている C2D メッセージを受信します。 詳細については、次を参照してください。 [デバイス メッセージングにクラウド](#c2d)します。
* **サービス エンドポイント**: 各 IoT Hub は、デバイスと通信するためにアプリケーション バックエンド (*サービス*) で使用される一連のエンドポイントも公開しています。 このエンドポイントは現在のみ公開されているを使用して、 [AMQP ][lnk-amqp] プロトコルです。
    - *D2C メッセージの受信*。 このエンドポイントは、互換性 [Azure Event Hubs ][lnk-event-hubs] と、デバイスによって送信されるすべてのデバイスからクラウドへのメッセージの読み取りに使用することができます。 詳細については、次を参照してください。 [メッセージングをクラウドへのデバイス](#d2c)します。
    - *C2D メッセージの送信と、配信の確認メッセージの受信*。 これらのエンドポイントにより、アプリケーション バックエンドは、信頼性の高い C2D メッセージを送信し、対応する配信または有効期限の確認メッセージを受信できます。 詳細については、次を参照してください。 [デバイス メッセージングにクラウド](#c2d)します。

[IoT Hub Api と Sdk ][lnk-apis-sdks] これらのエンドポイントにアクセスできるさまざまな方法について説明します。

最後に、を介してすべての IoT Hub エンドポイントが公開されることに注意する必要は [TLS ][lnk-tls], 、暗号化されていない、セキュリティ チャネルでこれまで公開されているエンドポイントがないとします。

### イベント ハブと互換性のあるエンドポイントから読み取る方法 <a id="eventhubcompatible"></a>

使用する場合、 [Azure Service Bus SDK for .NET](https://www.nuget.org/packages/WindowsAzure.ServiceBus) または [Event Hubs のイベント プロセッサ ホスト][], 、正しいアクセス許可を持つ接続文字列は IoT Hub を使用して使用して、 `メッセージやイベント` として Event Hub の名前。

Sdk (または製品の統合) を使用して IoT Hub に対応していないこと、IoT Hub の設定から、イベント ハブと互換性のあるエンドポイントと Event Hub の名前を取得する必要があります、 [Azure ポータルの][]:

1. [IoT Hub] ブレードで、**[設定]**、**[メッセージング]** の順にクリックします。
2. **[D2C の設定]** セクションに、**[イベント ハブと互換性のあるエンドポイント]**、**[イベント ハブと互換性のある名前]**、**[パーティション]** の各ボックスがあります。

    ![][img-eventhubcompatible]

> [AZURE.NOTE] SDK は、**ホスト名**または**名前空間**の値を必要とする場合があります。 その場合、**[イベント ハブと互換性のあるエンドポイント]** からスキームを削除します。 たとえば、イベント ハブと互換性のあるエンドポイントが `sb://iothub-ns-myiothub-1234.servicebus.windows.net/`, 、 **ホスト名** になります `iothub ns myiothub 1234.servicebus.windows.net`, 、および **名前空間** になります `iothub ns myiothub 1234`します。

この場合、指定したイベント ハブに接続するための **ServiceConnect** のアクセス許可を持つ、共有アクセスのセキュリティ ポリシーを使用できます。

前の情報を使用してイベント ハブの接続文字列を作成する必要がある場合は、次のパターンを使用できます。

        Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}

IoT Hub に使用できる SDK と統合の一覧を次に示します。

* [Java Event Hubs のクライアント](https://github.com/hdinsight/eventhubs-client)
* [Apache Storm のスパウト](../hdinsight/hdinsight-storm-develop-csharp-event-hub-topology.md)します。 表示することができます、 [ソースをスパウト](https://github.com/apache/storm/tree/master/external/storm-eventhubs) GitHub にします。
* [Apache Spark の統合](../hdinsight/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming.md)

## デバイス ID レジストリ

各 IoT hub は、クラウドとデバイスの実行中のメッセージを含む、キューなどのサービスでデバイスごとのリソースを作成するために使用してで説明したように、デバイスに接続するエンドポイントへのアクセスを許可するデバイスの id のレジストリ、 [アクセス制御](#accesscontrol) セクションです。

大まかに言うと、デバイス ID レジストリは、デバイス ID リソースの REST 対応のコレクションです。 次のセクションでは、デバイス ID リソースのプロパティと、レジストリで ID に対して許可されている操作について詳しく説明します。
> [AZURE.NOTE] 参照してください [IoT Hub Api と Sdk ][lnk-apis-sdks] HTTP プロトコルとデバイスの id のレジストリとの対話に使用可能な Sdk の詳細についてです。

### デバイスの id プロパティ <a id="deviceproperties"></a>

デバイス ID は、次のプロパティを持つ JSON ドキュメントとして表されます。

| プロパティ| オプション| 説明|
| -------- | ------- | ----------- |
| deviceId| 必須、読み取り専用 (更新時)| 英数字文字の ASCII 7 ビットの文字列 (最大 128 文字長) + `{'-',': ','.'、'+', '& おかない'、'_'、' & num;'、' (& a) ast; ',' ですか? ','!'、'(',')', ',', '='、'& commat;'、';'、' (& a) ドルです。 '、' ''}`します。|
| generationId| 必須、読み取り専用| ハブによって生成された大文字と小文字が区別される文字列。最大 128 文字。これは、削除され再作成された場合に、同じ **deviceId** を持つデバイスと区別するために使用します。|
| etag| 必須、読み取り専用| に従って、デバイス id の弱い etag を表す文字列 [RFC7232 ][lnk-rfc7232]します。|
| auth| 省略可能| 認証情報とセキュリティのマテリアルを含む複合オブジェクト。|
| auth.symkey| 省略可能| base64 形式でプライマリ キーとセカンダリ キーを格納する複合オブジェクト。|
| status| 必須| **[有効]** か **[無効]** のいずれか。**[有効]** の場合、デバイスからの接続が許可されます。**[無効]** の場合、このデバイスからデバイス向けのエンドポイントにアクセスできません。|
| statusReason| 省略可能| デバイス ID の状態の理由を格納する、長さが 128 文字の文字列。すべての UTF-8 文字を使用できます。|
| statusUpdateTime| 読み取り専用| 状態が最後に更新された日付と時刻。|
| connectionState| 読み取り専用| **[接続]** または **[切断]**。デバイスの接続状態に関する IoT Hub ビューを表します。|
| connectionStateUpdatedTime| 読み取り専用| 前回接続状態が更新された日付と時刻。|
| lastActivityTime| 読み取り専用| 前回デバイスが接続された、またはメッセージを送受信した日付と時刻。|

> [AZURE.NOTE] 接続状態が表すのは、その接続状態を示す IoT Hub ビューのみです。 ネットワークの状態と構成によっては、この状態が遅延することがあります。

### デバイス ID の操作

IoT Hub のデバイス ID レジストリでは、次の操作が公開されています。

* デバイス ID の作成
* デバイス ID の更新
* ID ごとのデバイス ID の取得
* デバイス ID の削除
* 最大 1000 個の ID の一覧表示

これらすべての操作で指定されているオプティミスティック同時実行制御の使用を許可する [RFC7232 ][lnk-rfc7232]します。
> [AZURE.IMPORTANT] ハブの identity レジストリ内のすべての id を取得する唯一の方法が使用するには、 [エクスポート](#importexport) 機能します。

ハブのデバイス ID レジストリには、アプリケーションのメタデータが含まれていません。また、**deviceId** をキーとして使用するディクショナリと同様にアクセスできます。ただし、詳細なクエリはサポートされていません。 IoT ソリューションには、スマート ビルディング ソリューションの温度センサーがデプロイされたルームなど、アプリケーション固有のメタデータを格納するソリューション専用ストアがあります。

### デバイスの無効化

レジストリで ID の **status** プロパティを更新することにより、デバイスを無効にすることができます。 通常これは、次の 2 つのシナリオで使用されます。

1. オーケストレーション プロセスのプロビジョニング中。 詳細については、次を参照してください。 [[lnk のガイダンス-プロビジョニング] をプロビジョニング - Azure の IoT Hub ガイダンス][lnk-guidance-provisioning]します。
2. 何らかの理由でデバイスが侵害された、または一時的に許可されていないと考えられる場合。

### デバイスの id をエクスポートします。 <a id="importexport"></a>

エクスポートは、顧客が指定する BLOB コンテナーを使用してデバイスの ID データを読み書きする、長時間実行されるジョブです。

IoT Hub リソース プロバイダーのエンドポイントで、非同期操作を使用して IoT hub の識別情報のレジストリから一括でのデバイス id をエクスポートすることができます ([エンドポイント](#endpoints))します。

エクスポート ジョブで使用できる操作を次に示します。

* エクスポート ジョブの作成
* 実行中のジョブの状態の取得
* 実行中のジョブのキャンセル

> [AZURE.NOTE] 各ハブで一度に実行できるジョブは 1 つのみです。

インポートとエクスポートの Api の詳細については、次を参照してください。 [Azure IoT Hub - リソース プロバイダー Api ][lnk-resource-provider-apis]します。

#### ジョブ

エクスポート ジョブには次のプロパティがあります。

| プロパティ| オプション| 説明|
| -------- | ------- | ----------- |
| jobId| システムにより生成、作成時は無視| |
| creationTime| システムにより生成、作成時は無視| |
| endOfProcessingTime| システムにより生成、作成時は無視| |
| type| 読み取り専用| **エクスポート**|
| status| システムにより生成、作成時は無視| **[エンキュー]**、**[開始]**、**[完了]**、**[失敗]**|
| 進捗状況| システムにより生成、作成時は無視| 完了の割合を示す整数値。|
| outputBlobContainerURI| すべてのジョブで必須| Blob コンテナーに対する書き込みアクセスで共有アクセス署名 URI を blob (を参照してください [を作成し、SAS を使用して、Blob サービス ][lnk-createuse-sas] と [Java ][lnk-sas-java])。これは、ジョブの状態と結果の出力に使用されます。|
| includeKeysInExport| 省略可能| **true** の場合、キーがエクスポートの出力に含まれます。false の場合、キーは **null** としてエクスポートされます。既定値は **false** です。|
| failureReason| システムにより生成、作成時は無視| 状態が **[失敗]** の場合、理由を含む文字列。|

#### エクスポート ジョブ

エクスポート ジョブは、BLOB の Shared Access Signature URI をパラメーターとして取得します。 これには、blob コンテナーに対する書き込みアクセスが与えられます。 操作の結果を出力するために使用します。

出力結果がという名前のファイルで指定された blob コンテナーに書き込まれる `job_{job_id}_devices.txt`します。 このファイルには、デバイスの id で指定されているの JSON としてシリアル化が含まれています。 [デバイス id プロパティ](#deviceproperties)します。 **includeKeysInExport** が **false** に設定されている場合、セキュリティ マテリアルは **null** に設定されます。

**例**:

    {"deviceId":"devA","auth":{"symKey":{"primaryKey":"123"}},"status":"enabled"}
    {"deviceId":"devB","auth":{"symKey":{"primaryKey":"234"}},"status":"enabled"}
    {"deviceId":"devC","auth":{"symKey":{"primaryKey":"345"}},"status":"enabled"}
    {"deviceId":"devD","auth":{"symKey":{"primaryKey":"456"}},"status":"enabled"}

## セキュリティ <a id="security"></a>

このセクションでは、Azure IoT Hub をセキュリティで保護するためのオプションについて説明します。

### アクセス制御 <a id="accesscontrol"></a>

IoT Hub では、次の一連の*アクセス許可*を使用して、各 IoT Hub のエンドポイントへのアクセスを許可します。 次のアクセス許可により、機能に応じて IoT Hub へのアクセスを制限します。

* **RegistryRead**。 このアクセス許可により、デバイス ID レジストリへの読み取りアクセスを許可します。 詳細については、次を参照してください。 [デバイス identity レジストリ](#device-identity-registry)します。
* **RegistryReadWrite**。 デバイス ID レジストリへの読み取りと書き込みのアクセスを許可します。 詳細については、次を参照してください。 [デバイス identity レジストリ](#device-identity-registry)します。
* **ServiceConnect**。 クラウド サービス向けの通信エンドポイントと監視エンドポイントへのアクセスを許可します。 たとえば、D2C メッセージの受信、C2D メッセージの送信、対応する配信確認メッセージの取得のアクセス許可をクラウド サービスに付与します。
* **DeviceConnect**。 デバイス向けの通信エンドポイントへのアクセスを許可します。 たとえば、D2C メッセージの送信と、C2D メッセージの受信のアクセス許可を付与します。 ほとんどの場合、このアクセス許可はデバイスによって使用されます。

アクセス許可は次の方法で付与されます。

* **ハブレベルの共有アクセス ポリシー**。 *共有アクセス ポリシー*により、前のセクションの一覧にあるアクセス許可を自由に組み合わせて付与できます。 内のポリシーを定義する、 [Azure ポータルの ][lnk-management-portal] プログラムを使用するか、 [Azure IoT Hub リソース プロバイダー Api ][lnk-resource-provider-apis]します。 新しく作成された IoT Hub には、次の既定のポリシーがあります。

    - *iothubowner*: すべてのアクセス許可を持つポリシー
    - *service*: **ServiceConnect** アクセス許可を持つポリシー
    - *device*: **DeviceConnect** アクセス許可を持つポリシー
    - *registryRead*: **RegistryRead** アクセス許可を持つポリシー
    - *registryReadWrite*: **RegistryRead** アクセス許可と **RegistryWrite** アクセス許可を持つポリシー

* **デバイスごとのセキュリティ資格情報**。 各 IoT Hub を含む、 [デバイス identity レジストリ](#device-identity-registry)します。 このレジストリ内の各デバイスでは、対応するデバイスのエンドポイントを対象として **DeviceConnect** アクセス許可を付与する、セキュリティ資格情報を構成できます。

**例**。 IoT ソリューションには通常、*registryReadWrite* ポリシーを使用するデバイス管理コンポーネントと、*service* ポリシーを使用するイベント プロセッサ コンポーネントおよびランタイム デバイス ビジネス ロジック コンポーネントがあります。 個々のデバイスは、IoT Hub の ID レジストリに格納されている資格情報を使用して接続します。

IoT Hub セキュリティ トピックに関するガイダンスについては、セキュリティ」セクションを参照してください。 [Azure IoT Hub ガイダンス ][lnk-guidance-security]します。

### 認証

Azure IoT Hub では、共有アクセス ポリシーとデバイス ID レジストリのセキュリティ資格情報に対してトークンを確認することにより、エンドポイントへのアクセスを許可します。

対称キーなどのセキュリティの資格情報がネットワーク上で送信されることはありません。
> [AZURE.NOTE] Azure サブスクリプションを通じて Azure IoT Hub リソース プロバイダーはセキュリティで保護のすべてのプロバイダーにも、 [Azure リソース マネージャー ][lnk-azure-resource-manager]します。

#### セキュリティ トークンの形式 <a id="tokenformat"></a>

セキュリティ トークンには、次の形式があります。

    SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}

考えられる値を次に示します。

| 値| 説明|
| ----- | ----------- |
| {signature}| フォームの hmac-sha256 署名文字列: `{URL でエンコードされた-resourceURI}"\n"+ 有効期限`|
| {resourceURI}| このトークンを使用してアクセスできるエンドポイントの (セグメント単位の) URI プレフィックス。たとえば、 `/events`|
| {expiry}| 1970 年 1 月 1 日の 00 時 00 分 00 秒 UTC からのエポック秒で表される UTF8 文字列。|
| {URL-encoded-resourceURI}| URL でエンコードされたリソース URI (小文字)|
| {policyName}| このトークンの参照先となる共有アクセス ポリシーの名前。デバイスとレジストリの資格情報を参照するトークンの場合は存在しません。|

**プレフィックスに関する注意事項**: URI プレフィックスは、文字単位ではなくセグメント単位で計算されます。 たとえば `/a と b` のプレフィックス `/a/b/c` のではなく `a/bc`します。

#### プロトコルの詳細

(AMQP や HTTP など) サポートされているプロトコルごとに、さまざまな方法でトークンが転送されます。

HTTP 認証は、**Authorization** 要求ヘッダーに有効なトークンを含めることによって実装されます。 **Authorization** というクエリ パラメーターでも、トークンを転送できます。

使用する場合 [AMQP ][lnk-amqp], 、IoT Hub をサポートしています [SASL プレーン ][lnk-sasl-plain] と [AMQP クレーム ベースのセキュリティ ][lnk-cbs]します。

AMQP Claims-Based-Security の場合、標準でこれらのトークンの送信方法が指定されます。

SASL PLAIN では、**ユーザー名**を以下のように指定できます。

* `{policyName} & commat;sas.root {。iothubName}` ハブ レベル トークンの場合。
* `{deviceid}` デバイス スコープのトークンの場合。

どちらの場合も、パスワード フィールドに含まれる、トークン」の説明に従って、 [トークン形式](#tokenformat) セクションです。
> [AZURE.NOTE] [Azure IoT Hub Sdk ][lnk-apis-sdks] サービスに接続するときに自動的にトークンを生成します。 場合によっては、サポートするプロトコル、または使用可能な認証方法で SDK が制限されます。 詳細については、次を参照してください。、 [Azure IoT Hub Sdk ][lnk-apis-sdks] ドキュメントです。

#### SASL PLAIN と CBS の比較

SASL PLAIN を使用する場合、IoT Hub に接続するクライアントは、TCP 接続ごとにトークンを 1 つ使用できます。 トークンの有効期限が切れると、サービスから TCP 接続が切断され、再接続がトリガーされます。 この動作は、アプリケーション バックエンド コンポーネントの場合は問題ありませんが、デバイス側アプリケーションの場合は、次の理由から大きな損害を及ぼす可能性があります。

*  ゲートウェイは、通常多くのデバイスの代理として接続しています。 SASL PLAIN を使用する場合、IoT Hub に接続するデバイスごとに、個別の TCP 接続を作成する必要があります。 これにより、電源とネットワーク リソースの消費量が大幅に増大し、各デバイスの接続の待機時間が増加します。
* リソースが限られたデバイスは、各トークンの有効期限が切れた後に、再接続に使用するリソースの量が増加すると、多くの場合影響を受けます。

### ハブレベルの資格情報のスコープ

制限付きのリソース URI を持つトークンを作成することにより、ハブレベルのセキュリティ ポリシーのスコープを指定できます。 たとえば、デバイスからの D2C メッセージを送信するエンドポイントは `/devices/{deviceId}/events` になります。 また、**DeviceConnect** アクセス許可を持つハブレベルの共有アクセス ポリシーを使用して、resourceURI が `/devices/{deviceId}` であるトークンに署名し、デバイスの **deviceId** の代わりにデバイスを送信する場合にのみ使用可能なトークンを作成できます。

このメカニズムに似ています、 [Event Hubs 発行者ポリシー ][lnk-event-hubs-publisher-policy] でき、独自の認証方式の実装のセキュリティのセクションで説明したよう [Azure IoT Hub ガイダンス ][lnk-guidance-security]します。

## メッセージング

IoT Hub には、アプリケーション バックエンド (*サービス*または*クラウド*) からデバイス、またはその逆方向の通信を行うための、メッセージング プリミティブが用意されています。 これらの機能と呼ば [デバイスからクラウド](#d2c) と [クラウドとデバイス](#c2d)します。

IoT Hub のメッセージング機能の中心となる特性は、メッセージの信頼性と持続性です。 これにより、デバイス側では断続的な接続に対する復元性を、クラウド側ではイベント処理の負荷の急増に対する復元性を実現できます。 IoT Hub では、D2C と C2D の両方のメッセージについて、*少なくとも 1 回*の配信の保証が実装されます。

IoT Hub では、(AMQP や HTTP/1 などの) デバイス用のプロトコルが複数サポートされています。 プロトコル間でのシームレスな相互運用性をサポートするために、IoT Hub では、すべてのデバイス用プロトコルでサポートされる共通のメッセージ形式が定義されます。

### メッセージ形式 <a id="messageformat"></a>

IoT Hub のメッセージは、次の要素で構成されています。

* 一連の*システム プロパティ*。 これらは、IoT Hub によって解釈または設定されるプロパティです。 この設定は、事前に決定されています。
* 一連の*アプリケーション プロパティ*。 これは、本文を逆シリアル化しなくてもアプリケーションが定義してアクセスできる、文字列プロパティのディクショナリです。 IoT Hub でこれらのプロパティが変更されることはありません。
* 非透過的なバイナリ本文。

参照してください [IoT Hub Api と Sdk ][lnk-apis-sdks] さまざまなプロトコルでメッセージをエンコードする方法の詳細についてです。

これは、IoT Hub メッセージ内の一連のシステム プロパティです。

| プロパティ| 説明|
| -------- | ----------- |
| MessageId| 通常、要求/応答パターンに使用する、メッセージのユーザー設定 ID。形式: 文字列 (最大 128 文字長) ASCII 7 ビットの英数字文字を使用 + `{'-',': ','.'、'+', '%'、'_'、'#' '* ','? ','!'、'(',')', ',', '='' @'、';', '$' ''}`します。|
| Sequence number| IoT Hub によって各 C2D メッセージに割り当てられる数値 (デバイスとキューごとに一意)。|
| To| 使用される [クラウドとデバイス](#c2d) 、メッセージの送信先にします。|
| ExpiryTimeUtc| メッセージの有効期限の日時。|
| EnqueuedTime| IoT Hub でメッセージを受信した時刻。|
| CorrelationId| 通常、要求/応答パターンで要求のメッセージ ID を格納する文字列プロパティ。|
| UserId| メッセージの送信元の指定に使用します。設定されているメッセージが IoT Hub で生成されると、 `{iot hub name}`します。|
| Ack| デバイスがメッセージを使用した結果としてのフィードバック メッセージの生成を IoT Hub に要求するために、C2D メッセージで使用します。使用可能な値: **none** (既定値): フィードバック メッセージは生成されません。**positive**: メッセージが完了した場合にフィードバック メッセージを受信します。**negative**: デバイスでメッセージが完了しないまま、メッセージの有効期限が切れた場合 (または最大配信数に達した場合) にフィードバック メッセージを受信します。**full**: positive と negative の両方の値。詳細については、次を参照してください。 [フィードバック メッセージ](#feedback)します。|
| ConnectionDeviceId| D2C メッセージで IoT Hub によって設定されます。メッセージを送信したデバイスの **deviceId** が含まれます。|
| ConnectionDeviceGenerationId| D2C メッセージで IoT Hub によって設定されます。含まれている、 **generationId** (に従って [デバイス id プロパティ](#deviceproperties)) メッセージを送信したデバイスのです。|
| ConnectionAuthMethod| D2C メッセージで IoT Hub によって設定されます。メッセージを送信するデバイスの認証に使用する認証方法に関する情報。詳細については、次を参照してください。 [D2C アンチ スプーフィング](#antispoofing)します。|

### 通信プロトコルを選択します。 <a id="amqpvshttp"></a>

Iot Hub は、両方をサポート、 [AMQP ][lnk-amqp] とデバイス側の通信に HTTP/1 プロトコルです。 その使用方法に関する考慮事項の一覧を次に示します。

* **C2D のパターン**。 HTTP/1 には、サーバー プッシュを実装する効率的な方法がありません。 そのため、HTTP/1 を使用する場合、デバイスは、C2D メッセージの IoT Hub をポーリングします。 これは、デバイスと IoT Hub の両方できわめて非効率的です。 現在のガイドラインでは、HTTP/1 を使用する場合、各デバイスで 25 分に 1 回未満のポーリング間隔を設定するようになっています。 一方 AMQP では、C2D メッセージを受信する場合のサーバー プッシュがサポートされていて、IoT Hub からデバイスへのメッセージも即座にプッシュできます。 配信の待機時間が問題となる場合は、AMQP が使用に最適なプロトコルです。 一方、頻繁に接続されないデバイスの場合は、HTTP/1 でも機能します。
* **フィールド ゲートウェイ**。 サーバー プッシュに関する HTTP/1 の制限を指定するには、これは適していませんで使用される [フィールド ゲートウェイ シナリオ ][lnk-azure-gateway-guidance]します。
* **リソースの少ないデバイス**。 HTTP/1 のライブラリは、AMQP のライブラリよりもはるかに小規模です。 そのため、リソースが少ないデバイス (RAM が 1 MB 未満など) の場合、HTTP/1 が実装可能な唯一のプロトコルとなります。
* **ネットワーク トラバーサル**。 AMQP Standard は、ポート 5672 でリッスンします。 これにより、HTTP 以外のプロトコルに限定されているネットワークの場合は、問題が発生する可能性があります。
* **ペイロードのサイズ**。 AMQP は、HTTP/1 よりもはるかに簡潔なバイナリ プロトコルです。

大まかに言うと、可能な限り AMQP の使用をお勧めします。また、デバイスのリソースやネットワーク構成の関係上、AMQP が許可されない場合は HTTP/1 の使用をお勧めします。 さらに、HTTP/1 を使用する場合は、各デバイスでポーリングの頻度が 25 分に 1 回未満になるように設定してください。 当然のことですが、開発段階では、より高いポーリングの頻度を指定できます。

最後の考慮事項とすることが重要を参照してください、 [Azure IoT プロトコル ゲートウェイ ][lnk-azure-protocol-gateway], 、IoT Hub と直接やり取りするパフォーマンスの高い MQTT ゲートウェイを展開できます。 MQTT では、サーバー プッシュがサポートされています (したがって、デバイスに C2D メッセージを即座に配信できます)。また、リソースが非常に少ないデバイスでも使用できます。 このアプローチの主な短所は、プロトコル ゲートウェイの自己ホストと管理が必要な点です。

### クラウドへのデバイス <a id="d2c"></a>

詳しく説明したよう、 [エンドポイント](#endpoints) ] セクションで、デバイスに接続するエンドポイントを通じて、デバイスからクラウドへのメッセージが送信されます (具体的には `/devices/{deviceid}/メッセージ/イベント`)、およびサービスに接続するエンドポイントを介して受信した (`/メッセージ/イベント`)、つまりと互換性のある [Event Hubs ][lnk-event-hubs]します。 Standard Event Hubs の統合と SDK を使用してメッセージを受信できます。

IoT Hub は、クラウドへのデバイスは、次のような方法でメッセージングを実装する [Event Hubs ][lnk-event-hubs], 、Event Hubs のようにする IoT Hub のデバイスからクラウド メッセージ *イベント* よりも [Service Bus ][lnk-servicebus] *メッセージ*します。

この実装には、次のような意味があります。

* Event Hubs と同様に *イベント*, 、デバイスからクラウド メッセージは、永続的な IoT hub で保持されている最大 7 日間 (を参照してください [クラウドへのデバイスの構成オプション](#d2cconfiguration))します。
* デバイスからクラウドへのメッセージが作成時に設定されているパーティションの固定セットでパーティション分割 (を参照してください [クラウドへのデバイスの構成オプション](#d2cconfiguration))します。
* Event Hubs と同様に、D2C メッセージを読み取るクライアントでは、パーティションと、チェックポイントを処理する必要があります。 参照してください [Event Hubs のイベントの ][lnk-event-hubs-consuming-events]します。
* Event Hubs のイベントと同様、D2C メッセージのサイズは最大 256 KB で、バッチとしてグループ化して送信を最適化できます。 バッチでも最大 256 KB、メッセージ数にして最大 500 個です。

ただし、IoT Hub の D2C と Event Hubs には、いくつかの重要な違いがあります。

* 説明に従って、 [セキュリティ](#security) ] セクションで、IoT Hub は、デバイスごとの認証とアクセス制御
* IoT Hub は、何百万もの同時に接続されているデバイス (を参照してください [クォータおよび調整](#throttling)), Event Hubs は、名前空間ごとに 5000 の AMQP 接続に制限されますが、します。
* IoT Hub では、**PartitionKey** を使用した任意のパーティション分割は許可されていません。 D2C メッセージは、発信元の **deviceId** を基準にパーティション分割されています。
* IoT Hub のスケーリングは Event Hubs と若干異なります。 詳細については、次を参照してください。 [スケーリング IoT Hub ][lnk-guidance-scale]します。

上記の説明は、IoT Hub がすべてのシナリオで Event Hubs に代わるという意味ではないので、ご注意ください。 たとえば、一部のイベントの計算処理では、データ ストリームを分析する前に、別のプロパティまたはフィールドを基準にイベントを再パーティション分割する必要があります。 その場合、Event Hub を使用して、ストリーム処理パイプラインの 2 つの部分を分離できます。

デバイスとクラウド メッセージングを使用する方法の詳細については、次を参照してください。 [IoT Hub Api と Sdk ][lnk-apis-sdks]します。

#### 非テレメトリ トラフィック

多くの場合、デバイスは、テレメトリ データ ポイントをアプリケーション バックエンドに送信するだけでなく、アプリケーションのビジネス ロジック層での実行と処理を必要とする、*対話型*のメッセージと要求も送信します。 バックエンドで特定のアクションをトリガーする必要がある重大なアラートや、コマンドに対するデバイスの応答などがその良い例です。

参照してください、 [クラウドへのデバイスの処理 ][lnk-guidance-d2c-processing] このようなメッセージを処理する最善の方法の詳細については、"IoT Hub Guidance"のセクションです。

#### クラウドへのデバイスの構成オプション <a id="d2cconfiguration"></a>

IoT Hub では、D2C メッセージングを制御する次のプロパティを公開しています。

* **パーティション数**。 このプロパティは作成時に設定し、D2C イベントを取り込む場合のパーティション数を定義します。
* **保存期間**。 このプロパティでは、D2C メッセージのリテンション期間を指定します。 既定値は 1 日ですが、最大 7 日まで増やすことができます。

また Event Hubs と同様、IoT Hub でも、D2C の受信エンドポイントでコンシューマー グループを管理できます。

いずれかを使用してこれらすべてのプロパティを変更することができます、 [Azure ポータルの ][lnk-management-portal], 、または使用してプログラムによって、 [Azure IoT Hub - リソース プロバイダー Api ][lnk-resource-provider-apis]します。

#### スプーフィング対策プロパティ <a id="antispoofing"></a>

D2C メッセージでのデバイスのなりすましを回避するために、IoT Hub では、すべてのメッセージに次のプロパティを持つスタンプが使用されます。

* **ConnectionDeviceId**
* **ConnectionDeviceGenerationId**
* **ConnectionAuthMethod**

最初の 2 つを含む、 **deviceId** と **generationId** 元のデバイスのに従って [デバイス id プロパティ](#deviceproperties)します。

**ConnectionAuthMethod** プロパティには、次のプロパティを使用してシリアル化された JSON オブジェクトが含まれています。

    {
        "scope": "{ hub | device}",
        "type": "{ symkey | sas}",
        "issuer": "iothub"
    }

### デバイスへのクラウドします。 <a id="c2d"></a>

既にで詳細に説明、 [エンドポイント](#endpoints) ] セクションで、クラウドとデバイスのメッセージは、サービスに接続するエンドポイントを通じて送信されます (`/メッセージ/devicebound`)、一連のデバイスに接続されたエンドポイントで受信されると (`/devices/{deviceid}/メッセージ/devicebound`)。

各 C2D メッセージで 1 つのデバイスを対象にし、**to** プロパティを `/devices/{deviceId}/messages/devicebound` に設定します。

**重要**: 各デバイスのキューで保持できる C2D メッセージは最大 50 個です。 同じデバイスにそれ以上のメッセージを送信しようとすると、エラーが発生します。

#### メッセージのライフ サイクル <a id="message lifecycle"></a>

*少なくとも 1 回*の配信の保証を実装するために、C2D メッセージはデバイスごとのキューに保持されます。そのため、IoT Hub がキューからメッセージを削除できるように、デバイスで明示的に*完了*を確認する必要があります。 そうすることで、接続とデバイスの障害に対する復元性が保証されます。

次の図に、C2D メッセージのライフサイクルの状態に関するグラフを示します。

![クラウドとデバイス間のメッセージのライフ サイクル][img-lifecycle]

サービスによってメッセージが送信されると、そのメッセージは*エンキュー*されたと見なされます。 デバイスでメッセージを*受信*する場合、同じデバイス上の他のスレッドが他のメッセージの受信を開始できるようにするために、IoT Hub によりメッセージが*ロック*されます (状態が **[非表示]** になります)。 デバイスのスレッドによってデバイスの処理が完了したら、メッセージを*完了*して IoT Hub にその旨を通知します。 メッセージは*拒否*することもできます。この場合、状態が **[配信不能]** になるか、またはメッセージが*放棄*されてキューに戻されます (状態が **[エンキュー]** になります)。

スレッドがメッセージの処理に失敗して IoT Hub に通知されない場合もあるため、*表示 (またはロック) のタイムアウト* (既定値: 1 分) 後に、メッセージの状態が自動的に **[非表示]** から **[エンキュー]** に戻ります。 IoT Hub の*最大配信数*のプロパティで指定された回数を限度として、メッセージの状態を **[エンキュー]** と **[非表示]** の間で切り替えることができます。 上記の移行回数に達すると、メッセージは自動的に配信不能となります。 同様に、メッセージが自動的にされます配信不能な期限後に (を参照してください [有効期限](#ttl))します。

クラウドとデバイス間のメッセージのチュートリアルについては、次を参照してください。 [Azure IoT Hub クラウドとデバイス間のメッセージ ][lnk-getstarted-c2d-tutorial]します。 さまざまな Api と Sdk の参照トピックでは、クラウドとデバイスの機能を公開を参照してください [IoT Hub Api と Sdk ][lnk-apis-sdks]します。
> [AZURE.NOTE] 通常、メッセージの損失がアプリケーション ロジックに影響しない場合、C2D メッセージは必ず完了されます。 これに該当するシナリオはさまざまです。たとえば、メッセージの内容が正常にローカル ストレージに格納された場合、操作が正常に実行された場合、メッセージの損失がアプリケーションの機能に影響しないことを示す一時的な情報がメッセージに含まれている場合などがあります。 長時間実行されるタスクの場合、タスクの説明をローカル ストレージ上に保持した後で C2D メッセージを完了し、その後、タスクの進行状況のさまざまな段階で、1 つ以上の D2C メッセージによってアプリケーション バックエンドに通知するのが一般的です。

#### 有効期限 <a id="ttl"></a>

すべての C2D メッセージに有効期限があります。 これはサービス (**ExpiryTimeUtc** プロパティ) で明示的に設定するか、IoT Hub のプロパティとして指定した既定の*有効期限*を使用して IoT Hub で設定できます。 参照してください [クラウドとデバイスの構成オプション](#c2dconfiguration)します。

#### メッセージからのフィードバック <a id="feedback"></a>

C2D メッセージを送信するときに、サービスは、そのメッセージの最終状態に関するメッセージごとのフィードバックの配信を要求できます。 **Ack** プロパティを **positive** に設定すると、C2D メッセージの状態が **[完了]** に達した場合に限り、IoT Hub によりフィードバックのメッセージが生成されます。 **Ack** プロパティを **negative** に設定すると、C2D メッセージの状態が **[配信不能]** に達した場合に限り、IoT Hub によりフィードバックのメッセージが生成されます。 **Ack** プロパティを **full** に設定すると、上記のどちらの場合にも IoT Hub によりフィードバック メッセージが生成されます。

説明したよう [エンドポイント](#endpoints), 、フィードバックがサービスに接続されたエンドポイントで配信されます (`/messages/servicebound/feedback`) メッセージとして。 フィードバックの受信セマンティクスは、同じ[メッセージのライフサイクル](#message lifecycle)を持つ C2D メッセージの場合と同様です。 可能な限り、メッセージのフィードバックは、次の形式で 1 つのメッセージのバッチとして処理します。

フィードバックのエンドポイントから取得された各メッセージには、次のプロパティがあります。

| プロパティ| 説明|
| -------- | ----------- |
| EnqueuedTime| バッチが作成された日時を示すタイムスタンプ。|
| UserId| `{iot hub name}`|
| ContentType| `application/vnd.microsoft.iothub.feedback.json`|

本文はシリアル化された JSON レコードの配列で、それぞれ次のプロパティを持っています。

| プロパティ| 説明|
| -------- | ----------- |
| EnqueuedTimeUtc| メッセージの結果が発生した日時を示すタイムスタンプ。たとえば、デバイスの完了やメッセージの期限切れなどです。|
| OriginalMessageId| このフィードバック情報が関連付けられている C2D メッセージの **MessageId** です。|
| 説明| 前の結果の文字列の値。|
| DeviceId| フィードバックのこの部分が関連付けられている C2D メッセージの宛先デバイスの **DeviceId**。|
| DeviceGenerationId| フィードバックのこの部分が関連付けられている C2D メッセージの宛先デバイスの **DeviceGenerationId**。|

**重要**。 メッセージのフィードバックを元のメッセージと関連付けることができるように、サービスは C2D メッセージの **MessageId** を指定する必要があります。

**例**。 フィードバック メッセージの本文の例を次に示します。

    [
        {
            "OriginalMessageId": "0987654321",
            "EnqueuedTimeUtc": "2015-07-28T16:24:48.789Z",
            "Description": "Success",
            "DeviceId": "123",
            "DeviceGenerationId": "abcdefghijklmnopqrstuvwxyz"
        },
        {
            ...
        },
        ...
    ]

#### クラウドとデバイスの構成オプション <a id="c2dconfiguration"></a>

各 IoT Hub では、C2D メッセージング用に次の構成オプションを公開しています。

| プロパティ| 説明| 範囲と既定値|
| -------- | ----------- | ----------------- |
| defaultTtlAsIso8601| C2D メッセージの TTL の既定値。| 最大 2D の ISO_8601 書式による間隔 (最小 1 分)。既定値: 1 時間。|
| maxDeliveryCount| デバイスごとの C2D キューの最大配信数。| 1 ～ 100。既定値: 10。|
| feedback.ttlAsIso8601| サービス宛てのフィードバックのメッセージの保有期間。| 最大 2D の ISO_8601 書式による間隔 (最小 1 分)。既定値: 1 時間。|
| feedback.maxDeliveryCount| フィードバック キューの最大配信数。| 1 ～ 100。既定値: 100。|

## クォータと調整 <a id="throttling"></a>

各 Azure サブスクリプションに最大 10 個の IoT Hub を割り当てることができます。

各 IoT hub がプロビジョニングされて特定の SKU の単位数 (詳細については、次を参照してください。 [IoT Hub の料金 ][lnk-pricing])。 SKU とユニット数により、送信できるメッセージの 1 日あたりの最大クォータと、ID レジストリ内のデバイス ID の最大数が決まります。 同時に接続されるデバイスの数は、レジストリ内の ID の数によって制限されます。

また、IoT Hub によって操作に適用されるスロットルの制限も決まります。

### デバイス ID レジストリのクォータ

IoT Hub で許可される、1 日のユニットあたりの (SKU に関係なく) デバイスの更新 (作成、更新、削除) は、最大で 1100 個です。

### 操作のスロットル

操作のスロットルは、ごく限られた範囲に適用される、不正使用を回避するためのレート制限です。 IoT Hub は、可能な限りエラーを返さないようにしようとしますが、長時間にわたりスロットル違反が続く場合は、例外が返され始めます。

適用されるスロットルの一覧を次に示します。 値は個々のハブのものです。

| スロットル| ハブごとの値|
| -------- | ------------- |
| ID レジストリの操作 (作成、取得、一覧表示、更新、削除)、個別または一括のインポートとエクスポート| 100/分/ユニット、最大 5000/分|
| デバイスの接続| 100/秒/ユニット|
| D2C の送信| 2000/分/ユニット (S2 の場合)、60/分/ユニット (S1 の場合)。最小 100/秒。|
| C2D の操作 (送信、受信、フィードバック)| 100/分/ユニット|

**注**: 任意の時点で、IoT Hub にプロビジョニングされたユニットを増やすことで、クォータやスロットルの制限値を増やすことができます。

## 次のステップ

IoT Hub の開発の概要については以上です。詳細については、以下のリンク先にアクセスしてください。

- [IoT Hub (チュートリアル) ][lnk-get-started]
- [OS プラットフォームとハードウェアの互換性 ][lnk-compatibility]
- [Azure の IoT デベロッパー センター ][lnk-iotdev]
- [IoT 実装 ][lnk-guidance]


[event hubs - event processor host]: http://blogs.msdn.com/b/servicebus/archive/2015/01/16/event-processor-host-best-practices-part-1.aspx 
[azure portal]: https://portal.azure.com 
[img-summary]: ./media/iot-hub-devguide/summary.png 
[img-endpoints]: ./media/iot-hub-devguide/endpoints.png 
[img-lifecycle]: ./media/iot-hub-devguide/lifecycle.png 
[img-eventhubcompatible]: ./media/iot-hub-devguide/eventhubcompatible.png 
[lnk-compatibility]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/tested_configurations.md 
[lnk-apis-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md 
[lnk-azure-hub-sdks]: https://msdn.microsoft.com/library/mt488521.aspx 
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub 
[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx 
[lnk-reference-architecture]: iot-hub-what-is-azure-iot.md 
[lnk-azure-gateway-guidance]: iot-hub-guidance.md#fieldgateways 
[lnk-guidance-provisioning]: iot-hub-guidance.md#provisioning 
[lnk-guidance-scale]: iot-hub-scaling.md 
[lnk-guidance-security]: iot-hub-guidance.md#customauth 
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md 
[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md 
[lnk-guidance]: iot-hub-guidance.md 
[lnk-getstarted-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md 
[lnk-amqp]: https://www.amqp.org/ 
[lnk-arm]: ../resource-group-overview.md 
[lnk-azure-resource-manager]: https://azure.microsoft.com/documentation/articles/resource-group-overview/ 
[lnk-cbs]: https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc 
[lnk-createuse-sas]: ../storage/storage-dotnet-shared-access-signature-part-2/ 
[lnk-event-hubs-publisher-policy]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab 
[lnk-event-hubs]: http://azure.microsoft.com/services/event-hubs/ 
[lnk-event-hubs-consuming-events]: ../event-hubs/event-hubs-programming-guide.md#event-consumers 
[lnk-guidance-d2c-processing]: iot-hub-csharp-csharp-process-d2c.md 
[lnk-management-portal]: https://portal.azure.com 
[lnk-rfc7232]: https://tools.ietf.org/html/rfc7232 
[lnk-sas-java]: https://msdn.microsoft.com/library/azure/Hh875756.aspx 
[lnk-sasl-plain]: http://tools.ietf.org/html/rfc4616 
[lnk-servicebus]: http://azure.microsoft.com/services/service-bus/ 
[lnk-tls]: https://tools.ietf.org/html/rfc5246 
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/ 

