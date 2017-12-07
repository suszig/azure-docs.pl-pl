---
title: "Sposób użycia interfejsu API (Python) — Przewodnik po funkcji zarządzania usługami"
description: "Dowiedz się, jak programowo wykonywać typowe zadania zarządzania dla usługi w języku Python."
services: cloud-services
documentationcenter: python
author: lmazuel
manager: wpickett
editor: 
ms.assetid: 61538ec0-1536-4a7e-ae89-95967fe35d73
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/30/2017
ms.author: lmazuel
ms.openlocfilehash: a55a38df765dcd1947312e729dbd37e3284876cf
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/07/2017
---
# <a name="how-to-use-service-management-from-python"></a>Jak używać usługi zarządzania w języku Python
Ten przewodnik przedstawia, jak programowo wykonywać typowe zadania zarządzania dla usługi w języku Python. **ServiceManagementService** klasy w [zestaw Azure SDK for Python](https://github.com/Azure/azure-sdk-for-python) obsługuje dostęp programistyczny umożliwiający wiele usług związanych z zarządzaniem funkcji dostępnych w [Azure Portal] [ management-portal] (takich jak **tworzenie, aktualizowanie i usuwanie usługi w chmurze, wdrożeń, dane usługi zarządzania i maszyn wirtualnych**). Ta funkcja może być przydatne do tworzenia aplikacji, które muszą dostęp programistyczny umożliwiający zarządzanie usługą.

## <a name="WhatIs"></a>Co to jest usługa zarządzania
Interfejs API zarządzania usługami zapewnia dostęp programistyczny umożliwiający wiele funkcji zarządzania usługi dostępne za pośrednictwem [portalu Azure][management-portal]. Zestaw Azure SDK for Python umożliwia zarządzanie usługi w chmurze i kont magazynu.

Aby użyć interfejs API zarządzania usługami, musisz [tworzenia konta platformy Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="Concepts"></a>Pojęcia
Zestaw Azure SDK for Python opakowuje [interfejs API zarządzania usługami Azure][svc-mgmt-rest-api], która jest interfejs API REST. Wszystkie operacje interfejsu API są realizowane poprzez protokół SSL z wzajemnym uwierzytelnieniem certyfikatami X.509 v3. Dostęp do usługi zarządzania można uzyskać poprzez usługę uruchomioną w systemie Azure lub bezpośrednio przez Internet z dowolnej aplikacji, która potrafi przesłać żądanie HTTPS i odbierać odpowiedzi HTTPS.

## <a name="Installation"></a>Instalacji
Wszystkie funkcje opisane w tym artykule są dostępne w `azure-servicemanagement-legacy` pakiet, który można zainstalować przy użyciu narzędzia pip. Aby uzyskać więcej informacji na temat instalacji (na przykład jeśli jesteś nowym użytkownikiem Python), zobacz ten artykuł: [instalowanie Python i zestawu Azure SDK](../python-how-to-install.md)

## <a name="Connect"></a>Jak: nawiązać połączenia z usługi zarządzania
Aby połączyć się z punktem końcowym usługi zarządzania, należy identyfikator subskrypcji platformy Azure i prawidłowy certyfikat zarządzania. Możesz uzyskać identyfikator subskrypcji za pośrednictwem [portalu Azure][management-portal].

> [!NOTE]
> Obecnie istnieje możliwość używania certyfikatów utworzone za pomocą biblioteki OpenSSL uruchomionej w systemie Windows.  Wymaga to Python 2.7.4 lub nowszym. Firma Microsoft zaleca użytkownikom używanie biblioteki OpenSSL zamiast PFX, ponieważ obsługa PFX certyfikatów prawdopodobnie zostaną usunięte w przyszłości.
>
>

### <a name="management-certificates-on-windowsmaclinux-openssl"></a>Certyfikaty zarządzania na systemem Windows lub Mac/Linux (OpenSSL)
Można użyć [OpenSSL](http://www.openssl.org/) można utworzyć certyfikat zarządzania.  Rzeczywiście, należy utworzyć dwa certyfikaty, jeden dla serwera ( `.cer` plików) i jeden dla klienta ( `.pem` pliku). Aby utworzyć `.pem` plików, należy wykonać:

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Aby utworzyć `.cer` certyfikatów, należy wykonać:

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

Aby uzyskać więcej informacji o certyfikatach Azure, zobacz [Omówienie certyfikatów dla usług w chmurze Azure](cloud-services-certs-create.md). Pełny opis parametrów biblioteki OpenSSL, zobacz dokumentację w [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html).

Po utworzeniu tych plików, należy przekazać `.cer` plików na platformie Azure przy użyciu akcji "Przekazywanie" kartę "Ustawienia" [portalu Azure][management-portal], i zwróć uwagę na którym został zapisany `.pem` pliku.

Po uzyskaniu identyfikator subskrypcji utworzony certyfikat i przekazać `.cer` plików na platformie Azure, można połączyć z punktem końcowym zarządzania platformy Azure przy przekazywanie identyfikator subskrypcji i ścieżkę do `.pem` pliku **ServiceManagementService**:

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

W powyższym przykładzie `sms` jest **ServiceManagementService** obiektu. **ServiceManagementService** klasa jest klasą podstawowy używany do zarządzania usługami Azure.

### <a name="management-certificates-on-windows-makecert"></a>Certyfikaty zarządzania w systemie Windows (MakeCert)
Można utworzyć certyfikat zarządzania z podpisem własnym na przy użyciu maszyny `makecert.exe`.  Otwórz **wiersz polecenia programu Visual Studio** jako **administratora** i należy użyć następującego polecenia, zastępując *AzureCertificate* o nazwie certyfikat, chcesz użyć.

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

Polecenie tworzy `.cer` pliku i instaluje je w **osobistych** magazynu certyfikatów. Aby uzyskać więcej informacji, zobacz [Omówienie certyfikatów dla usług w chmurze Azure](cloud-services-certs-create.md).

Po utworzeniu certyfikatu należy przekazać `.cer` plików na platformie Azure przy użyciu akcji "Przekazywanie" kartę "Ustawienia" [portalu Azure][management-portal].

Po uzyskaniu identyfikator subskrypcji utworzony certyfikat i przekazać `.cer` plików na platformie Azure, można połączyć z punktem końcowym zarządzania platformy Azure przy przekazywanie identyfikator subskrypcji i lokalizacji certyfikatu w Twojej **osobistych** magazyn certyfikatów **ServiceManagementService** (ponownie, Zastąp *AzureCertificate* o nazwie certyfikatu):

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

W powyższym przykładzie `sms` jest **ServiceManagementService** obiektu. **ServiceManagementService** klasa jest klasą podstawowy używany do zarządzania usługami Azure.

## <a name="ListAvailableLocations"></a>Jak: wyświetlić listę dostępnych lokalizacji
Aby wyświetlić lokalizacje, które są dostępne dla usług hostingu, użyj **listy\_lokalizacje** metody:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

Podczas tworzenia usługi w chmurze lub Usługa magazynu, musisz podać prawidłową lokalizację. **Listy\_lokalizacje** metoda zawsze zwraca aktualną listę dostępnych lokalizacji. Opracowywania tego tekstu, są dostępne lokalizacje:

* Europa Zachodnia
* Europa Północna
* Azja Południowo-Wschodnia
* Azja Wschodnia
* Środkowe stany USA
* Środkowo-północne stany USA
* Południowo-środkowe stany USA
* Zachodnie stany USA
* Wschodnie stany USA
* Japonia Wschodnia
* Japonia Zachodnia
* Brazylia Południowa
* Australia Wschodnia
* Australia Południowo-Wschodnia

## <a name="CreateCloudService"></a>Porady: Tworzenie usługi w chmurze
Podczas tworzenia aplikacji i uruchom go na platformie Azure, kod i konfiguracja łącznie są nazywane Azure [usługi w chmurze] [ cloud service] (nazywane *usługi hostowanej* we wcześniejszych wersjach platformy Azure). **Utworzyć\_hostowane\_usługi** metoda pozwala utworzyć nową usługę hostowaną, podając nazwę usługi hostowanej (który musi być unikatowa na platformie Azure), etykiety (automatycznie zakodowane w formacie base64), opis oraz lokalizację.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

Można wyświetlić listę wszystkich usług hostowanych dla Twojej subskrypcji z **listy\_hostowanej\_usług** metody:

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

Jeśli chcesz uzyskać informacje o określonej usługi hostowanej, możesz to zrobić przez przekazanie nazwa usługi hostowanej **uzyskać\_hostowanej\_usługi\_właściwości** metody:

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

Po utworzeniu usługi w chmurze, można wdrożyć kodu do usługi z **utworzyć\_wdrożenia** metody.

## <a name="DeleteCloudService"></a>Porady: usuwanie usługi w chmurze
Można usunąć usługi w chmurze, przekazując nazwy usługi na **usunąć\_hostowane\_usługi** metody:

    sms.delete_hosted_service('myhostedservice')

Przed usunięciem usługi należy najpierw usunąć wszystkie wdrożenia dla usługi. (Zobacz [porady: usuwanie wdrożenia](#DeleteDeployment) Aby uzyskać więcej informacji.)

## <a name="DeleteDeployment"></a>Porady: usuwanie wdrożenia
Aby usunąć wdrożenia, użyj **usunąć\_wdrożenia** metody. Poniższy przykład pokazuje, jak usunąć wdrożenie o nazwie `v1`.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <a name="CreateStorageService"></a>Porady: Tworzenie usługi magazynu
A [usługi magazynu](../storage/common/storage-create-storage-account.md) zapewnia dostęp do usługi Azure [obiekty BLOB](../storage/blobs/storage-python-how-to-use-blob-storage.md), [tabel](../cosmos-db/table-storage-how-to-use-python.md), i [kolejek](../storage/queues/storage-python-how-to-use-queue-storage.md). Aby utworzyć usługę magazynu, należy nazwę usługi (od 3 do 24 małych liter i unikatowa na platformie Azure), opis, etykiety (maksymalnie 100 znaków, automatycznie zakodowane w formacie base64) i lokalizację. Poniższy przykład przedstawia sposób tworzenia usługi magazynu, określając lokalizację.

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

Uwaga w poprzednim przykładzie który stan **utworzyć\_magazynu\_konta** operacji mogą być pobierane przez przekazanie wynik zwracany przez **utworzyć\_magazynu\_konta** do **uzyskać\_operacji\_stan** — metoda.  

Można wyświetlić listę kont magazynu oraz ich właściwości z **listy\_magazynu\_kont** metody:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <a name="DeleteStorageService"></a>Porady: usuwanie usługi magazynu
Usługa magazynu można usunąć, przekazując nazwy usługi magazynu do **usunąć\_magazynu\_konta** metody. Usunięcie usługi magazynu powoduje usunięcie wszystkich danych przechowywanych w usłudze (obiekty BLOB, tabel i kolejek).

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <a name="ListOperatingSystems"></a>Jak: wyświetlić listę dostępnych systemów operacyjnych
Aby wyświetlić listę systemów operacyjnych, które są dostępne dla usług hostingu, użyj **listy\_operacyjnego\_systemów** metody:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

Alternatywnie można użyć **listy\_operacyjnego\_systemu\_rodzin** metodę, która grupuje rodziny systemów operacyjnych:

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <a name="CreateVMImage"></a>Porady: Tworzenie obrazu systemu operacyjnego
Aby dodać obraz systemu operacyjnego do repozytorium obrazów, użyj **dodać\_os\_obrazu** metody:

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

Aby wyświetlić listę dostępnych obrazów systemu operacyjnego, należy użyć **listy\_os\_obrazów** metody. Zawiera wszystkie obrazy platformy i obrazów użytkowników:

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

## <a name="DeleteVMImage"></a>Porady: usuwanie obrazu systemu operacyjnego
Aby usunąć obrazu użytkownika, użyj **usunąć\_systemu operacyjnego\_obrazu** metody:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <a name="CreateVM"></a>Porady: tworzenie maszyny wirtualnej
Aby utworzyć maszynę wirtualną, należy najpierw utworzyć [usługi w chmurze](#CreateCloudService).  Następnie utworzyć przy użyciu wdrożenia maszyny wirtualnej **utworzyć\_wirtualnego\_maszyny\_wdrożenia** metody:

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

## <a name="DeleteVM"></a>Porady: usuwanie maszyny wirtualnej
Aby usunąć maszynę wirtualną, należy najpierw usunąć wdrożenia przy użyciu **usunąć\_wdrożenia** metody:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

Następnie można usunąć usługi w chmurze przy użyciu **usunąć\_hostowane\_usługi** metody:

    sms.delete_hosted_service(service_name='myvm')

## <a name="how-to-create-a-virtual-machine-from-a-captured-virtual-machine-image"></a>Porady: Tworzenie maszyny wirtualnej z obrazu przechwyconych maszyny wirtualnej
Aby przechwycić obraz maszyny Wirtualnej, należy najpierw wywołać **przechwytywania\_wirtualna\_obrazu** metody:

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

Następnie, aby upewnić się, że pomyślnie przechwyceniu obrazu, należy użyć **listy\_wirtualna\_obrazów** interfejsu api i upewnij się, że obraz jest wyświetlana w wynikach:

    images = sms.list_vm_images()

Aby na koniec utwórz maszynę wirtualną przy użyciu przechwyconego obrazu, należy użyć **utworzyć\_wirtualnego\_maszyny\_wdrożenia** jak wcześniej, ale tym razem przekazywanie vm_image_name zamiast tego

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

Aby dowiedzieć się więcej o sposobie przechwytywania maszyny wirtualnej systemu Linux, zobacz [Przechwytywanie maszyny wirtualnej systemu Linux.](../virtual-machines/linux/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

Aby dowiedzieć się więcej o sposobie przechwytywania maszyny wirtualnej systemu Windows, temacie [jak przechwycić maszynę wirtualną systemu Windows.](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="What's Next"> </a>Następne kroki
Teraz, kiedy znasz już podstawy usługi zarządzania, należy uzyskać dostęp [kompletne API dokumentacji dla zestawu Azure SDK Python](http://azure-sdk-for-python.readthedocs.org/) i wykonać złożone zadania łatwe do zarządzania aplikacją python.

Więcej informacji możesz znaleźć w [Centrum deweloperów języka Python](/develop/python/).

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
[management-portal]: https://portal.azure.com/
[svc-mgmt-rest-api]: http://msdn.microsoft.com/library/windowsazure/ee460799.aspx


[cloud service]:/services/cloud-services/
