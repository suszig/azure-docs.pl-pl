---
title: "Jak utworzyć i wdrożyć usługi w chmurze | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie tworzenia i wdrażania usługi w chmurze przy użyciu metody szybkie tworzenie w systemie Azure."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 0ea78ccc-5e7d-40f8-bdb6-478c0eb0e265
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 2a2172a78bfd3ac923edbc9de366b035629dd27b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-and-deploy-a-cloud-service"></a>Jak utworzyć i wdrożyć usługi w chmurze
> [!div class="op_single_selector"]
> * [Witryna Azure Portal](cloud-services-how-to-create-deploy-portal.md)
> * [Klasyczna witryna Azure Portal](cloud-services-how-to-create-deploy.md)
> 
> 

Klasyczny portal Azure udostępnia dwa sposoby tworzenia i wdrażania usługi w chmurze: **szybkie tworzenie** i **Utwórz niestandardowy**.

W tym temacie wyjaśniono, jak utworzyć nową usługę w chmurze, a następnie użyj przy użyciu metody szybkie tworzenie **przekazać** Aby przekazać i wdrożyć pakiet usługi chmury na platformie Azure. Korzystając z tej metody, klasycznym portalu Azure sprawia, że dostępne wygodne łącza ukończenia wszystkich wymagań w trakcie pracy. Jeśli wszystko jest gotowe do wdrożenia usługi w chmurze podczas jego tworzenia, można wykonać jednocześnie na tym samym czasie przy użyciu **Utwórz niestandardowy**.

> [!NOTE]
> Jeśli planujesz opublikować usługi w chmurze z programu Visual Studio Team Services (VSTS), użyj **szybkie tworzenie**, a następnie skonfigurować publikowanie programu VSTS z **Szybki Start** lub do pulpitu nawigacyjnego.
> 
> 

## <a name="concepts"></a>Pojęcia
Aby wdrożyć aplikację jako usługa w chmurze na platformie Azure wymagane są trzy składniki:

* **Definicja usługi**  
  Chmura pliku definicji usługi (csdef) definiuje modelu usługi, w tym liczba ról.
* **Konfiguracja usługi**  
  Plik konfiguracji usługi chmury (cscfg) zawiera ustawienia konfiguracji dla chmury usługi i poszczególne role, w tym liczbę wystąpień roli.
* **Pakiet usługi**  
  Pakiet usługi (cspkg) zawiera kod aplikacji i konfiguracji i pliku definicji usługi.

Dowiedz się więcej na temat tych i tworzenie pakietu [tutaj](cloud-services-model-and-package.md).

## <a name="prepare-your-app"></a>Przygotowanie aplikacji
Przed wdrożeniem usługi w chmurze, należy utworzyć pakiet usługi chmury (cspkg) w kodzie aplikacji i pliku konfiguracji usługi chmury (cscfg). Zestaw Azure SDK oferuje narzędzia przygotowania te pliki wymagane wdrożenie. Można zainstalować zestawu SDK [Azure pobiera](https://azure.microsoft.com/downloads/) strony w języku wolisz do opracowywania kod aplikacji.

Trzy funkcje usługi chmury wymagają specjalnych konfiguracji przed wyeksportowaniem pakietu usług:

* Jeśli ma zostać wdrożona usługa w chmurze, która używa protokołu Secure Sockets Layer (SSL) do szyfrowania danych [konfigurowania aplikacji](cloud-services-configure-ssl-certificate.md#step-2-modify-the-service-definition-and-configuration-files) dla protokołu SSL.
* Jeśli chcesz skonfigurować połączenia pulpitu zdalnego z wystąpień roli [konfigurowania ról](cloud-services-role-enable-remote-desktop.md) dla pulpitu zdalnego.
* Jeśli chcesz skonfigurować pełne, monitorowanie dla usługi w chmurze, należy włączyć diagnostyki Azure dla usługi w chmurze. *Monitorowanie minimalnego* (domyślne monitorowania poziomu) używa liczniki wydajności zebrane z hosta systemów operacyjnych dla wystąpień roli (maszyny wirtualne). "Monitorowanie pełne * zbiera dodatkowe metryki na podstawie danych wydajności w ramach wystąpienia roli, aby umożliwić analizę bliżej problemów występujących podczas przetwarzania aplikacji. Aby dowiedzieć się, jak włączyć diagnostyki Azure, zobacz [Włączanie diagnostyki na platformie Azure](cloud-services-dotnet-diagnostics.md).

Aby utworzyć usługi w chmurze z wdrożeniami ról sieć web i proces roboczy ról, należy [Utwórz pakiet usługi](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Przed rozpoczęciem
* Jeśli nie zainstalowano zestaw SDK usługi Azure, kliknij przycisk **Zainstaluj zestaw Azure SDK** otworzyć [strona plików do pobrania usługi Azure](https://azure.microsoft.com/downloads/), a następnie pobrać zestaw SDK dla języka wolisz tworzenia kodu. (Będziesz mieć możliwość zrobić to później).
* Jeśli wszystkie wystąpienia roli jest wymagany certyfikat, należy utworzyć certyfikaty. Usługi w chmurze wymagają pliku pfx z kluczem prywatnym. Możesz [Przekaż certyfikaty Azure](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) podczas tworzenia i wdrażania usługi w chmurze.
* Jeśli planujesz wdrożyć usługę w chmurze do grupy koligacji, należy utworzyć grupy koligacji. Grupy koligacji służy do wdrażania usługi w chmurze i innymi usługami Azure w tej samej lokalizacji, w regionie. Można utworzyć grupy koligacji w **sieci** obszaru klasyczny Portal Azure portalu na **grup koligacji** strony.

## <a name="how-to-create-a-cloud-service-using-quick-create"></a>Porady: Tworzenie usługi w chmurze przy użyciu szybkie tworzenie
1. W [klasycznego portalu Azure](http://manage.windowsazure.com/), kliknij przycisk **nowy**>**obliczeniowe**>**usługi w chmurze** > **Szybkie tworzenie**.
   
    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_QuickCreate.png)
2. W **adres URL**, wprowadź nazwę domeny podrzędnej do użycia w publiczny adres URL do uzyskiwania dostępu do usługi w chmurze we wdrożeniach produkcyjnych. Format adresu URL wdrożeń produkcyjnych jest: http://*myURL*. cloudapp.net.
3. W **Region lub grupę koligacji**, wybierz region geograficzny lub grupy koligacji, aby wdrożyć w usłudze chmury. Wybierz grupy koligacji, jeśli chcesz wdrożyć usługi w chmurze do tej samej lokalizacji co innymi usługami Azure w obrębie regionu.
4. Kliknij pozycję **Utwórz usługę w chmurze**.
   
    ![CloudServices_Region](./media/cloud-services-how-to-create-deploy/CloudServices_Regionlist.png)
   
    Stan procesu można monitorować w obszarze wiadomości w dolnej części okna.
   
    **Usługi w chmurze** powoduje otwarcie obszaru z nową usługę w chmurze wyświetlane. Stan zmieni się na utworzone, tworzenia usługi w chmurze została zakończona pomyślnie.
   
    ![CloudServices_CloudServicesPage](./media/cloud-services-how-to-create-deploy/CloudServices_CloudServicesPage.png)

## <a name="how-to-upload-a-certificate-for-a-cloud-service"></a>Porady: przekazywanie certyfikatu dla usługi w chmurze
1. W [klasycznego portalu Azure](http://manage.windowsazure.com/), kliknij przycisk **usługi w chmurze**, kliknij nazwę usługi w chmurze, a następnie kliknij przycisk **certyfikaty**.
   
    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_EmptyDashboard.png)
2. Kliknij opcję **przekazać certyfikat** lub **przekazać**.
3. W **pliku**, użyj **Przeglądaj** umożliwia wybranie certyfikatu (pfx).
4. W **hasło**, wprowadź klucz prywatny dla certyfikatu.
5. Kliknij przycisk **OK** (znacznikiem wyboru).
   
    ![CloudServices_AddaCertificate](./media/cloud-services-how-to-create-deploy/CloudServices_AddaCertificate.png)
   
    Możesz obserwować postęp przekazywania w obszarze wiadomości pokazano poniżej. Po zakończeniu przekazywania, certyfikat zostanie dodany do tabeli. W obszarze wiadomości kliknij przycisk OK, aby zamknąć komunikat.
   
    ![CloudServices_CertificateProgress](./media/cloud-services-how-to-create-deploy/CloudServices_CertificateProgress.png)

## <a name="how-to-deploy-a-cloud-service"></a>Porady: Wdrażanie usługi w chmurze
1. W [klasycznego portalu Azure](http://manage.windowsazure.com/), kliknij przycisk **usługi w chmurze**, kliknij nazwę usługi w chmurze, a następnie kliknij przycisk **pulpitu nawigacyjnego**.
2. Kliknij opcję **przekazania nowego wdrożenia produkcyjnego** lub **przekazać**.
3. W **etykieta wdrożenia**, wprowadź nazwę dla nowego wdrożenia — na przykład MyCloudServicev4.
4. W **pakietu**, użyj **Przeglądaj** i wybierz plik pakietu usługi (cspkg) do użycia.
5. W **konfiguracji**, użyj **Przeglądaj** i wybierz plik konfiguracji usługi (cscfg) do użycia.
6. Jeśli usługa w chmurze będzie zawierać żadnych ról z tylko jednym wystąpieniem, zaznacz **Wdróż, nawet jeśli co najmniej jedna rola zawiera pojedyncze wystąpienie** pole wyboru, aby umożliwić wdrożenie, aby kontynuować.
   
    Każdy rola ma co najmniej dwa wystąpienia Azure tylko może zagwarantować 99,95% dostępu do usługi w chmurze podczas konserwacji i obsługi aktualizacji. W razie potrzeby można dodać wystąpienia dodatkowych ról na **skali** strony po wdrożeniu usługi w chmurze. Aby uzyskać więcej informacji, zobacz [umowy dotyczące poziomu usług](https://azure.microsoft.com/support/legal/sla/).
7. Kliknij przycisk **OK** (zaznaczone), aby rozpocząć wdrażanie usługi chmury.
   
    ![CloudServices_UploadaPackage](./media/cloud-services-how-to-create-deploy/CloudServices_UploadaPackage.png)
   
    Można monitorować stan wdrożenia w obszarze wiadomości. Kliknij przycisk OK, aby ukryć wiadomości.
   
    ![CloudServices_UploadProgress](./media/cloud-services-how-to-create-deploy/CloudServices_UploadProgress.png)

## <a name="verify-your-deployment-completed-successfully"></a>Sprawdź wdrożenia ukończone pomyślnie
1. Kliknij przycisk **pulpitu nawigacyjnego**.
   
    Stan powinny wskazywać, że usługa jest **systemem**.
2. W obszarze **szybkiego dostępu**, kliknij adres URL witryny, aby otworzyć usługi w chmurze w przeglądarce sieci web.
   
    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy/CloudServices_QuickGlance.png)


## <a name="next-steps"></a>Następne kroki
* [Konfiguracja ogólna usługi w chmurze](cloud-services-how-to-configure.md).
* Skonfiguruj [niestandardowej nazwy domeny](cloud-services-custom-domain-name.md).
* [Usługi w chmurze zarządzanie](cloud-services-how-to-manage.md).
* Skonfiguruj [certyfikaty ssl](cloud-services-configure-ssl-certificate.md).

