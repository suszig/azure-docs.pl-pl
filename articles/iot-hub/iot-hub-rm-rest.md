<properties
    pageTitle="REST API を使用して IoT Hub を作成する | Microsoft Azure"
    description="このチュートリアルに従って、REST API を使用した IoT Hub の作成を開始できます。"
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="11/23/2015"
     ms.author="dobett"/>

# チュートリアル: C# プログラムを使って IoT Hub を作成する

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## はじめに

使用することができます、 [IoT Hub リソース プロバイダーの REST API][lnk-rest-api] を作成し、Azure の IoT hub をプログラムで管理します。 このチュートリアルでは、Resource Provider REST API を使用して C# プログラムから IoT Hub を作成する方法を説明します。

このチュートリアルを完了するには、以下が必要になります。

- Microsoft Visual Studio 2015
- アクティブな Azure アカウント <br/>アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、[Azure の無料試用版サイト][lnk-free-trial]をご覧ください。
- [Microsoft Azure PowerShell 1.0][lnk-powershell-install] またはそれ以降。

[AZURE.INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## Visual Studio プロジェクトの準備

1. Visual Studio で、作成、新しい Visual c# プロジェクトを使用して Windows、 **コンソール アプリケーション** プロジェクト テンプレートです。 プロジェクトに名前を **CreateIoTHubREST**します。

2. ソリューション エクスプ ローラーで、プロジェクトを右クリックし、をクリックして **NuGet パッケージの管理**します。

3. NuGet パッケージ マネージャーで確認 **プレリリースを含める** 探します **Microsoft.Azure.Management.Resources**します。 Select バージョン **2.18.11-preview**します。 をクリックして **インストール**, で、 **の変更の確認** ] をクリックして **[ok]**, 、順にクリックして **同意** ライセンス条項に同意します。

4. NuGet パッケージ マネージャーで、検索 **Microsoft.IdentityModel.Clients.ActiveDirectory**します。 Select バージョン **2.19.208020213**します。 をクリックして **インストール**, で、 **の変更の確認** ] をクリックして **[ok]**, 、順にクリックして **同意** ライセンスに同意します。

5. NuGet パッケージ マネージャーで、検索 **Microsoft.Azure.Common**します。 Select バージョン **2.1.0**します。 をクリックして **インストール**, で、 **の変更の確認** ] をクリックして **[ok]**, 、順にクリックして **同意** ライセンス条項に同意します。

6. Program.cs で置き換える既存 **を使用して** を次のステートメント。

    ```
    using System;
    using System.IO;
    using System.Net;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using Microsoft.Azure;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.Resources.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Newtonsoft.Json;
    ```
    
7. Program.cs で、次の静的変数を追加して、プレースホルダーの値を置き換えます。 メモした **ApplicationId**, 、**SubscriptionId**, 、**TenantId**, 、および **パスワード** このチュートリアルでは以前です。 **リソース グループ名** 名前は、IoT hub を作成するときに使用するリソース グループのことができます、既存のリソース グループまたは新しい 1 つです。 **IoT Hub 名** など、作成、IoT Hub の名前を指定 **MyIoTHub**します。 **展開名** など、デプロイの名前では **Deployment_01**します。

    ```
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    
    static string rgName = "{Resource group name}";
    static string iotHubName = "{IoT Hub name}";
    static string deploymentName = "{Deployment name}";
    ```

[AZURE.INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## REST API を使用して IoT Hub を作成する

IoT Hub Resource Provider REST API を使用して、リソース グループで新しい IoT Hub を作成します。 使用することも、 [REST API][lnk-rest-api] 既存 IoT hub を変更します。

1. 次のメソッドを Program.cs に追加します。
    
    ```
    static bool CreateIoTHub(ResourceManagementClient client, string token)
    {
        
    }
    ```

2. 次のコードを追加、 **CreateIoTHub** 要求に認証トークンを追加します。

    ```
    client.HttpClient.DefaultRequestHeaders.Authorization = 
      new AuthenticationHeaderValue("Bearer", token);
    ```

3. 次のコードを追加、 **CreateIoTHub** メソッドを作成し、JSON 表現を生成する IoT hub を説明します。

    ```
    var description = new
    {
      name = iotHubName,
      location = "East US",
      sku = new
      {
        name = "S1",
        tier = "Standard",
        capacity = 1
      }
    };
    
    var content = new StringContent(
      JsonConvert.SerializeObject(description),
      Encoding.UTF8, "application/json");
    ```

4. 次のコードを追加、 **CreateIoTHub** REST 要求を Azure に送信し、応答を検査します。

    ```
    var requestUri = string.Format("https://management.azure.com/subscriptions/{0}/resourcegroups/{1}/providers/Microsoft.devices/IotHubs/{2}?api-version=2015-08-15-preview",
      subscriptionId, rgName, iotHubName);
    var httpsRepsonse = client.HttpClient.PutAsync(
      requestUri, content).Result;

    if (!httpsRepsonse.IsSuccessStatusCode)
    {
      Console.WriteLine("Failed {0}", httpsRepsonse.Content.ReadAsStringAsync().Result);
      return false;
    }
    ```

5. 末尾に次のコードを追加、 **CreateIoTHub** 配置を完了するまで待機します。

    ```
    ResourceGetResult resourceGetResult = null;
    do
    {
    resourceGetResult = client.Resources.GetAsync(
      rgName,
      new ResourceIdentity()
      {
        ResourceName = iotHubName,
        ResourceProviderApiVersion = "2015-08-15-preview",
        ResourceProviderNamespace = "Microsoft.Devices",
        ResourceType = "IotHubs"
      }).Result;
    Console.WriteLine("IoTHub state {0}", 
      resourceGetResult.Resource.ProvisioningState);
    } while (resourceGetResult.Resource.ProvisioningState != "Succeeded" 
          && resourceGetResult.Resource.ProvisioningState != "Failed");
    
    if (resourceGetResult.Resource.ProvisioningState != "Succeeded")
    {
        Console.WriteLine("Failed to create iothub");
        return false;
    }
    return true;
    ```

[AZURE.INCLUDE [iot-hub-retrieve-keys](../../includes/iot-hub-retrieve-keys.md)]

## アプリケーションの完了と実行

ここで呼び出すことによって、アプリケーションを完了できます、 **CreateIoTHub** と **ShowIoTHubKeys** メソッドをビルドして実行する前にします。

1. 末尾に次のコードを追加、 **Main** メソッド。

    ```
    if (CreateIoTHub(client, token.AccessToken))
        ShowIoTHubKeys(client, token.AccessToken);
    Console.ReadLine();
    ```
    
2. クリックして **ビルド** し **ソリューションをビルド**します。 すべてのエラーを修正します。

3. をクリックして **デバッグ** し **[デバッグ開始]** アプリケーションを実行します。 デプロイメントが実行されるまでに数分かかる場合があります。

4. アクセスして、アプリケーションに新しい IoT hub が追加されたことを確認できる、 [ポータル][lnk-azure-portal] またはを使用して、リソースの一覧を表示して、 **Get AzureRmResource** PowerShell コマンドレット。

> [AZURE.NOTE] このアプリケーションの例では、課金を S1 の標準的な IoT Hub を追加します。 を通じて IoT hub を削除する、 [ポータル][lnk-azure-portal] またはを使用して、 **削除 AzureRmResource** が完了したら、PowerShell コマンドレット。

## 次のステップ

- 機能の紹介、 [IoT Hub リソース プロバイダーの REST API][lnk-rest-api]します。
- 読み取り [Azure リソース マネージャーの概要][lnk-azure-rm-overview] を Azure リソース マネージャーの機能の詳細について参照してください。

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-powershell-install]: https://azure.microsoft.com/en-us/blog/azps-1-0-pre/
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-azure-rm-overview]: https://azure.microsoft.com/documentation/articles/resource-group-overview/

