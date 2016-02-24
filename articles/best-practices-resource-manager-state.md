<properties
    pageTitle="Azure リソース マネージャーのテンプレートで状態を処理するためのベスト プラクティス"
    description="複合オブジェクトを使用して Azure リソース マネージャー テンプレートやリンクされたテンプレートと状態データを共有する推奨方法を示します。"
    services="azure-resource-manager"
    documentationCenter=""
    authors="mmercuri"
    manager="georgem"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/10/2015"
    ms.author="mmercuri"/>

# Azure リソース マネージャーのテンプレートでの状態の共有

このトピックでは、Azure リソース マネージャー テンプレート内およびリンクされたテンプレート間で状態を管理して共有するためのベスト プラクティスを示します。 このトピックで使用するパラメーターと変数は、デプロイ要件を適切に整理するために定義できる種類のオブジェクトの例を示しています。 これらの例から、使用環境で意味のあるプロパティ値を使用する独自のオブジェクトを実装できます。

このトピックは大型のホワイト ペーパーの一部です。 フルのペーパーを参照するには、ダウンロード [世界クラスの ARM テンプレートの考慮事項と実証済みプラクティス] (http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World クラス ARM テンプレートの考慮事項と実証済みの Practices.pdf)。


## 複合オブジェクトを使用した、状態の共有

高い柔軟性と多様性に対応したテンプレートというのは、決して一般的ではありません。一般的なパターンはむしろ既製の構成を選べるようにすることであり、事実、T シャツのような標準的なサイズ区分 (サンドボックス、S、M、L など) が多く使われています。 コミュニティ エディション、エンタープライズ エディションなど製品の提供形態も、T シャツのサイズと同様の区分といえます。 また、MapReduce、NoSQL など、テクノロジのワークロードに基づく構成で大中小を決める場合もあります。

複合オブジェクトを使用すると、データのコレクションを格納する変数 ("プロパティ バッグ" と呼ばれます) を作成し、そのデータを使用して、テンプレートでのリソースの宣言をしやすくすることができます。 このアプローチの特徴は、さまざまなサイズの構成を顧客向けにあらかじめ用意しておき、良質な既製の構成を提供できることです。 既製の構成が用意されていなければ、エンド カスタマー自身がクラスターの規模を決め、プラットフォーム リソースの制約を加味しながら、ストレージ アカウントや各種リソースの分割サイズを計算しなければなりません (クラスター サイズとリソースの制約のため)。 既製の構成を用いることによって、"T シャツの適切なサイズ"、つまり特定のデプロイを顧客が選びやすくなります。 顧客の利便性を高めることとは別に、既製の構成の数は少ない方がサポートしやすく、密集度も高めやすくなります。


次の例は、データのコレクションを表す複合オブジェクトを含む変数を定義する方法を示します。 コレクション値仮想マシンのサイズに使用されるネットワーク設定を定義します。 
オペレーティング システムの設定と可用性の設定です。

    "variables": {
      "tshirtSizeLarge": {
        "vmSize": "Standard_A4",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-16disk-resources.json')]",
        "vmCount": 3,
        "slaveCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1,1],
          "jumpbox": 0
        }
      },
      "osSettings": {
        "scripts": [
          "[concat(variables('templateBaseUrl'), 'install_postgresql.sh')]",
          "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/shared_scripts/ubuntu/vm-disk-utils-0.1.sh"
        ],
        "imageReference": {
      "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "14.04.2-LTS",
          "version": "latest"
        }
      },
      "networkSettings": {
        "vnetName": "[parameters('virtualNetworkName')]",
        "addressPrefix": "10.0.0.0/16",
        "subnets": {
          "dmz": {
            "name": "dmz",
            "prefix": "10.0.0.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          },
          "data": {
            "name": "data",
            "prefix": "10.0.1.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          }
        }
      },
      "availabilitySetSettings": {
        "name": "pgsqlAvailabilitySet",
        "fdCount": 3,
        "udCount": 5
      }
    }

テンプレートで、後でこれらの変数を参照できます。 名前付きの変数とそのプロパティを参照することは、テンプレートの構文を簡略化します。 
コンテキストを理解しやすくなります。 次の例では、上記のオブジェクトを使用して値を設定することで、デプロイするリソースを定義します。 たとえば、VM のサイズが値を取得して設定されていることに注意してください。 
 `variables('tshirtSize').vmSize` から、ディスクのサイズを取得するために、値を中に `variables('tshirtSize').diskSize`します。 さらに、リンクされたテンプレートの URI に設定します 
場合は値 `variables('tshirtSize').vmTemplate`です。

    "name": "master-node",
    "type": "Microsoft.Resources/deployments",
    "apiVersion": "2015-01-01",
    "dependsOn": [
        "[concat('Microsoft.Resources/deployments/', 'shared')]"
    ],
    "properties": {
        "mode": "Incremental",
        "templateLink": {
          "uri": "[variables('tshirtSize').vmTemplate]",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "value": "[parameters('adminPassword')]"
          },
          "replicatorPassword": {
            "value": "[parameters('replicatorPassword')]"
          },
          "osSettings": {
        "value": "[variables('osSettings')]"
          },
          "subnet": {
            "value": "[variables('networkSettings').subnets.data]"
          },
          "commonSettings": {
            "value": {
              "region": "[parameters('region')]",
              "adminUsername": "[parameters('adminUsername')]",
              "namespace": "ms"
            }
          },
          "storageSettings": {
            "value":"[variables('tshirtSize').storage]"
          },
          "machineSettings": {
            "value": {
              "vmSize": "[variables('tshirtSize').vmSize]",
              "diskSize": "[variables('tshirtSize').diskSize]",
              "vmCount": 1,
              "availabilitySet": "[variables('availabilitySetSettings').name]"
            }
          },
          "masterIpAddress": {
            "value": "0"
          },
          "dbType": {
            "value": "MASTER"
          }
        }
      }
    }

## テンプレートおよびリンクされたテンプレートに状態を渡す

次を使用して、テンプレートおよびリンクされたテンプレートと状態情報を共有できます。

- デプロイ中に、メイン テンプレートに直接渡されるパラメーター
- メイン テンプレートが、リンクされたテンプレートと共有するパラメーター、静的変数、生成された変数

### メイン テンプレートに渡される共通パラメーター

次の表では、テンプレートで一般的に使用されるパラメーターを一覧にしています。

**メイン テンプレートに渡される、一般的に使用されるパラメーター**

名前 | 値 | 説明
---- | ----- | -----------
location    | Azure リージョンの制約付き一覧の文字列   | リソースがデプロイされる場所です。
storageAccountNamePrefix    | String    | VM のディスクが配置されるストレージ アカウントの一意の DNS 名
domainName  | String    | 形式でパブリックにアクセスできる jumpbox VM のドメイン名: **{domainname} {。{location}.cloudapp.com** 例: **mydomainname.westus.cloudapp.azure.com**
adminUsername   | String    | VM のユーザー名
adminPassword   | String    | VM のパスワード
tshirtSize  | 提供される T シャツ サイズの制約付き一覧の文字列   | プロビジョニングする名前付きのスケール ユニットのサイズ。 たとえば、"Small"、"Medium"、"Large"
virtualNetworkName  | String    | コンシューマーが使用する仮想ネットワークの名前。
enableJumpbox   | 制約付き一覧の文字列 (enabled/disabled) | 環境の jumpbox を有効にするかどうかを識別するパラメーター。 値: "enabled"、"disabled"

### リンクされたテンプレートに送信されるパラメーター

リンクされたテンプレートへの接続時には、多くの場合、静的変数と生成された変数を組み合わせて使用します。

#### 静的変数

静的変数は、URL など、基本の値を指定するためによく使用されます。このような基本の値は、テンプレート全体で、または動的な変数の値を作成するための値として使用されます。

次のテンプレートの抜粋で *templateBaseUrl* GitHub におけるテンプレートのルートの場所を指定します。 次の行では、新しい変数 *sharedTemplateUrl* を連結する、 
値 *templateBaseUrl* 共有リソース テンプレートの既知の名前に置き換えます。 T シャツのサイズを格納する複合オブジェクト変数を使用するその下に、ここで、 *templateBaseUrl* は 
格納されている既知の構成テンプレートの場所を指定する連結、 *vmTemplate* プロパティです。

このアプローチの利点は、新しいテンプレートの基礎として簡単にテンプレートを移動、分岐、使用できることです。 のみ静的変数を変更する必要がある場合は、テンプレートの場所が変更されました。 
1 か所で — メイン テンプレート-テンプレート全体を渡します。

    "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
    "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
    "tshirtSizeSmall": {
      "vmSize": "Standard_A1",
      "diskSize": 1023,
      "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
      "vmCount": 2,
      "slaveCount": 1,
      "storage": {
        "name": "[parameters('storageAccountNamePrefix')]",
        "count": 1,
        "pool": "db",
        "map": [0,0],
        "jumpbox": 0
      }
    }

#### 生成された変数

静的変数に加えて、いくつかの変数が動的に生成されます。 このセクションでは、生成された変数の一般的な種類を明確にします。

##### tshirtSize

メイン テンプレートを呼び出す場合は、通常などの値が含まれているオプションの固定された数から t シャツのサイズを選択できる *小さな*, 、*中*, と *大*します。

メイン テンプレートでは、このオプションが表示されます、パラメーターとしてなど *tshirtSize*:

    "tshirtSize": {
      "type": "string",
      "defaultValue": "Small",
      "allowedValues": [
        "Small",
        "Medium",
        "Large"
      ],
      "metadata": {
        "Description": "T-shirt size of the MongoDB deployment"
      }
    }

メイン テンプレート内では、変数は、それぞれのサイズに対応します。 たとえば、利用可能なサイズは、小規模、中規模および大規模には、変数セクション変数が含まれます 
名前付き *tshirtSizeSmall*, 、*tshirtSizeMedium*, 、および *tshirtSizeLarge*します。

次の例に示すように、これらの変数には、特定の T シャツ サイズのプロパティを定義します。 VM の種類、ディスクのサイズ、関連付けられたスケール ユニット リソースのテンプレートを識別する各 
リンク、インスタンス、ストレージ アカウントの詳細、および jumpbox の状態の数。

ストレージ アカウント名のプレフィックスは、ユーザーが指定したパラメーターから取得され、リンクされたテンプレートは、テンプレートのベース URL と特定のスケール ユニット リソースのテンプレートのファイル名を連結したものです。

    "tshirtSizeSmall": {
      "vmSize": "Standard_A1",
            "diskSize": 1023,
      "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
      "vmCount": 2,
      "storage": {
        "name": "[parameters('storageAccountNamePrefix')]",
        "count": 1,
        "pool": "db",
        "map": [0,0],
        "jumpbox": 0
      }
    },
    "tshirtSizeMedium": {
      "vmSize": "Standard_A3",
      "diskSize": 1023,
      "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-8disk-resources.json')]",
      "vmCount": 2,
      "storage": {
        "name": "[parameters('storageAccountNamePrefix')]",
        "count": 2,
        "pool": "db",
        "map": [0,1],
        "jumpbox": 0
      }
    },
    "tshirtSizeLarge": {
      "vmSize": "Standard_A4",
      "diskSize": 1023,
      "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-16disk-resources.json')]",
      "vmCount": 3,
      "storage": {
        "name": "[parameters('storageAccountNamePrefix')]",
        "count": 2,
        "pool": "db",
        "map": [0,1,1],
        "jumpbox": 0
      }
    }

 *TshirtSize* 変数のセクションで変数が詳しく表示されます。 提供する、t シャツの終了サイズ (*小さな*, 、*中*, 、*大*) は、テキストと連結され *tshirtSize* を取得する、 
その t シャツ サイズに関連付けられた複合オブジェクト変数:

    "tshirtSize": "[variables(concat('tshirtSize', parameters('tshirtSize')))]",

この変数は、リンクされているスケール ユニット リソースのテンプレートに渡されます。

##### networkSettings

容量、機能、またはエンド ツー エンドのスコープを持つソリューション テンプレートでは、リンクされたテンプレートは通常、ネットワーク上に存在するリソースを作成します。 1 つのわかりやすい方法としては、格納する複合オブジェクトを使用するには 
ネットワークの設定と、リンクされたテンプレートに渡したりします。

以下にネットワーク設定をやり取りする例を示します。

    "networkSettings": {
      "vnetName": "[parameters('virtualNetworkName')]",
      "addressPrefix": "10.0.0.0/16",
      "subnets": {
        "dmz": {
          "name": "dmz",
          "prefix": "10.0.0.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        },
        "data": {
          "name": "data",
          "prefix": "10.0.1.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        }
      }
    }

##### availabilitySettings

リンクされたテンプレートで作成されたリソースは、多くの場合、可用性セットに配置されます。 次の例では、可用性セット名と共に、使用する障害ドメインと更新ドメインの数も指定しています。

    "availabilitySetSettings": {
      "name": "pgsqlAvailabilitySet",
      "fdCount": 3,
      "udCount": 5
    }

複数の可用性セット (たとえば、1 つのマスター ノード) とデータ ノード用に別の名前をプレフィックスとして使用できる場合は、複数の可用性セットを指定するか、前に示したモデルに従います 
特定の t シャツ サイズの変数を作成します。

##### storageSettings

多くの場合、ストレージの詳細は、リンクされたテンプレートと共有されます。 次の例で、 *storageSettings* オブジェクトはストレージ アカウントとコンテナー名の詳細を提供します。

    "storageSettings": {
        "vhdStorageAccountName": "[parameters('storageAccountName')]",
        "vhdContainerName": "[variables('vmStorageAccountContainerName')]",
        "destinationVhdsContainer": "[concat('https://', parameters('storageAccountName'), variables('vmStorageAccountDomain'), '/', variables('vmStorageAccountContainerName'), '/')]"
    }

##### osSettings

リンクされたテンプレートを使用すると、場合によっては、異なる既知の構成の種類における、さまざまなノードの種類に対して、オペレーティング システムの設定を渡す必要があります。 複雑なオブジェクトを格納し、オペレーティング システムの情報を共有する簡単な方法し、も 
展開に複数のオペレーティング システムの選択をサポートする容易にします。

次の例では、オブジェクトを *osSettings*:

    "osSettings": {
      "imageReference": {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "14.04.2-LTS",
        "version": "latest"
      }
    }

##### machineSettings

生成された変数 *machineSettings* さまざまな新しい VM を作成するための中核となる変数を含む複雑なオブジェクトです管理者のユーザー名とパスワード、VM 名、および、運用のプレフィックス。 
次のようには、システム イメージ リファレンス:

    "machineSettings": {
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]",
        "machineNamePrefix": "mongodb-",
        "osImageReference": {
            "publisher": "[variables('osFamilySpec').imagePublisher]",
            "offer": "[variables('osFamilySpec').imageOffer]",
            "sku": "[variables('osFamilySpec').imageSKU]",
            "version": "latest"
        }
    },

 *OsImageReference* から値を取得、 *osSettings* メイン テンプレートで定義されている変数。 つまり、VM のオペレーティング システムを簡単に変更することができます-完全またはベース 
[テンプレートの利用者の優先順位。

##### vmScripts

 *VmScripts* オブジェクトには、外側と内側の参照を含めて、VM インスタンスでダウンロードして実行するスクリプトの詳細が含まれています。 外部の参照には、インフラストラクチャが含まれます。 
内部の参照には、インストールされているソフトウェアと構成が含まれます。

使用する、 *scriptsToDownload* プロパティを VM にダウンロードするスクリプトを一覧表示します。

次の例に示すように、このオブジェクトには、さまざまな種類のアクションで使用するコマンドライン引数への参照も含まれます。 次の操作は、既定のインストールを実行します。 
個々 のノード、すべてのノードを展開した後に実行されるインストールおよびその他のスクリプトが指定されたテンプレートに固有です。

この例は、MongoDB をデプロイするのに使用するテンプレートの一部です。この MongoDB には、高可用性を実現するためにアービターが必要です。  *ArbiterNodeInstallCommand* に追加されている *vmScripts* アービターをインストールします。

変数のセクションには特定のテキストを定義する変数があり、適切な値を指定してスクリプトを実行します。

    "vmScripts": {
        "scriptsToDownload": [
            "[concat(variables('scriptUrl'), 'mongodb-', variables('osFamilySpec').osName, '-install.sh')]",
            "[concat(variables('sharedScriptUrl'), 'vm-disk-utils-0.1.sh')]"
        ],
        "regularNodeInstallCommand": "[variables('installCommand')]",
        "lastNodeInstallCommand": "[concat(variables('installCommand'), ' -l')]",
        "arbiterNodeInstallCommand": "[concat(variables('installCommand'), ' -a')]"
    },


## テンプレートから状態を返す

テンプレートにデータを渡すだけでなく、呼び出し元のテンプレートにデータを返して共有することもできます。  **出力** セクションのリンクされたテンプレートで消費可能なキー/値ペアを指定することができます 
でソース テンプレートです。

次の例は、リンクされているテンプレートで生成されたプライベート IP アドレスを渡す方法を示します。

    "outputs": {
        "masterip": {
            "value": "[reference(concat(variables('nicName'),0)).ipConfigurations[0].privateIPAddress]",
            "type": "string"
         }
    }

メイン テンプレート内では、次の構文でこのデータを使用できます。

    "masterIpAddress": {
        "value": "[reference('master-node').outputs.masterip.value]"
    }

## 次のステップ
- [Azure リソース マネージャーのテンプレートの作成](resource-group-authoring-templates.md)
- [Azure リソース マネージャーのテンプレートの関数](resource-group-template-functions.md)


