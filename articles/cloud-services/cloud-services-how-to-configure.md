---
title: "Jak skonfigurować usługi w chmurze (klasyczny portal) | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować usługi w chmurze na platformie Azure. Dowiedz się zaktualizować konfigurację usługi w chmurze i konfigurowania dostępu zdalnego do wystąpień roli."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 4902f79d-ea91-41ca-89a4-7c818180ee5f
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 39bb294c96ce0c12d91cf8b3488ac3e1a7b2f7b2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-cloud-services"></a>Jak skonfigurować usługi w chmurze
> [!div class="op_single_selector"]
> * [Witryna Azure Portal](cloud-services-how-to-configure-portal.md)
> * [Klasyczna witryna Azure Portal](cloud-services-how-to-configure.md)
> 
> 

Najczęściej używane ustawienia dla usługi w chmurze można skonfigurować w klasycznym portalu Azure. Jeśli wolisz bezpośrednio aktualizować pliki konfiguracji, pobierz odpowiedni plik, a następnie przekaż jego zaktualizowaną wersję i zaktualizuj usługę w chmurze, wprowadzając zmiany w konfiguracji. W obu przypadkach aktualizacje konfiguracji są przekazywane do wszystkich wystąpień ról.

Klasyczny portal Azure umożliwia również [włączyć Podłączanie pulpitu zdalnego dla roli w usług Azure Cloud Services](cloud-services-role-enable-remote-desktop.md)

Azure tylko sposobem zapewnienia dostępności usługi 99,95% podczas aktualizowania konfiguracji, jeśli masz co najmniej dwóch wystąpień roli dla każdej roli. Umożliwiająca jednej maszyny wirtualnej do przetworzenia żądania klientów podczas innych aktualizacji. Aby uzyskać więcej informacji, zobacz [umowy dotyczące poziomu usług](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Zmiana usługi w chmurze
1. W [klasycznego portalu Azure](http://manage.windowsazure.com/), kliknij przycisk **usługi w chmurze**, kliknij nazwę usługi w chmurze, a następnie kliknij przycisk **Konfiguruj**.
   
    ![Na stronie konfiguracji](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage1.png)
   
    Na **Konfiguruj** strony, można skonfigurować monitorowanie ustawień roli aktualizacji i wybierz system operacyjny gościa i rodziny wystąpień roli. 
2. W **monitorowania**ustawiony poziom monitorowania pełne lub minimalnego i konfigurowania parametrów połączeń diagnostyki, które są wymagane do pełnego monitorowania.
3. Usługi ról (pogrupowane według roli) można aktualizować następujące ustawienia:
   
    * **Ustawienia** zmodyfikuj wartości ustawień dodatkowych konfiguracji, które są określone w *appSettings* elementy usługi pliku konfiguracji (cscfg).

    * **Certyfikaty**  
        Zmień odcisk palca certyfikatu, który jest używany w przypadku szyfrowania SSL dla roli. Aby zmienić certyfikat, należy najpierw przekazać nowy certyfikat (na **certyfikaty** strony). Następnie zaktualizować odcisk palca w ciągu certyfikat wyświetlany w ustawieniach roli.
4. W **systemu operacyjnego**, można zmienić rodziny systemów operacyjnych lub wersja dla wystąpień roli, lub wybierz **automatyczne** Aby włączyć Aktualizacje automatyczne w bieżącej wersji systemu operacyjnego. Ustawienia systemu operacyjnego mają zastosowanie do ról sieć web i roli proces roboczy, ale nie wpływają na maszynach wirtualnych.
   
    Podczas wdrażania najnowszej wersji systemu operacyjnego jest zainstalowany na wszystkich wystąpień ról i systemy operacyjne są automatycznie aktualizowane domyślnie. 
   
    Jeśli potrzebujesz dla usługi w chmurze do uruchamiania na innej wersji systemu operacyjnego ze względu na wymagania zgodności w kodzie, można wybrać rodziny systemów operacyjnych i wersji. Po wybraniu konkretnej wersji systemu operacyjnego są wstrzymywane, aktualizacje automatyczne systemu operacyjnego dla usługi w chmurze. Należy upewnić się, że systemy operacyjne otrzymywać aktualizacje.
   
    Jeśli należy rozwiązać wszystkie problemy ze zgodnością, które aplikacje mają z najnowszej wersji systemu operacyjnego, można włączyć Aktualizacje automatyczne systemu operacyjnego przez ustawienie wersji systemu operacyjnego na **automatyczne**. 
   
    ![Ustawienia systemu operacyjnego](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage_OSSettings.png)
5. Kliknij, aby zapisać ustawienia konfiguracji i wypychanie ich wystąpień roli **zapisać**. (Kliknij **odrzucić** Aby anulować zmiany.) **Zapisz** i **odrzucić** po zmianie ustawienia zostaną dodane do paska poleceń.

## <a name="update-a-cloud-service-configuration-file"></a>Aktualizuj plik konfiguracji usługi w chmurze
1. Pobierz chmury pliku konfiguracji usługi (cscfg) w bieżącej konfiguracji. Na **Konfiguruj** dla usługi w chmurze kliknij pozycję **Pobierz**. Następnie kliknij przycisk **zapisać**, lub kliknij przycisk **Zapisz jako** można zapisać pliku.
2. Po zaktualizowaniu pliku konfiguracji usługi, przekazywanie i stosowania aktualizacji konfiguracji:
   
   1. Na **Konfiguruj** kliknij przycisk **przekazać**.
      
       ![Przekazywanie konfiguracji](./media/cloud-services-how-to-configure/CloudServices_UploadConfigFile.png)
   2. W **pliku konfiguracyjnego**, użyj **Przeglądaj** i wybierz plik .cscfg zaktualizowane.
   3. Usługi w chmurze zawiera wszystkie role, które mają tylko jedno wystąpienie, wybierz opcję **zastosowanie konfiguracji, nawet jeśli co najmniej jedna rola zawiera pojedyncze wystąpienie** pole wyboru, aby włączyć aktualizacje konfiguracji dla ról, aby kontynuować.
      
       Jeśli nie możesz zdefiniować co najmniej dwa wystąpienia każdej roli, Azure nie może zagwarantować co najmniej 99,95% dostępności usługi w chmurze podczas aktualizacji konfiguracji usługi. Aby uzyskać więcej informacji, zobacz [umowy dotyczące poziomu usług](https://azure.microsoft.com/support/legal/sla/).
   4. Kliknij przycisk **OK** (znacznikiem wyboru). 

## <a name="next-steps"></a>Następne kroki
* Dowiedz się, jak [wdrażania usługi w chmurze](cloud-services-how-to-create-deploy.md).
* Skonfiguruj [niestandardowej nazwy domeny](cloud-services-custom-domain-name.md).
* [Usługi w chmurze zarządzanie](cloud-services-how-to-manage.md).
* [Włączanie połączeń usług pulpitu zdalnego dla roli usług w chmurze Azure](cloud-services-role-enable-remote-desktop.md)
* Skonfiguruj [certyfikaty ssl](cloud-services-configure-ssl-certificate.md).

