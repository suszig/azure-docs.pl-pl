<properties
    pageTitle="cURL と Azure REST API を使用した HDInsight 用の Linux ベースの Hadoop、HBase、または Storm クラスターの作成 | Microsoft Azure"
    description="cURL、Azure リソース マネージャー テンプレート、および Azure REST API を使用して Linux ベースの HDInsight クラスターを作成する方法について説明します。クラスターの種類 (Hadoop、HBase、または Storm) を指定するか、スクリプトを使用してカスタム コンポーネントをインストールすることができます。"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="paulettm"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="12/04/2015"
    ms.author="larryfr"/>


# cURL と Azure REST API を使用して HDInsight に Linux ベースのクラスターを作成する

[AZURE.INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Azure REST API を使用すると、Azure プラットフォームでホストされたサービスで、Linux ベースの HDInsight クラスターなど新しいリソースの作成を含む管理操作を実行できます。 このドキュメントでは、Azure リソース マネージャー テンプレートを作成し、HDInsight クラスターと関連するストレージを構成してから、cURL を使用してテンプレートを Azure REST API にデプロイして新しい HDInsight クラスターを作成する方法を学びます。
> [AZURE.IMPORTANT] この文書の手順では、HDInsight クラスターにワーカー ノードの既定数 (4) を使用します。 クラスター作成または作成後の拡大で 32 以上の worker ノードを予定している場合、コア数が 8 個以上で RAM が 14GB 以上のサイズのヘッド ノードを選択する必要があります。
>
> ノードのサイズとそれに伴うコストに関する詳細については、次を参照してください。 [HDInsight 料金](https://azure.microsoft.com/pricing/details/hdinsight/)します。

## 前提条件

- **Azure サブスクリプション**。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。

- __Azure CLI__。 Azure CLI を使用してサービス プリンシパルを作成し、Azure REST API に対する要求用の認証トークンを生成します。

    CLI のインストール方法の詳細については、次を参照してください。 [Azure CLI をインストール](../xplat-cli-install.md)します。

- __cURL__。 このユーティリティは、パッケージ管理システムからは入手またはからダウンロードできます [http://curl.haxx.se/](http://curl.haxx.se/)します。
    > [AZURE.NOTE] 最初に削除する必要がありますこのドキュメントでコマンドを実行する PowerShell を使用している場合、 `curl` 既定で作成される別名です。 このエイリアスは、cURL を使用する場合ではなく Invoke-webrequest、PowerShell コマンドレットを使用して、 `curl` PowerShell プロンプトでコマンドをこのドキュメントで使用されるコマンドの多くのエラーが返されます。
    > 
    > このエイリアスを削除するには、PowerShell プロンプトから以下を使用します。
    >
    > `' Remove-item エイリアス: curl`
    >
    > エイリアスが削除されると、システムにインストールした cURL のバージョンを使用できるようになります。

## テンプレートの作成

Azure リソース管理テンプレートは、__リソース グループ__とその中のすべてのリソース (HDInsight など) について記述する JSON ドキュメントです。 このテンプレート基準の手法では、1 つのテンプレートで HDInsight に必要なすべてのリソースを定義でき、グループに変更を適用する__デプロイメント__を介して、グループに対する変更をまとめて管理できます。

通常、テンプレートは 2 つの部分 (テンプレート自体と、自身の構成に固有の値を読み込んだパラメーター ファイル) から成ります。 たとえば、クラスター名、管理者名、パスワードなどです。 直接 REST API を使用する場合、これらを 1 つのファイルにまとめる必要があります。 この JSON ドキュメントの形式は次のようになります。

    {
        "properties": {
            "template": {
                contents of template file
            },
            "mode": "deploymentmode",
            "Parameters": {
                contents of the parameters element from parameters file
            }
        }
    }

たとえば、次に示しますからテンプレートおよびパラメーター ファイルの合併 [https://github.com/Azure/azure-quickstart-templates/tree/master/hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/hdinsight-linux-ssh-password), 、SSH ユーザー アカウントをセキュリティで保護するパスワードを使用して Linux ベースのクラスターを作成します。

    {
        "properties": {
            "template": {
                "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                "contentVersion": "1.0.0.0",
                "parameters": {
                    "location": {
                        "type": "string",
                        "allowedValues": ["Central US",
                        "East Asia",
                        "East US",
                        "Japan East",
                        "Japan West",
                        "North Europe",
                        "South Central US",
                        "Southeast Asia",
                        "West Europe",
                        "West US"],
                        "metadata": {
                            "description": "The location where all azure resources will be deployed."
                        }
                    },
                    "clusterType": {
                        "type": "string",
                        "allowedValues": ["hadoop",
                        "hbase",
                        "storm",
                        "spark"],
                        "metadata": {
                            "description": "The type of the HDInsight cluster to create."
                        }
                    },
                    "clusterName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the HDInsight cluster to create."
                        }
                    },
                    "clusterLoginUserName": {
                        "type": "string",
                        "metadata": {
                            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
                        }
                    },
                    "clusterLoginPassword": {
                        "type": "securestring",
                        "metadata": {
                            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                        }
                    },
                    "sshUserName": {
                        "type": "string",
                        "metadata": {
                            "description": "These credentials can be used to remotely access the cluster."
                        }
                    },
                    "sshPassword": {
                        "type": "securestring",
                        "metadata": {
                            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                        }
                    },
                    "clusterStorageAccountName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the storage account to be created and be used as the cluster's storage."
                        }
                    },
                    "clusterWorkerNodeCount": {
                        "type": "int",
                        "defaultValue": 4,
                        "metadata": {
                            "description": "The number of nodes in the HDInsight cluster."
                        }
                    }
                },
                "variables": {
                    "defaultApiVersion": "2015-05-01-preview",
                    "clusterApiVersion": "2015-03-01-preview"
                },
                "resources": [{
                    "name": "[parameters('clusterStorageAccountName')]",
                    "type": "Microsoft.Storage/storageAccounts",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('defaultApiVersion')]",
                    "dependsOn": [],
                    "tags": {
    
                    },
                    "properties": {
                        "accountType": "Standard_LRS"
                    }
                },
                {
                    "name": "[parameters('clusterName')]",
                    "type": "Microsoft.HDInsight/clusters",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('clusterApiVersion')]",
                    "dependsOn": ["[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"],
                    "tags": {
    
                    },
                    "properties": {
                        "clusterVersion": "3.2",
                        "osType": "Linux",
                        "clusterDefinition": {
                            "kind": "[parameters('clusterType')]",
                            "configurations": {
                                "gateway": {
                                    "restAuthCredential.isEnabled": true,
                                    "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                                    "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                                }
                            }
                        },
                        "storageProfile": {
                            "storageaccounts": [{
                                "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                                "isDefault": true,
                                "container": "[parameters('clusterName')]",
                                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), variables('defaultApiVersion')).key1]"
                            }]
                        },
                        "computeProfile": {
                            "roles": [{
                                "name": "headnode",
                                "targetInstanceCount": "2",
                                "hardwareProfile": {
                                    "vmSize": "Standard_D3"
                                },
                                "osProfile": {
                                    "linuxOperatingSystemProfile": {
                                        "username": "[parameters('sshUserName')]",
                                        "password": "[parameters('sshPassword')]"
                                    }
                                }
                            },
                            {
                                "name": "workernode",
                                "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                                "hardwareProfile": {
                                    "vmSize": "Standard_D3"
                                },
                                "osProfile": {
                                    "linuxOperatingSystemProfile": {
                                        "username": "[parameters('sshUserName')]",
                                        "password": "[parameters('sshPassword')]"
                                    }
                                }
                            }]
                        }
                    }
                }],
                "outputs": {
                    "cluster": {
                        "type": "object",
                        "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                    }
                }
            },
            "mode": "incremental",
            "Parameters": {
                "location": {
                    "value": "North Europe"
                },
                "clusterName": {
                    "value": "newclustername"
                },
                "clusterType": {
                    "value": "hadoop"
                },
                "clusterStorageAccountName": {
                    "value": "newstoragename"
                },
                "clusterLoginUserName": {
                    "value": "admin"
                },
                "clusterLoginPassword": {
                    "value": "changeme"
                },
                "sshUserName": {
                    "value": "sshuser"
                },
                "sshPassword": {
                    "value": "changeme"
                }
            }
        }
    }

このサンプルをこのドキュメントの手順で使用します。 ドキュメント末尾の __Parameters__ セクションのプレースホルダーの_値_を、クラスターに使用する値に置き換える必要があります。

## Azure サブスクリプションへのログイン

記載されている手順に従います [Azure サブスクリプションへの接続から、Azure コマンド ライン インターフェイス (Azure CLI)](../xplat-cli-connect.md) を使用して、サブスクリプションに接続し、 __ログイン__ メソッドです。

## サービス プリンシパルの作成

> [AZURE.IMPORTANT] 以下にリンクされた記事の手順に従う場合、次の変更を行う必要があります。
> 
> * 値を使用する手順がと答えて __リーダー__, 、代わりに使用する必要があります __所有者__します。 これにより、サブスクリプションのサービスに変更を追加できるサービス プリンシパルが作成されます。これは、HDInsight クラスターの作成に必要です。
>
> また、このプロセスで使用する次の情報を保存する必要もあります。
> 
> * 使用する場合、サブスクリプション ID] – が受信した `azure アカウント一覧`
> * テナント ID に使用するときに受け取った `azure アカウント一覧`
> * アプリケーション ID - サービス プリンシパルを作成するときに返される
> * サービス プリンシパルを作成するときに、- サービス プリンシパルのパスワードが使用されます。

_Authenticate のサービス プリンシパルをパスワードでの Azure CLI_ セクションの手順に従って、 [Azure リソース マネージャーでサービス プリンシパルの認証](https://azure.microsoft.com/documentation/articles/resource-group-authenticate-service-principal/#authenticate-service-principal-with-password---azure-cli) ドキュメントです。 これにより、クラスター作成要求の認証に使用できる新しいサービス プリンシパルが作成されます。

## 認証トークンの取得

以下を使用して Azure から新しいトークンを取得します。 __TENANTID__、__APPLICATIONID__、__PASSWORD__ を、サービス プリンシパルの作成時に保存した情報に置き換えます。

    curl -X "POST" "https://login.microsoftonline.com/TENANTID/oauth2/token" \
    -H "Cookie: flight-uxoptin=true; stsservicecookie=ests; x-ms-gateway-slice=productionb; stsservicecookie=ests" \
    -H "Content-Type: application/x-www-form-urlencoded" \
    --data-urlencode "client_id=APPLICATIONID" \
    --data-urlencode "grant_type=client_credentials" \
    --data-urlencode "client_secret=PASSWORD" \
    --data-urlencode "resource=https://management.azure.com/"

この要求が成功したら、200 シリーズの応答が届きます。応答本文に JSON ドキュメントが含まれています。
> [AZURE.IMPORTANT] この要求によって返される JSON ドキュメントには __access_token__ という名前の要素が含まれます。この要素の値はアクセス トークンで、これは本ドキュメントの次のセクションで使用される要求の認証に使用する必要があります。

## リソース グループの作成

次を利用して新しいリソース グループを作成します。 リソース (HDInsight クラスターなど) を作成するには、まずグループを作成する必要があります。

* __SUBSCRIPTIONID__ をサービス プリンシパルの作成時に受信するサブスクリプション ID に置き換えます。
* __ACCESSTOKEN__ を前の手順で受け取ったアクセス トークンに置き換えます。
* __DATACENTERLOCATION__ をリソース グループやリソースを作成するデータ センターに置き換えます。 たとえば、"South Central US" のようになります。
* __GROUPNAME__ を、このグループに使用する名前に置き換えます。

    curl-x"PUT""https://management.azure.com/subscriptions/SUBSCRIPTIONID/resourcegroups/GROUPNAME?api-version=2015-01-01"\
        -H"承認: ベアラー ACCESSTOKEN"\
        -H"コンテンツの種類: アプリケーション/json"\
        -d $'{
    "location":"DATACENTERLOCATION"
    }’

この要求が成功したら、200 シリーズの応答が届きます。応答本文に含まれる JSON ドキュメントにグループに関する情報が含まれています。  `"ProvisioningState"` 要素の値が格納されます `「成功」`します。

## デプロイの作成

以下を使用して、クラスター構成 (テンプレートとパラメーター値) をリソース グループにデプロイします。

* __SUBSCRIPTIONID__ と __ACCESSTOKEN__ を前に使用した値に置き換えます。
* __GROUPNAME__ を前のセクションで作成したリソース グループの名前に置き換えます。
* __DEPLOYMENTNAME__ を、このデプロイに使用する名前に置き換えます。

    curl-x"PUT""https://management.azure.com/subscriptions/SUBSCRIPTIONID/resourcegroups/GROUPNAME/providers/microsoft.resources/deployments/DEPLOYMENTNAME?api-version=2015-01-01"\
    -H"承認: ベアラー ACCESSTOKEN"\
    -H"コンテンツの種類: アプリケーション/json"\
    次元の"{set 本文文字列テンプレートとパラメーターは、}"

> [AZURE.NOTE] テンプレートとパラメーターを含む JSON ドキュメントをファイルに保存した場合、`-d "{テンプレートとパラメーター}"' の代わりに以下を使用できます。
>
> `-データ バイナリ"@/path/to/file.json"`

この要求が成功したら、200 シリーズの応答が届きます。応答本文に含まれる JSON ドキュメントにデプロイ操作に関する情報が含まれています。
> [AZURE.IMPORTANT] デプロイは送信済みですが、この時点では完了していないことに注意してください。 デプロイを完了するには時間がかかる場合があります (通常約 15 分)。

## デプロイの状態の確認

デプロイの状態を確認するには、次のコマンドを使用します。

* __SUBSCRIPTIONID__ と __ACCESSTOKEN__ を前に使用した値に置き換えます。
* __GROUPNAME__ を前のセクションで作成したリソース グループの名前に置き換えます。

    curl-x"GET""https://management.azure.com/subscriptions/SUBSCRIPTIONID/resourcegroups/GROUPNAME/providers/microsoft.resources/deployments/DEPLOYMENTNAME?api-version=2015-01-01"\
    -H"承認: ベアラー ACCESSTOKEN"\
    -H"コンテンツの種類: アプリケーション/json"

これにより返される JSON ドキュメントにデプロイ操作に関する情報が含まれます。  `"ProvisioningState"` これの値が含まれている場合は、要素には、展開の状態が格納されます `「成功」`, 、展開が正常に完了しません。 この時点で、クラスターは使用可能になっています。

## 次のステップ

HDInsight クラスターが正常に作成されました。次に、クラスターの使用方法について、以下のトピックをご覧ください。

### Hadoop クラスター

* [HDInsight での Hive を使用します。](hdinsight-use-hive.md)
* [HDInsight での Pig を使用します。](hdinsight-use-pig.md)
* [HDInsight での MapReduce を使用します。](hdinsight-use-mapreduce.md)

### HBase クラスター

* [HDInsight での HBase を使用します。](hdinsight-hbase-tutorial-get-started-linux.md)
* [HDInsight での HBase の Java アプリケーションを開発します。](hdinsight-hbase-build-java-maven-linux.md)

### Storm クラスター

* [HDInsight で Storm の Java トポロジを開発します。](hdinsight-storm-develop-java-topology.md)
* [HDInsight での Storm で Python コンポーネントを使用します。](hdinsight-storm-develop-python-topology.md)
* [展開および HDInsight での Storm トポロジの監視](hdinsight-storm-deploy-monitor-topology-linux.md)





