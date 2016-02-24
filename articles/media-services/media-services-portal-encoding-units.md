<properties
    pageTitle="Azure クラシック ポータルを使用してメディア処理の規模を設定する方法"
    description="アカウントにプロビジョニングする [オンデマンド ストリーミング占有ユニット] および [エンコード占有ユニット] の数を指定することでメディア サービスの規模を設定する方法について説明します。"
    services="media-services"
    documentationCenter=""
    authors="juliako,milangada"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/15/2015"
    ms.author="juliako"/>


# Azure クラシック ポータルを使用してメディア処理の規模を設定する方法

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-encoding-units.md)
- [ポータル](media-services-portal-encoding-units.md)
- [REST ()](https://msdn.microsoft.com/library/azure/dn859236.aspx)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

## 概要

Media Services アカウントは、メディア処理タスクを処理する速度を決定する予約ユニットの種類に関連付けられます。 占有ユニットの種類を選択することができます: **基本的な**, 、**標準**, 、または **Premium**します。 たとえば、同じエンコード ジョブを実行高速化を使用する場合、 **標準** 占有ユニットの種類、 **基本的な** 型です。 詳細については、次を参照してください。、 [エンコード予約ユニットの種類](http://azure.microsoft.com/blog/author/milanga)します。

予約ユニットの種類を指定するだけでなく、エンコード予約ユニットを使用して、アカウントをプロビジョニングすることも指定できます。 用意したエンコード予約ユニットの数によって、所定のアカウントで並列処理できるメディア タスクの数が決まります。 たとえば、アカウントの予約ユニットの数が 5 である場合、処理するタスクがある限り、5 個のメディア タスクが並列実行されます。 残りのタスクはキューで待機して、実行中のタスクが完了すると直ちにキューから取り出されて順番に処理されます。 アカウントに予約ユニットが用意されていない場合、タスクは逐次処理されます。 この場合、あるタスクが終了した後、次のタスクが開始するまでの待機時間は、システムのリソースが利用できるかどうかに左右されます。

>[AZURE.IMPORTANT] 占有ユニットは、Azure Media Indexer を使用してジョブのインデックスを含む、すべてのメディア処理の並列処理の動作します。  ただし、エンコーディングと異なり、インデックス作成ジョブは処理されません速く予約単位より速く。

予約ユニットの種類とエンコード予約ユニットの数を変更するには、以下の手順に従います。

1.  [Azure Classic Portal](https://manage.windowsazure.com/), 、クリックして **Media Services**します。 次に、メディア サービスの名前をクリックします。

2. 選択、 **エンコード** ページです。

    変更する、 **占有ユニットの種類**, 、BASIC、STANDARD、または PREMIUM キーを押します。

    選択した占有ユニットの種類の占有ユニットの数を変更するには、使用、 **エンコード** スライダーです。


    ![Processors page](./media/media-services-portal-encoding-units/media-services-encoding-scale.png)


    >[AZURE.NOTE] The following data centers do not offer the Premium reserved unit type: Singapore, Hong Kong, Osaka, Beijing, Shanghai.

3. [保存] ボタンを押して、変更を保存します。

    新しいエンコード予約ユニットは、[保存] をクリックするとすぐに割り当てられます。

    >[AZURE.NOTE]コストの計算時には、24 時間の期間内に指定されたユニットの最大数が使用されます。

##クォータと制限

クォータと制限をサポート チケットを開く方法については、次を参照してください。 [クォータと制限](media-services-quotas-and-limitations.md)します。



##Media Services のラーニング パス

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##フィードバックの提供

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

