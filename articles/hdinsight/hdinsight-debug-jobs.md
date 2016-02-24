<properties
    pageTitle="HDInsight で Hadoop のデバッグをする: ログの表示とエラーメッセージの解釈 | Microsoft Azure"
    description="PowerShell を使用して HDInsight を管理しているときに表示されることがあるエラー メッセージと、回復するために使用できる手順について説明します。"
    services="hdinsight"
    tags="azure-portal"
    editor="cgronlun"
    manager="paulettm"
    authors="mumian"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2015"
    ms.author="jgao"/>

# HDInsight で Hadoop のデバッグをする: ログの表示とエラーメッセージの解釈

このトピックで取り上げるエラー メッセージは、Azure HDInsight で Hadoop のユーザーが Azure PowerShell を使用してサービスを管理する際に発生する可能性のあるエラー状況を理解するのに役立ちます。また、エラーから回復する手順も示されています。

一部のエラー メッセージは、Azure ポータルで HDinsight クラスターを管理している場合にも表示されます。 しかし、その場合に遭遇する可能性のあるエラー メッセージは、その状況で可能な対応策に制約があるため、さほどきめ細かいものではありません。 その他のエラー メッセージは、対応策が明白な文脈で提供されています。 たとえば、パラメーターの制約違反である場合、値が入力されたボックスの右側にポップアップ形式でメッセージが表示されます。 下図は、要求されたデータ ノードの数が多すぎた場合です。 対応策は、許容値である 33 以下まで数を減らすことです。

![HDInsight ポータルのエラー メッセージ][image-hdi-debugging-error-messages-portal]

エラーが Azure HDInsight に固有となる状況では、そのエラーが何であるかを理解すると役に立つことがあります。 参照してください [HDInsight のエラー コード](#hdi-error-codes) 、別のエラー コード、およびそれらの修正方法を理解します。 状況によっては、Hadoop ログ自体にアクセスします。 それは Azure ポータルから直接実行できます。

## クラスターの状態とジョブ ログの表示

* **Hadoop UI アクセス**します。 Azure ポータルから、HDInsight クラスター名をクリックし、クラスター ブレードを開きます。 [クラスター] ブレードをクリックして **ダッシュ ボード**します。

    ![クラスター ダッシュボードの起動](./media/hdinsight-debug-jobs/hdi-debug-launch-dashboard.png)
  
    入力を求められたら、クラスターの管理者資格情報を入力します。 表示されたクエリ コンソールで [ **Hadoop UI**します。

    ![Hadoop UI の起動](./media/hdinsight-debug-jobs/hdi-debug-launch-dashboard-hadoop-ui.png)

* **Yarn UI にアクセス**します。 Azure ポータルから、HDInsight クラスター名をクリックし、クラスター ブレードを開きます。 [クラスター] ブレードをクリックして **ダッシュ ボード**します。 入力を求められたら、クラスターの管理者資格情報を入力します。 表示されたクエリ コンソールで [ **YARN UI**します。

    YARN UI では、次の操作を実行できます。

    * **クラスターの状態の取得**します。 左側のウィンドウから、展開 **クラスター**, 、] をクリック **に関する**します。 割り当て済みメモリの合計、使用済みコア、クラスター リソース マネージャーの状態、クラスター バージョンなど、クラスターの状態に関する詳細が表示されます。

        ![クラスター ダッシュボードの起動](./media/hdinsight-debug-jobs/hdi-debug-yarn-cluster-state.png)

    * **ノードの状態を取得**します。 左側のウィンドウから、展開 **クラスター**, 、] をクリック **ノード**します。 ここにはクラスターの全ノード、各ノードの HTTP アドレス、各ノードに割り当てられているリソースなどが一覧表示されます。

    * **ジョブの状態を監視**します。 左側のウィンドウから、展開 **クラスター**, 、] をクリックし、 **アプリケーション** をクラスター内のすべてのジョブを一覧表示します。 (新しい、送信されて実行されているなど) などの特定の状態でジョブを見て、該当するリンクをクリックする場合 **アプリケーション**します。 さらに、ジョブ名をクリックすると、出力やログなど、ジョブに関する詳細がわかります。

* **HBase UI アクセス**します。 Azure ポータルから、HDInsight HBase クラスター名をクリックし、クラスター ブレードを開きます。 [クラスター] ブレードをクリックして **ダッシュ ボード**します。 入力を求められたら、クラスターの管理者資格情報を入力します。 表示されたクエリ コンソールで [ **HBase UI**

## <a id="hdi-error-codes"></a>HDInsight のエラー コード

Azure PowerShell またはポータルでユーザーが遭遇する可能性のあるエラーを以下に名前のアルファベット順で示します。 内のエントリには、エラーはリンクされたさらに、 [説明とエラーの軽減](#discription-mitigation-errors) 次のエラーの情報を提供するセクション。

- **説明**: ユーザーが、エラー メッセージを参照してください
- **対応策**: エラーから回復する手順を実行できます。



- [AtleastOneSqlMetastoreMustBeProvided](#AtleastOneSqlMetastoreMustBeProvided)
- [AzureRegionNotSupported](#AzureRegionNotSupported)
- [ClusterContainerRecordNotFound](#ClusterContainerRecordNotFound)
- [ClusterDnsNameInvalidReservedWord](#ClusterDnsNameInvalidReservedWord)
- [ClusterNameUnavailable](#ClusterNameUnavailable)
- [ClusterUserNameInvalid](#ClusterUserNameInvalid)
- [ClusterUserNameInvalidReservedWord](#ClusterUserNameInvalidReservedWord)
- [ContainerNameMisMatchWithDnsName](#ContainerNameMisMatchWithDnsName)
- [DataNodeDefinitionNotFound](#DataNodeDefinitionNotFound)
- [DeploymentDeletionFailure](#DeploymentDeletionFailure)
- [DnsMappingNotFound](#DnsMappingNotFound)
- [DuplicateClusterContainerRequest](#DuplicateClusterContainerRequest)
- [DuplicateClusterInHostedService](#DuplicateClusterInHostedService)
- [FailureToUpdateDeploymentStatus](#FailureToUpdateDeploymentStatus)
- [HdiRestoreClusterAltered](#HdiRestoreClusterAltered)
- [HeadNodeConfigNotFound](#HeadNodeConfigNotFound)
- [HeadNodeConfigNotFound](#HeadNodeConfigNotFound)
- [HostedServiceCreationFailure](#HostedServiceCreationFailure)
- [HostedServiceHasProductionDeployment](#HostedServiceHasProductionDeployment)
- [HostedServiceNotFound](#HostedServiceNotFound)
- [HostedServiceWithNoDeployment](#HostedServiceWithNoDeployment)
- [InsufficientResourcesCores](#InsufficientResourcesCores)
- [InsufficientResourcesHostedServices](#InsufficientResourcesHostedServices)
- [InternalErrorRetryRequest](#InternalErrorRetryRequest)
- [InvalidAzureStorageLocation](#InvalidAzureStorageLocation)
- [InvalidNodeSizeForDataNode](#InvalidNodeSizeForDataNode)
- [InvalidNodeSizeForHeadNode](#InvalidNodeSizeForHeadNode)
- [InvalidRightsForDeploymentDeletion](#InvalidRightsForDeploymentDeletion)
- [InvalidStorageAccountBlobContainerName](#InvalidStorageAccountBlobContainerName)
- [InvalidStorageAccountConfigurationSecretKey](#InvalidStorageAccountConfigurationSecretKey)
- [InvalidVersionHeaderFormat](#InvalidVersionHeaderFormat)
- [MoreThanOneHeadNode](#MoreThanOneHeadNode)
- [OperationTimedOutRetryRequest](#OperationTimedOutRetryRequest)
- [ParameterNullOrEmpty](#ParameterNullOrEmpty)
- [PreClusterCreationValidationFailure](#PreClusterCreationValidationFailure)
- [RegionCapabilityNotAvailable](#RegionCapabilityNotAvailable)
- [StorageAccountNotColocated](#StorageAccountNotColocated)
- [SubscriptionIdNotActive](#SubscriptionIdNotActive)
- [SubscriptionIdNotFound](#SubscriptionIdNotFound)
- [UnableToResolveDNS](#UnableToResolveDNS)
- [UnableToVerifyLocationOfResource](#UnableToVerifyLocationOfResource)
- [VersionCapabilityNotAvailable](#VersionCapabilityNotAvailable)
- [VersionNotSupported](#VersionNotSupported)
- [VersionNotSupportedInRegion](#VersionNotSupportedInRegion)
- [WasbAccountConfigNotFound](#WasbAccountConfigNotFound)



## <a id="discription-mitigation-errors"></a>エラーの診断と対応策


### <a id="AtleastOneSqlMetastoreMustBeProvided"></a>AtleastOneSqlMetastoreMustBeProvided
- **説明**: Hive メタストアと Oozie メタストアにカスタム設定を使用するために、少なくとも 1 つのコンポーネントの Azure SQL データベースの詳細を指定してください。
- **対応策**: ユーザーが有効な SQL Azure メタストアを指定し、要求を再試行が必要です。  

### <a id="AzureRegionNotSupported"></a>AzureRegionNotSupported
- **説明**: リージョンにクラスターを作成できませんでした *nameOfYourRegion*します。 有効な HDInsight リージョンを使用して要求し直してください。
- **対応策**: お客様は現在サポートされているクラスターのリージョンを作成する必要があります: 東南アジア、西ヨーロッパ、北ヨーロッパ、米国東部、米国西部です。  

### <a id="ClusterContainerRecordNotFound"></a>ClusterContainerRecordNotFound
- **説明**: サーバーは、要求されたクラスター レコードを見つけられませんでした。  
- **対応策**: 操作をやり直してください。

### <a id="ClusterDnsNameInvalidReservedWord"></a>ClusterDnsNameInvalidReservedWord
- **説明**: クラスター DNS 名 *yourDnsName* が無効です。 名前の先頭と末尾が英数字であり、使っている特殊文字は '-' だけであることを確認してください。  
- **対応策**: 有効な DNS 名は、ダッシュ ボード以外の文字が始まり、末尾が英数字でを特別なが含まれていないクラスターを使用したことを確認 '-'、操作を再試行してください。

### <a id="ClusterNameUnavailable"></a>ClusterNameUnavailable
- **説明**: クラスター名 *yourClusterName* は使用できません。 別の名前を選択してください。  
- **対応策**: ユーザー必要があります指定して、やり直す一意であるとはありませんが存在し、再試行してください、。 ポータルを使用している場合は、作成処理中にクラスター名が既に使用されていると、その時点で通知されます。


### <a id="ClusterPasswordInvalid"></a>ClusterPasswordInvalid
- **説明**: クラスターのパスワードが無効です。 パスワードは、数字、大文字、小文字、特殊文字を少なくとも 1 文字ずつ組み合わせて、10 文字以上とし、スペースは使わず、ユーザー名を含まないようにします。  
- **対応策**: 有効なクラスター パスワードを指定して、操作を再試行します。

### <a id="ClusterUserNameInvalid"></a>ClusterUserNameInvalid
- **説明**: クラスターのユーザー名が無効です。 ユーザー名に特殊文字またはスペースが含まれていないことを確認してください。  
- **対応策**: 有効なクラスター ユーザー名を指定し、操作を再試行します。

### <a id="ClusterUserNameInvalidReservedWord"></a>ClusterUserNameInvalidReservedWord
- **説明**: クラスター DNS 名 *yourDnsClusterName* が無効です。 名前の先頭と末尾が英数字であり、使っている特殊文字は '-' だけであることを確認してください。  
- **対応策**: 有効な DNS クラスター ユーザー名を指定し、操作を再試行します。

### <a id="ContainerNameMisMatchWithDnsName"></a>ContainerNameMisMatchWithDnsName
- **説明**: URI 内のコンテナー名 *yourcontainerURI* と DNS 名の *yourDnsName* 要求の本文は、同じことがあります。  
- **対応策**: コンテナー名と DNS 名、操作を再試行して、同じことを確認します。

### <a id="DataNodeDefinitionNotFound"></a>DataNodeDefinitionNotFound
- **説明**: 無効なクラスター構成です。 データ ノード定義がノード サイズに見つかりません。  
- **対応策**: 操作をやり直してください。

### <a id="DeploymentDeletionFailure"></a>DeploymentDeletionFailure
- **説明**: クラスターのデプロイの削除に失敗しました  
- **対応策**: 削除操作をやり直します。

### <a id="DnsMappingNotFound"></a>DnsMappingNotFound
- **説明**: サービスの構成エラー。 必要な DNS マッピング情報が見つかりません。  
- **対応策**: クラスターを削除し、新しいクラスターを作成します。

### <a id="DuplicateClusterContainerRequest"></a>DuplicateClusterContainerRequest
- **説明**: クラスター コンテナー作成操作が重複しています。 レコードが存在する *nameOfYourContainer* Etag が一致しません。
- **対応策**: コンテナーの一意の名前を指定し、作成操作を再試行してください。

### <a id="DuplicateClusterInHostedService"></a>DuplicateClusterInHostedService
- **説明**: ホステッド サービス *nameOfYourHostedService* クラスターが既に含まれています。 ホストされるサービスに複数のクラスターを含めることはできません。  
- **対応策**: 別のホステッド サービスでクラスターをホストします。

### <a id="FailureToUpdateDeploymentStatus"></a>FailureToUpdateDeploymentStatus
- **説明**: サーバーは、クラスターの展開の状態を更新できませんでした。  
- **対応策**: 操作をやり直してください。 これが何度も起きる場合は、CSS にお問い合わせください。

### <a id="HdiRestoreClusterAltered"></a>HdiRestoreClusterAltered
- **説明**: クラスター *yourClusterName* メンテナンスの一環として削除されました。 クラスターを作成し直してください。
- **対応策**: クラスターを再作成します。

### <a id="HeadNodeConfigNotFound"></a>HeadNodeConfigNotFound
- **説明**: 無効なクラスター構成です。 必要なヘッド ノード構成がノード サイズに見つかりません。
- **対応策**: 操作をやり直してください。

### <a id="HostedServiceCreationFailure"></a>HostedServiceCreationFailure
- **説明**: ホステッド サービスを作成できません。 *nameOfYourHostedService*します。 要求を再試行してください。  
- **対応策**: 要求をやり直してください。

### <a id="HostedServiceHasProductionDeployment"></a>HostedServiceHasProductionDeployment
- **説明**: ホステッド サービス *nameOfYourHostedService* 既に運用環境のデプロイします。 ホストされるサービスに運用環境のデプロイを含めることはできません。 別のクラスター名を使用して要求を再試行してください。
- **対応策**: 別のクラスター名を使用し、要求を再試行します。

### <a id="HostedServiceNotFound"></a>HostedServiceNotFound
- **説明**: ホステッド サービス *nameOfYourHostedService* クラスターが見つかりませんでした。  
- **対応策**: クラスターがエラー状態の場合は、それを削除し、もう一度やり直しています。

### <a id="HostedServiceWithNoDeployment"></a>HostedServiceWithNoDeployment
- **説明**: ホステッド サービス *nameOfYourHostedService* 関連付けられたデプロイがありません。  
- **対応策**: クラスターがエラー状態の場合は、それを削除し、もう一度やり直しています。

### <a id="InsufficientResourcesCores"></a>InsufficientResourcesCores
- **説明**:「SubscriptionId *yourSubscriptionId* いないコアが残さクラスターを作成する *yourClusterName*します。 必要な: *resourcesRequired*, 、利用可能: *resourcesAvailable*します。  
- **対応策**: サブスクリプション内のリソースを解放し、サブスクリプションに使用可能なリソースを増やすか、クラスターの作成を試みるとします。

### <a id="InsufficientResourcesHostedServices"></a>InsufficientResourcesHostedServices
- **説明**: サブスクリプション ID *yourSubscriptionId* は、新しいホステッド サービスがクラスターを作成するクォータがありません *yourClusterName*します。  
- **対応策**: サブスクリプション内のリソースを解放し、サブスクリプションに使用可能なリソースを増やすか、クラスターの作成を試みるとします。

### <a id="InternalErrorRetryRequest"></a>InternalErrorRetryRequest
- **説明**: サーバーで内部エラーが発生しました。 要求を再試行してください。  
- **対応策**: 要求をやり直してください。

### <a id="InvalidAzureStorageLocation"></a>InvalidAzureStorageLocation
- **説明**: Azure ストレージの場所 *dataRegionName* 有効な場所ではありません。 リージョンが正しいことを確認し、要求を再試行してください。
- **対応策**: HDInsight をサポートする記憶域の場所を選択して、クラスターが併置されていることを確認および操作をやり直してください。

### <a id="InvalidNodeSizeForDataNode"></a>InvalidNodeSizeForDataNode
- **説明**: データ ノードの無効な VM のサイズ。 すべてのデータ ノードでサポートされているのは、'L VM' サイズのみです。  
- **対応策**: データ ノードでサポートされているノード サイズを指定し、操作を再試行します。

### <a id="InvalidNodeSizeForHeadNode"></a>InvalidNodeSizeForHeadNode
- **説明**: ヘッド ノードの無効な VM のサイズ。 ヘッド ノードでサポートされているのは 'XL VM' サイズのみです。  
- **対応策**: ヘッド ノードでサポートされているノード サイズを指定し、操作をやり直してください

### <a id="InvalidRightsForDeploymentDeletion"></a>InvalidRightsForDeploymentDeletion
- **説明**: サブスクリプション ID *yourSubscriptionId* 使用されているしてもクラスターの削除操作を実行するための十分なアクセス許可がありません *yourClusterName*します。  
- **対応策**: クラスターがエラー状態の場合は、削除して再試行します。  

### <a id="InvalidStorageAccountBlobContainerName"></a>InvalidStorageAccountBlobContainerName
- **説明**: 外部ストレージ アカウント blob コンテナー名 *yourContainerName* が無効です。 名前の先頭にアルファベットが使用され、名前には小文字、数字、およびダッシュのみが使用されていることを確認してください。  
- **対応策**: 有効なストレージ アカウントの blob コンテナー名を指定し、操作を再試行します。

### <a id="InvalidStorageAccountConfigurationSecretKey"></a>InvalidStorageAccountConfigurationSecretKey
- **説明**: 外部ストレージ アカウントの構成を *yourStorageAccountName* 秘密キーの詳細を設定するが必要です。  
- **対応策**: ストレージ アカウントの有効な秘密キーを指定し、操作を再試行します。

### <a id="InvalidVersionHeaderFormat"></a>InvalidVersionHeaderFormat
- **説明**: バージョン ヘッダー *yourVersionHeader* - yyyy-mm-dd の有効な形式ではありません  
- **対応策**: バージョン ヘッダーの有効な形式を指定して、要求を再試行します。

### <a id="MoreThanOneHeadNode"></a>MoreThanOneHeadNode
- **説明**: 無効なクラスター構成です。 ヘッド ノード構成が複数見つかりました。  
- **対応策**: 構成を編集して、ヘッド ノードが 1 つだけ指定されるようにします。

### <a id="OperationTimedOutRetryRequest"></a>OperationTimedOutRetryRequest
- **説明**: 許容時間内で、操作を完了できませんでしたまたは、最大試行回数。 要求を再試行してください。  
- **対応策**: 要求をやり直してください。

### <a id="ParameterNullOrEmpty"></a>ParameterNullOrEmpty
- **説明**: パラメーター *yourParameterName* null または空にすることはできません。  
- **対応策**: パラメーターの有効な値を指定します。

### <a id="PreClusterCreationValidationFailure"></a>PreClusterCreationValidationFailure
- **説明**: 1 つ以上のクラスターの作成要求入力が無効です。 入力値が正しいことを確認して、要求を再試行してください。  
- **対応策**: 入力値が正しい要求し直すかどうかを確認します。

### <a id="RegionCapabilityNotAvailable"></a>RegionCapabilityNotAvailable
- **説明**: 地域の利用できない *yourRegionName* およびサブスクリプション ID *yourSubscriptionId*します。  
- **対応策**: HDInsight クラスターをサポートするリージョンを指定します。 パブリックにサポートされているリージョンは、東南アジア、西ヨーロッパ、北ヨーロッパ、米国東部、米国西部です。

### <a id="StorageAccountNotColocated"></a>StorageAccountNotColocated
- **説明**: ストレージ アカウント *yourStorageAccountName* の領域が *currentRegionName*します。 クラスターのリージョンと同じである必要があります *yourClusterRegionName*します。  
- **対応策**: データが既にストレージ アカウントにいる場合は、既存のストレージ アカウントと同じリージョンに新しいクラスターを作成またはクラスターと同じリージョンにストレージ アカウントを指定します。 ポータルを使っている場合は、この問題があると、事前に通知されます。

### <a id="SubscriptionIdNotActive"></a>SubscriptionIdNotActive
- **説明**: 指定されたサブスクリプション ID *yourSubscriptionId* アクティブではありません。  
- **対応策**: サブスクリプションを再アクティブ化または新しい有効なサブスクリプションを取得します。

### <a id="SubscriptionIdNotFound"></a>SubscriptionIdNotFound
- **説明**: サブスクリプション ID *yourSubscriptionId* で見つかりませんでした。  
- **対応策**: サブスクリプション ID が有効なことを確認し、操作を再試行します。

### <a id="UnableToResolveDNS"></a>UnableToResolveDNS
- **説明**: DNS を解決するのにはできません。 *yourDnsUrl*します。 BLOB エンドポイントの完全修飾 URL が指定されていることを確認してください。  
- **対応策**: 有効な blob URL を指定します。 始まるを含め、完全に有効にする URL の必要があります *http://* 行と最終 *.com*します。

### <a id="UnableToVerifyLocationOfResource"></a>UnableToVerifyLocationOfResource
- **説明**: リソースの場所を確認できません *yourDnsUrl*します。 BLOB エンドポイントの完全修飾 URL が指定されていることを確認してください。  
- **対応策**: 有効な blob URL を指定します。 始まるを含め、完全に有効にする URL の必要があります *http://* 行と最終 *.com*します。

### <a id="VersionCapabilityNotAvailable"></a>VersionCapabilityNotAvailable
- **説明**: バージョンは利用できないバージョン能力 *specifiedVersion* およびサブスクリプション ID *yourSubscriptionId*します。  
- **対応策**: が利用可能なバージョンを選択し、操作をやり直してください。

### <a id="VersionNotSupported"></a>VersionNotSupported
- **説明**: バージョン *specifiedVersion* はサポートされていません。
- **対応策**: サポートされているバージョンを選択し、操作をやり直してください。

### <a id="VersionNotSupportedInRegion"></a>VersionNotSupportedInRegion
- **説明**: バージョン *specifiedVersion* が Azure のリージョンで利用できない *specifiedRegion*します。  
- **対応策**: 指定したリージョンでサポートされているバージョンを選択し、操作を再試行します。

### <a id="WasbAccountConfigNotFound"></a>WasbAccountConfigNotFound
- **説明**: 無効なクラスター構成です。 必要な WASB アカウント構成が外部アカウントに見つかりません。  
- **対応策**: 構成で適切に指定、アカウントが存在し、あるを確認し、操作を再試行します。

## <a id="resources"></a>その他のデバッグ リソース

* [Azure HDInsight SDK のドキュメント][hdinsight-sdk-documentation]

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[image-hdi-debugging-error-messages-portal]: ./media/hdinsight-debug-jobs/hdi-debug-errormessages-portal.png

