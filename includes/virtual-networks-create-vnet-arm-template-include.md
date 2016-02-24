## ARM テンプレートのダウンロードおよび理解

Github から既存の ARM テンプレートをダウンロードして VNet と 2 つのサブネットを作成し、そのテンプレートに変更を加えて再利用することができます。 再利用するには、次の手順に従ってください。

1. Https://github.com/Azure/azure-quickstart-templates/tree/master/101-two-subnets に移動します。
2. クリックして **azuredeploy.json**, 、] をクリックし、 **RAW**します。
3. お使いのコンピューター上のローカル フォルダーにファイルを保存します。
4. ARM テンプレートを使用したことがある場合は、手順 7. に進みます。
5. 保存したファイルを開き、フォルダー内のコンテンツを見て **パラメーター** 5 行目にします。 ARM テンプレートのパラメーターでは、デプロイ中に入力できる、値のプレース ホルダーが用意されています。

    | パラメーター | 説明 |
    |---|---|
    | **location** | VNet が作成される Azure リージョン |
    | **vnetName** | 新しい VNet の名前 |
    | **addressPrefix** | VNet のアドレス空間 (CIDR 形式) |
    | **subnet1Name** | 最初の VNet の名前 |
    | **subnet1Prefix** | 最初のサブネットの CIDR ブロック |
    | **subnet2Name** | 2 番目の VNet の名前 |
    | **subnet2Prefix** | 2 番目のサブネットの CIDR ブロック |

    >[AZURE.IMPORTANT] ARM テンプレートを github に保持されますが、時間の経過と共に変更できます。 使用する前に、必ずテンプレートを確認してください。
    
6. 下にあるコンテンツを確認して **リソース** し、次のことを確認します。

    - **型**します。 テンプレートによって作成されるリソースのタイプ。 この場合、 **Microsoft.Network/virtualNetworks**, 、VNet を表します。
    - **名前**します。 リソースの名前です。 使用して **[parameters('vnetName')]**, 、展開時にユーザーまたはパラメーター ファイルが入力として指定されますので、名前。
    - **プロパティ**します。 リソースのプロパティの一覧です。 VNet の作成中、このテンプレートではアドレス空間とサブネットのプロパティが使用されます。

7. Https://github.com/Azure/azure-quickstart-templates/tree/master/101-two-subnets に移動します。
8. クリックして **azuredeploy paremeters.json**, 、] をクリックし、 **RAW**します。
9. お使いのコンピューター上のローカル フォルダーにファイルを保存します。
10. 保存したファイルを開き、パラメーターの値を編集します。 次の値を使用して、このシナリオで説明した VNet をデプロイします。

        {
          "location": {
            "value": "Central US"
          },
          "vnetName": {
              "value": "TestVNet"
          },
          "addressPrefix": {
              "value": "192.168.0.0/16"
          },
          "subnet1Name": {
              "value": "FrontEnd"
          },
          "subnet1Prefix": {
            "value": "192.168.1.0/24"
          },
          "subnet2Name": {
              "value": "BackEnd"
          },
          "subnet2Prefix": {
              "value": "192.168.2.0/24"
          }
        }

11. ファイルを保存します。
  
