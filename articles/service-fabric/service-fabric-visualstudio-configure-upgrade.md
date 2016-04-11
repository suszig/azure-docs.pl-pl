<properties
   pageTitle="Service Fabric アプリケーションのアップグレードの構成 | Microsoft Azure"
   description="Microsoft Visual Studio を使用して、Service Fabric アプリケーションをアップグレードするための設定を構成する方法について説明します。"
   services="service-fabric"
   documentationCenter="na"
   authors="cawaMS"
   manager="paulyuk"
   editor="tglee" />
<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="10/08/2015"
   ms.author="cawaMS" />

# Visual Studio での Service Fabric アプリケーションのアップグレードの構成

Visual Studio Service Fabric Tools は、ローカルまたはリモート クラスターへの発行のアップグレード サポートを提供します。 テストおよびデバッグ中にアプリケーションを置き換えるのではなく、アプリケーションを新しいバージョンにアップグレードすることには、2 つの利点があります。1 つは、アップグレード中にアプリケーション データが失われないことです。もう 1 つは、複数のアップグレード ドメインにわたって十分な数のサービス インスタンスが存在すれば、アップグレード中にサービスの中断が発生しないので、高可用性が実現されることです。 アプリケーションのアップグレード中に、アプリケーションに対してテストを実行できます。 

## アップグレードに必要なパラメーター

標準とアップグレードの 2 種類のデプロイメントから選択できます。 標準デプロイメントではクラスター上の以前のデプロイメント情報とデータが消去され、アップグレード デプロイメントではこれらが保持されます。 Visual Studio で Service Fabric アプリケーションをアップグレードするときには、アプリケーション アップグレード パラメーターと正常性チェック ポリシーを提供する必要があります。 アプリケーション アップグレード パラメーターによってアップグレードが制御され、正常性チェック ポリシーによってアップグレードが成功したかどうかが判断されます。 参照してください [Service Fabric アプリケーションのアップグレード: アップグレード パラメーター](service-fabric-application-upgrade-parameters.md) 詳細です。

次の 3 つのアップグレード モードがあります: *監視*, 、*UnmonitoredAuto*, 、および *手動*します。

  - A *監視* アップグレードは、アップグレードを自動化し、アプリケーションの正常性チェックします。

  - * UnmonitoredAuto* アップグレードでは、アップグレードは自動化されますが、アプリケーションの正常性チェックは省略されます。

  - 実行すると、 *手動* 各アップグレード ドメインを手動でアップグレードする必要がアップグレードされます。

各アップグレード モードには、それぞれ異なるパラメーター セットが必要です。 参照してください [アプリケーション アップグレード パラメーター](service-fabric-application-upgrade-parameters.md) を使用できるアップグレード オプションの詳細を参照してください。

## Visual Studio での Service Fabric アプリケーションのアップグレード

Service Fabric アプリケーションをアップグレードする Visual Studio Service Fabric ツールを使用している場合は、発行プロセスをチェックして、日常的な展開ではなく、アップグレードを指定できます、 **アプリケーションをアップグレード** チェック ボックスをオンします。

### アップグレード パラメーターを構成するには

1. クリックして、 **設定** チェック ボックスの横にあるボタンをクリックします。  **アップグレード パラメーターの編集** ダイアログが表示されます。  **アップグレード パラメーターの編集** ダイアログをサポートしています、 *監視*, 、*UnmonitoredAuto* と *UnmonitoredManual* モードをアップグレードします。

2. 使用するアップグレード モードを選択し、パラメーター グリッドに値を入力します。

    各パラメーターには既定値があります。 省略可能なパラメーター *DefaultServiceTypeHealthPolicy* パラメーターには、ハッシュ テーブルの入力します。 ハッシュ テーブルの入力形式の例を次に示します *DefaultServiceTypeHealthPolicy*:

    ```
    @{ ConsiderWarningAsError = "false"; MaxPercentUnhealthyDeployedApplications = 0; MaxPercentUnhealthyServices = 0; MaxPercentUnhealthyPartitionsPerService = 0; MaxPercentUnhealthyReplicasPerPartition = 0 }
    ```
    
    *ServiceTypeHealthPolicyMap* を次の形式でハッシュ テーブルの入力を受け取るもう 1 つの省略可能なパラメーターには。

    ```    
    @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"}
    ```


    Here's a real-life example:

    ```
    @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" }
    ```

3. 選択した場合 *UnmonitoredManual* アップグレード モードは、手動で続行され、アップグレード処理を終了する PowerShell コンソールを開始する必要があります。 参照してください [Service Fabric アプリケーションのアップグレード: 高度なトピック](service-fabric-application-upgrade-advanced.md) については手動によるアップグレードの動作

## PowerShell を使用したアプリケーションのアップグレード

PowerShell コマンドレットを使用して、Service Fabric アプリケーションをアップグレードできます。 参照してください [Service Fabric アプリケーションのアップグレード チュートリアル](service-fabric-application-upgrade-tutorial.md) と [Start-servicefabricapplicationupgrade](https://msdn.microsoft.com/library/mt125975.aspx) 詳細です。

## アプリケーション マニフェスト ファイルでの正常性チェック ポリシーの指定

Service Fabric アプリケーションの各サービスでは、既定値を上書きした独自の正常性ポリシー パラメーターを使用できます。 これらのパラメーター値は、アプリケーション マニフェスト ファイルで指定できます。

次の例は、アプリケーション マニフェストで、サービスごとに独自の正常性チェック ポリシーを適用する方法を示しています。

```
<Policies>
    <HealthPolicy ConsiderWarningAsError="false" MaxPercentUnhealthyDeployedApplications="20">
        <DefaultServiceTypeHealthPolicy MaxPercentUnhealthyServices="20"               
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />
        <ServiceTypeHealthPolicy ServiceTypeName="ServiceTypeName1"
                MaxPercentUnhealthyServices="20"
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />      
    </HealthPolicy>
</Policies>
```
## 次のステップ
アプリケーションの展開に関する詳細については、次を参照してください。 [Azure Service Fabric の既存のアプリケーションを展開](service-fabric-deploy-existing-app.md)します。


