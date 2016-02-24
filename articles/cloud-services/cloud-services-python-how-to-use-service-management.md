<properties
    pageTitle="サービス管理 API の使用方法 (Python) - 機能ガイド"
    description="Python から一般的なサービス管理タスクをプログラムで実行する方法について説明します。"
    services="cloud-services"
    documentationCenter="python"
    authors="huguesv"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="11/03/2015"
    ms.author="huvalo"/>

# Python からサービス管理を使用する方法

このガイドでは、Python から一般的なサービス管理タスクをプログラムで実行する方法について説明します。  **ServiceManagementService** クラス、 [Azure SDK for Python](../python-how-to-install.md) で提供されるサービス管理に関連する機能の多くにプログラムでアクセスをサポートしている、 [Azure クラシック ポータル][management-portal] (など **の作成、更新、およびクラウド サービス、展開、データ管理サービスおよび仮想マシンを削除する**)。 この機能は、サービス管理へのプログラムによるアクセスが必要なアプリケーションをビルドするために役立つ場合があります。

> [AZURE.NOTE] サービス管理 API は、新しいリソース管理 API を使用して、プレビュー リリースで現在使用できる交換しています。  参照してください、 [Azure リソース管理ドキュメント](http://azure-sdk-for-python.readthedocs.org/) Python から新しいリソース管理 API の使用の詳細について。


## <a name="WhatIs"> </a>サービス管理とは
サービス管理 API を通じて使用可能なサービス管理の機能の多くにプログラムでアクセスを提供する、 [Azure クラシック ポータル][management-portal]します。 Azure SDK for Python を使用すると、クラウド サービスとストレージ アカウントを管理できます。

サービス管理 API を使用する必要があります [Azure アカウントの作成](http://azure.microsoft.com/pricing/free-trial/)します。

## <a name="Concepts"> </a>概念
Azure SDK for Python ラップ、 [Azure サービス管理 API][svc-mgmt-rest-api], 、REST API であります。 すべての API 操作は SSL 上で実行され、X.509 v3 証明書を使用して相互認証されます。 管理サービスへのアクセスは、Azure で実行されているサービス内から行うことも、HTTPS 要求の送信と HTTPS 応答の受信の機能を持つ任意のアプリケーションからインターネット上で直接行うこともできます。

## <a name="Connect"> </a>方法: サービス管理に接続する
サービス管理エンドポイントに接続するには、Azure サブスクリプション ID、および有効な管理証明書が必要です。 サブスクリプション ID を取得、 [Azure クラシック ポータル][management-portal]します。

> [AZURE.NOTE] Azure sdk for Python v0.8.0 以降、Windows で実行中の OpenSSL で作成した証明書を使用することは今すぐです。  これには、Python 2.7.4 以降が必要です。 .pfx 証明書のサポートは今後削除される可能性があるため、.pfx の代わりに OpenSSL を使用することをお勧めします。

### Windows、Mac または Linux での管理証明書 (OpenSSL)
使用する [OpenSSL](http://www.openssl.org/) 管理証明書を作成します。  実際は 2 つの証明書を作成する必要があります。1 つはサーバー用 (`.cer` ファイル)、もう 1 つはクライアント用 (`.pem` ファイル) です。 `.pem` ファイルを作成するには、次のコマンドを実行します。

    `openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem`

`.cer` 証明書を作成するには、次のコマンドを実行します。

    `openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer`

Azure 証明書の詳細については、次を参照してください。 [Azure クラウド サービスの証明書の概要](./cloud-services-certs-create.md)します。 OpenSSL のパラメーターの詳細については、あるドキュメントを参照してください。 [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html)します。

これらのファイルを作成した後は、アップロードする必要があります、 `.cer` ファイルを Azure の [設定] タブの [アップロード] アクション、 [Azure クラシック ポータル][management-portal], 、保存場所をメモしておいて必要があります、 `.pem` ファイルです。

証明書を作成し、アップロードするサブスクリプション ID を取得した後、 `.cer` ファイル、サブスクリプション id とへのパスを渡すことによって Azure に Azure の管理エンドポイントに接続できます、 `.pem` ファイルを **ServiceManagementService**:

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

上記の例で `sms` は、 **ServiceManagementService** オブジェクトです。  **ServiceManagementService** クラスは、Azure サービスを管理するために使用する主なクラスです。

### Windows での管理証明書 (MakeCert)

`makecert.exe` を使用して、マシン上で自己署名管理証明書を作成できます。  開いている、 **Visual Studio コマンド プロンプト** として、 **管理者** し以下のコマンドを交換 *AzureCertificate* を使用する証明書の名前とします。

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

コマンドは、作成、 `.cer` ファイルを開き、インストールで、 **個人** 証明書ストア。 詳細については、次を参照してください。 [Azure クラウド サービスの証明書の概要](./cloud-services-certs-create.md)します。

証明書を作成した後は、アップロードする必要があります、 `.cer` ファイルを Azure の [設定] タブの [アップロード] アクション、 [Azure クラシック ポータル][management-portal]します。

証明書を作成し、アップロードするサブスクリプション ID を取得した後、 `.cer` ファイル、サブスクリプション id とは証明書の場所を渡すことによって Azure に Azure の管理エンドポイントに接続できます、 **個人** する証明書ストア **ServiceManagementService** (ここでも、置き換える *AzureCertificate* 、証明書の名前に置き換えます)。

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

上記の例で `sms` は、 **ServiceManagementService** オブジェクトです。  **ServiceManagementService** クラスは、Azure サービスを管理するために使用する主なクラスです。

## <a name="ListAvailableLocations"> </a>方法: 利用可能な場所を列挙する

サービスをホストするために利用できる場所の一覧を表示する、 **list \_locations** メソッド。

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

クラウド サービスやストレージ サービスを作成するときは、有効な場所を指定する必要があります。  **List \_locations** メソッドは現在利用可能な場所の最新のリストを常に返します。 この記事の執筆時点で利用可能な場所は次のとおりです。

- 西ヨーロッパ
- 北ヨーロッパ
- 東南アジア
- 東アジア
- 米国中央部
- 米国中北部
- 米国中南部
- 米国西部
- 米国東部
- 東日本
- 西日本
- ブラジル南部
- オーストラリア東部
- オーストラリア南東部

## <a name="CreateCloudService"> </a>方法: クラウド サービスの作成

アプリケーションを作成してそれを Azure で実行して、コードと構成をあわせてと呼ばれる Azure [cloud service] (と呼ばれる、 *ホステッド サービス* 以前の Azure リリース)。  **\_Hosted\_service** メソッドを使用して、ホステッド サービス名 (Azure 内で一意である必要があります)、ラベル (base64 に自動的にエンコードされます)、説明、場所を提供し、新しいホステッド サービスを作成できます。

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

サブスクリプションのすべてのホステッド サービスを一覧表示、 **\_hosted\_services** メソッド。

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

特定のホステッド サービスに関する情報を取得する場合は、これを行うホステッド サービス名を渡すことによって、 **get \_hosted\_service\_properties** メソッド。

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

使用してサービスに、コードを展開するには、クラウド サービスを作成したら、 **create \_deployment** メソッドです。

## <a name="DeleteCloudService"> </a>方法: クラウド サービスの削除

クラウド サービスを削除するには、サービス名を渡す、 **delete \_hosted\_service** メソッド。

    sms.delete_hosted_service('myhostedservice')

サービスを削除する前に、そのサービスのすべてのデプロイを最初に削除する必要があることに注意してください (参照 [方法: デプロイの削除](#DeleteDeployment) 詳細です)。

## <a name="DeleteDeployment"> </a>方法: デプロイの削除

展開の削除を使用して、 **delete \_deployment** メソッドです。 次の例では、`v1` という名前のデプロイメントを削除する方法を示しています。

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <a name="CreateStorageService"> </a>方法: ストレージ サービスを作成する

A [記憶域サービス](../storage/storage-create-storage-account.md) Azure へのアクセスを提供 [Blob](../storage/storage-python-how-to-use-blob-storage.md), 、[テーブル](../storage/storage-python-how-to-use-table-storage.md), 、および [キュー](../storage/storage-python-how-to-use-queue-storage.md)します。 ストレージ サービスを作成するには、サービスの名前 (Azure 内で一意の 3 〜 24 文字の小文字)、説明、ラベル (最大 100 文字、Base64 に自動的にエンコードされます)、場所が必要です。 次の例では、場所を指定してストレージ サービスを作成する方法を示しています。

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mystorageaccount'
    label = 'mystorageaccount'
    location = 'West US'
    desc = 'My storage account description.'

    result = sms.create_storage_account(name, desc, label, location=location)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

状態を上記の例では注意してください、 **create \_storage\_account** によって返された結果を渡すことによって、操作を取得できる **create \_storage\_account** に、 **\_operation\_status** メソッドです。  

ストレージ アカウントとそのプロパティを表示できる、 **\_storage\_accounts** メソッド。

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <a name="DeleteStorageService"> </a>方法: ストレージ サービスを削除する

ストレージ サービスを削除するには、記憶域のサービス名を渡す、 **delete \_storage\_account** メソッドです。 ストレージ サービスを削除すると、サービスに格納されているすべてのデータ (BLOB、テーブル、キュー) が削除されます。

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <a name="ListOperatingSystems"> </a>方法: 利用可能なオペレーティング システムを列挙する

ホスティング サービスで利用可能なオペレーティング システムの一覧を表示する、 **\_operating\_systems** メソッド。

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

また、使用することができます、 **\_operating\_system\_families** メソッドで、オペレーティング システムがファミリにグループ化します。

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <a name="CreateVMImage"> </a>方法: オペレーティング システム イメージを作成する

オペレーティング システム イメージをイメージ リポジトリに追加するには、使用、 **add \_os\_image** メソッド。

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mycentos'
    label = 'mycentos'
    os = 'Linux' # Linux or Windows
    media_link = 'url_to_storage_blob_for_source_image_vhd'

    result = sms.add_os_image(label, media_link, name, os)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

利用できるオペレーティング システム イメージの一覧を使用して、 **\_os\_images** メソッドです。 このメソッドでは、すべてのプラットフォーム イメージとユーザー イメージが対象となります。

    result = sms.list_os_images()

    for image in result:
        print('Name: ' + image.name)
        print('Label: ' + image.label)
        print('OS: ' + image.os)
        print('Category: ' + image.category)
        print('Description: ' + image.description)
        print('Location: ' + image.location)
        print('Media link: ' + image.media_link)
        print('')

## <a name="DeleteVMImage"> </a>方法: オペレーティング システム イメージを削除する

ユーザー イメージを削除するには、 **delete \_os\_image** メソッド。

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <a name="CreateVM"> </a>方法: 仮想マシンを作成する

仮想マシンを作成するには、最初に作成する必要が、 [クラウド サービス](#CreateCloudService)します。  使用して仮想マシンの展開を作成し、 **create \_virtual\_machine\_deployment** メソッド。

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    # Name of an os image as returned by list_os_images
    image_name = 'OpenLogic__OpenLogic-CentOS-62-20120531-en-us-30GB.vhd'

    # Destination storage account container/blob where the VM disk
    # will be created
    media_link = 'url_to_target_storage_blob_for_vm_hd'

    # Linux VM configuration, you can use WindowsConfigurationSet
    # for a Windows VM instead
    linux_config = LinuxConfigurationSet('myhostname', 'myuser', 'mypassword', True)

    os_hd = OSVirtualHardDisk(image_name, media_link)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=os_hd,
        role_size='Small')

## <a name="DeleteVM"> </a>方法: 仮想マシンを削除する

削除する仮想マシンを最初に削除する展開を使用して、 **delete \_deployment** メソッド。

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

使用してクラウド サービスを削除することができます、 **delete \_hosted\_service** メソッド。

    sms.delete_hosted_service(service_name='myvm')

##方法: キャプチャした仮想マシン イメージから仮想マシンを作成する

VM イメージをキャプチャするには、まず呼び出して、 **capture\_vm\_image** メソッド。

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    # replace the below three parameters with actual values
    hosted_service_name = 'hs1'
    deployment_name = 'dep1'
    vm_name = 'vm1'

    image_name = vm_name + 'image'
    image = CaptureRoleAsVMImage    ('Specialized',
        image_name,
        image_name + 'label',
        image_name + 'description',
        'english',
        'mygroup')

    result = sms.capture_vm_image(
            hosted_service_name,
            deployment_name,
            vm_name,
            image
        )

次に、イメージが正常にキャプチャされたことを確認するには、 **list\_vm\_images** api し、イメージが結果に表示されているかどうかを確認します。

    images = sms.list_vm_images()

最後に、キャプチャしたイメージを使用して仮想マシンを作成するには、使用、 **create \_virtual\_machine\_deployment** 前に、今回は vm_image_name を渡します、代わりにメソッド

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=None,
        role_size='Small',
        vm_image_name = image_name)

Linux 仮想マシンをキャプチャする方法の詳細については、次を参照してください。 [を Linux 仮想マシンをキャプチャする方法です。](../virtual-machines/virtual-machines-linux-capture-image.md)

Windows 仮想マシンをキャプチャする方法の詳細については、次を参照してください。 [を Windows 仮想マシンをキャプチャする方法です。](../virtual-machines/virtual-machines-capture-image-windows-server.md)

## <a name="What's Next"> </a>次のステップ

これでサービス管理の基本を学習できました。、にアクセスできる、 [Azure Python SDK の完全な API リファレンス ドキュメント](http://azure-sdk-for-python.readthedocs.org/) python アプリケーションを管理するには、簡単に複雑なタスクを実行します。

詳細については、次を参照してください。、 [Python デベロッパー センター](/develop/python/)します。

[What is Service Management]: #WhatIs
[Concepts]: #Concepts
[How to: Connect to service management]: #Connect
[How to: List available locations]: #ListAvailableLocations
[How to: Create a cloud service]: #CreateCloudService
[How to: Delete a cloud service]: #DeleteCloudService
[How to: Create a deployment]: #CreateDeployment
[How to: Update a deployment]: #UpdateDeployment
[How to: Move deployments between staging and production]: #MoveDeployments
[How to: Delete a deployment]: #DeleteDeployment
[How to: Create a storage service]: #CreateStorageService
[How to: Delete a storage service]: #DeleteStorageService
[How to: List available operating systems]: #ListOperatingSystems
[How to: Create an operating system image]: #CreateVMImage
[How to: Delete an operating system image]: #DeleteVMImage
[How to: Create a virtual machine]: #CreateVM
[How to: Delete a virtual machine]: #DeleteVM
[Next Steps]: #NextSteps
[management-portal]: https://manage.windowsazure.com/
[svc-mgmt-rest-api]: http://msdn.microsoft.com/library/windowsazure/ee460799.aspx


[cloud service]:https://azure.microsoft.com/en-us/documentation/services/cloud-services/


