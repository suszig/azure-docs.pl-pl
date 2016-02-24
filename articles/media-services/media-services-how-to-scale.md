<properties 
    pageTitle="Media Services の規模の設定方法" 
    description="アカウントにプロビジョニングする [オンデマンド ストリーミング占有ユニット] および [エンコード占有ユニット] の数を指定することでメディア サービスの規模を設定する方法について説明します。" 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/05/2015"   
    ms.author="juliako"/>


#Media Services の規模の設定方法  

##概要

拡大/縮小することができます **Media Services** の数を指定することによって **ストリーミング占有ユニット** と **エンコード占有ユニット** プロビジョニングに使用するアカウントを希望します。 

ストレージ アカウントを追加して、Media Services アカウントの規模を設定することもできます。 各ストレージ アカウントの上限は 500 TB (テラバイト) です。 既定の上限を超えるストレージ容量を設定するために、複数のストレージ アカウントを単一の Media Services アカウントにアタッチすることを選択できます。

このトピックは、関連するトピックにリンクしています。

##<a id="streaming_endpoins"></a>ストリーミング占有ユニット

詳細については、次を参照してください。 [ストリーミング ユニットのスケーリング](media-services-manage-origins.md#scale_streaming_endpoints)します。

##<a id="encoding_reserved_units"></a>エンコード占有ユニット

エンコード ユニットの拡張方法については、次を参照してください。 **ポータル** と **.NET** トピックです。

[AZURE.INCLUDE [media-services-selector-scale-encoding-units](../../includes/media-services-selector-scale-encoding-units.md)]

占有ユニットは、エンコード タスクでもインデックス作成タスクでも同じです。

##<a id="storage"></a>ストレージのスケーリング

詳細については、次を参照してください。 [複数のストレージ アカウントで Media Services アセットの管理](https://msdn.microsoft.com/library/azure/dn271889.aspx) と [Azure Storage を操作](https://msdn.microsoft.com/library/azure/dn767951.aspx)します。



##Media Services のラーニング パス

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##フィードバックの提供

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

