---
title: "Włączenie pulpitu zdalnego w usłudze w chmurze platformy Azure | Dokumentacja firmy Microsoft"
description: "Jak skonfigurować aplikację usługi chmury azure, aby umożliwić połączenia pulpitu zdalnego"
services: cloud-services
documentationcenter: 
author: thraka
manager: timlt
editor: 
ms.assetid: d3110ee8-6526-4585-aba5-d0bc9a713e9b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: 413e72e9a39fcde84f56bfc61a6bc72dbadf1c97
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Włączanie połączeń usług pulpitu zdalnego dla roli usług w chmurze Azure

> [!div class="op_single_selector"]
> * [Witryna Azure Portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [Klasyczna witryna Azure Portal](cloud-services-role-enable-remote-desktop.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Program Visual Studio](../vs-azure-tools-remote-desktop-roles.md)

Umożliwia Podłączanie pulpitu zdalnego w roli podczas tworzenia przez moduły pulpitu zdalnego w tym w definicji usługi lub można włączyć pulpitu zdalnego za pomocą rozszerzenia usług pulpitu zdalnego. Jest to preferowane rozwiązanie próbę użycia rozszerzenia usług pulpitu zdalnego, jak można włączyć pulpitu zdalnego, nawet po wdrożeniu aplikacji bez konieczności ponownego wdrażania aplikacji.

## <a name="configure-remote-desktop-from-the-azure-classic-portal"></a>Konfigurowanie pulpitu zdalnego z klasycznego portalu Azure
Klasyczny portal Azure używa metody rozszerzenia usług pulpitu zdalnego, można włączyć pulpitu zdalnego, nawet po wdrożeniu aplikacji. **Konfiguruj** strony dla usługi w chmurze można włączyć pulpitu zdalnego, zmiana konta administratora lokalnego używanego do łączenia się z maszynami wirtualnymi, certyfikat używane podczas uwierzytelniania oraz ustawianie daty wygaśnięcia.

1. Kliknij przycisk **usługi w chmurze**, kliknij nazwę usługi w chmurze, a następnie kliknij przycisk **Konfiguruj**.
2. Kliknij przycisk **zdalnego** znajdujący się u dołu.

    ![Usługi w chmurze zdalnego](./media/cloud-services-role-enable-remote-desktop/CloudServices_Remote.png)

   > [!WARNING]
   > Wszystkie wystąpienia roli zostanie ponownie uruchomiony, gdy najpierw włączyć pulpitu zdalnego i kliknij przycisk OK (znacznikiem wyboru). Aby uniknąć ponownego uruchomienia systemu, certyfikat używany do szyfrowania hasła musi być instalowany w roli. Aby uniknąć ponownego uruchomienia, [Prześlij certyfikat usługi w chmurze](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) , a następnie wróć do tego okna dialogowego.

3. W **ról**, wybierz rolę, aby zaktualizować lub wybierz **wszystkie** dla wszystkich ról.
4. Wprowadź następujące zmiany:

   * Aby włączyć Pulpit zdalny, zaznacz **Włącz pulpit zdalny** pole wyboru. Aby wyłączyć Pulpit zdalny, wyczyść pole wyboru.
   * Utwórz konto do użycia w połączeń pulpitu zdalnego do wystąpień roli.
   * Zaktualizuj hasło do istniejącego konta.
   * Wybierz certyfikat przekazany do użycia na potrzeby uwierzytelniania (Przekaż certyfikat przy użyciu **przekazać** na **certyfikaty** strony) lub Utwórz nowy certyfikat.
   * Zmień datę wygaśnięcia w konfiguracji pulpitu zdalnego.

5. Po zakończeniu aktualizacji konfiguracji, kliknij przycisk **OK** (znacznikiem wyboru).

## <a name="remote-into-role-instances"></a>Zdalne do wystąpień roli
Włączenie pulpitu zdalnego na role można zdalnego do wystąpienia roli za pomocą różnych narzędzi.

Aby podłączyć się do wystąpienia roli z klasycznego portalu Azure:

1. Kliknij przycisk **wystąpień** otworzyć **wystąpień** strony.
2. Wybierz wystąpienia roli, który ma pulpitu zdalnego skonfigurowane.
3. Kliknij przycisk **Connect**, a następnie postępuj zgodnie z instrukcjami, aby otworzyć pulpit.
4. Kliknij przycisk **Otwórz** , a następnie **Connect** nawiązaniu połączenia pulpitu zdalnego.

### <a name="use-visual-studio-to-remote-into-a-role-instance"></a>Za pomocą programu Visual Studio zdalne do wystąpienia roli
W programie Visual Studio, Eksploratora serwera:

1. Rozwiń węzeł **Azure** > **usługi w chmurze** > **[nazwa usługi w chmurze]** węzła.
2. Rozwiń pozycję **przemieszczania** lub **produkcji**.
3. Rozwiń poszczególnych ról.
4. Kliknij prawym przyciskiem myszy jeden z wystąpień roli, kliknij przycisk **łączyć się przy użyciu pulpitu zdalnego...** , a następnie wprowadź nazwę użytkownika i hasło.

![Pulpit zdalny Eksploratora serwera](./media/cloud-services-role-enable-remote-desktop/ServerExplorer_RemoteDesktop.png)

### <a name="use-powershell-to-get-the-rdp-file"></a>Pobierz plik RDP za pomocą programu PowerShell
Można użyć [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) polecenia cmdlet, aby pobrać plik RDP. Można następnie użyć pliku RDP z podłączania pulpitu zdalnego do uzyskania dostępu do usługi w chmurze.

### <a name="programmatically-download-the-rdp-file-through-the-service-management-rest-api"></a>Programowo Pobierz plik RDP za pomocą interfejsu API REST zarządzania usługi
Można użyć [Pobierz plik RDP](https://msdn.microsoft.com/library/jj157183.aspx) operacji REST można pobrać pliku RDP.

## <a name="to-configure-remote-desktop-in-the-service-definition-file"></a>Aby skonfigurować w pliku definicji usługi pulpitu zdalnego
Ta metoda umożliwia włączenie pulpitu zdalnego dla aplikacji w czasie projektowania. Ta metoda wymaga szyfrowania haseł można przechowywać w konfiguracji usługi plików i wszelkie aktualizacje konfiguracji pulpitu zdalnego wymaga ponownego wdrażania aplikacji. Jeśli chcesz uniknąć tych downsides, należy użyć metody zdalnego pulpitu rozszerzenia na podstawie opisanych powyżej.  

Można użyć programu Visual Studio do [połączeń usług pulpitu zdalnego włączyć](../vs-azure-tools-remote-desktop-roles.md) podejście pliku definicji usługi.  
Poniżej opisano zmiany, konieczne jest plików modeli usług można włączyć pulpitu zdalnego. Visual Studio będzie automatycznie tworzy te zmiany podczas publikowania.

### <a name="set-up-the-connection-in-the-service-model"></a>Konfigurowanie połączenia w modelu usług
Użyj **importów** element, aby zaimportować **RemoteAccess** modułu i **RemoteForwarder** moduł [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) pliku.

Plik definicji usługi powinien być podobny do poniższego przykładu z `<Imports>` elementu dodany.

```xml
<ServiceDefinition name="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2013-03.2.0">
    <WebRole name="WebRole1" vmsize="Small">
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="Endpoint1" endpointName="Endpoint1" />
                </Bindings>
            </Site>
        </Sites>
        <Endpoints>
            <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
            <Import moduleName="Diagnostics" />
            <Import moduleName="RemoteAccess" />
            <Import moduleName="RemoteForwarder" />
        </Imports>
    </WebRole>
</ServiceDefinition>
```
[Pliku ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) plik powinien być podobny do poniższego przykładu, należy pamiętać, `<ConfigurationSettings>` i `<Certificates>` elementy. Określony certyfikat musi być [przekazane do usługi w chmurze](cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2013-03.2.0">
    <Role name="WebRole1">
        <Instances count="2" />
        <ConfigurationSettings>
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.Enabled" value="true" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountUsername" value="[name-of-user-account]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountEncryptedPassword" value="[base-64-encrypted-user-password]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountExpiration" value="[certificate-expiration]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteForwarder.Enabled" value="true" />
        </ConfigurationSettings>
        <Certificates>
            <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="[certificate-thumbprint]" thumbprintAlgorithm="sha1" />
        </Certificates>
    </Role>
</ServiceConfiguration>
```


## <a name="additional-resources"></a>Dodatkowe zasoby
[Jak skonfigurować usługi w chmurze](cloud-services-how-to-configure.md)
[usług w chmurze — często zadawane pytania — pulpitu zdalnego](cloud-services-faq.md)
