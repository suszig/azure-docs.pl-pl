---
title: "Wdrażanie usługi aplikacji w środowisku w trybie offline: stosu Azure | Dokumentacja firmy Microsoft"
description: "Szczegółowe instrukcje dotyczące wdrażania usługi aplikacji w środowisku bez połączenia stosu Azure zabezpieczonej przez usługi AD FS."
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2018
ms.author: anwestg
ms.openlocfilehash: 2e527620825a3b419c0191244ba0baff4b74f0fa
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="add-an-app-service-resource-provider-to-a-disconnected-azure-stack-environment-secured-by-ad-fs"></a>Dodaj dostawcy zasobów usługi aplikacji w środowisku bez połączenia stosu Azure zabezpieczonej przez usługi AD FS
*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Postępując zgodnie z instrukcjami w tym artykule, można zainstalować [dostawcy zasobów usługi aplikacji](azure-stack-app-service-overview.md) do środowiska Azure stosu:
- nie jest połączony z Internetem
- zabezpieczone przez usługi Active Directory Federation Services (AD FS).

Aby dodać dostawcę zasobów usługi aplikacji do wdrożenia stosu Azure w trybie offline, należy wykonać te zadania najwyższego poziomu:

1. Zakończenie [wstępnie wymagane kroki](azure-stack-app-service-before-you-get-started.md) (takich jak zakupienia certyfikatów, może to zająć kilka dni otrzymasz).
2. [Pobierać i wyodrębniać pliki instalacyjne i Pomocnika](azure-stack-app-service-before-you-get-started.md) maszynie połączony z Internetem.
3. Tworzenie pakietu instalacji w trybie offline.
4. Uruchom plik Instalatora appservice.exe.

## <a name="create-an-offline-installation-package"></a>Tworzenie pakietu instalacji w trybie offline

Wdrożenie usługi aplikacji w środowisku bez połączenia, należy najpierw utworzyć pakiet instalacji w trybie offline na komputerze, na którym jest połączony z Internetem.

1. Uruchom Instalatora AppService.exe na komputerze, na którym jest połączony z Internetem.

2. Kliknij przycisk **zaawansowane** > **pakietu instalacyjnego w trybie offline Utwórz**.

    ![Instalator usługi aplikacji](media/azure-stack-app-service-deploy-offline/image01.png)   

3. Instalator usługi aplikacji — tworzy pakiet instalacji w trybie offline i wyświetla ścieżkę do niego. Możesz kliknąć **Otwórz folder** aby otworzyć folder w sieci Eksploratora plików.

    ![Instalator usługi aplikacji](media/azure-stack-app-service-deploy-offline/image02.png)   

4. Skopiuj Instalatora (AppService.exe) i pakietu instalacyjnego w trybie offline do komputera-hosta Azure stosu.

## <a name="complete-the-offline-installation-of-app-service-on-azure-stack"></a>Zakończenie instalacji w trybie offline usługi aplikacji Azure stosu

1. Na komputerze bez połączenia hosta stosu Azure Uruchom appservice.exe jako azurestack\clouadmin.

2. Kliknij przycisk **zaawansowane** > **ukończyć instalację w trybie offline**.

    ![Instalator usługi aplikacji](media/azure-stack-app-service-deploy-offline/image03.png)   

3. Przejdź do lokalizacji pakietu instalacyjnego w trybie offline, została wcześniej utworzona, a następnie kliknij przycisk **dalej**.

    ![Instalator usługi aplikacji](media/azure-stack-app-service-deploy-offline/image04.png)   

4. Przejrzyj i zaakceptuj postanowienia licencyjne dotyczące oprogramowania firmy Microsoft, a następnie kliknij przycisk **dalej**.

5. Przejrzyj i zaakceptuj postanowienia licencyjne innych firm, a następnie kliknij przycisk **dalej**.

6. Upewnij się, że informacje o konfiguracji chmury usługi aplikacji jest prawidłowa. Jeśli domyślne ustawienia są używane podczas wdrażania usługi Azure stosu Development Kit, można zaakceptować wartości domyślne w tym miejscu. Jednak po wdrożeniu stosu Azure po dostosowaniu opcji, należy edytować wartości w tym oknie, aby odzwierciedlić który. Na przykład jeśli używasz mycloud.com sufiks domeny punktu końcowego musi zmienić na management.mycloud.com. Po upewnieniu się informacje, kliknij przycisk **dalej**.

    ![Instalator usługi aplikacji](media/azure-stack-app-service-deploy/image02.png)

7. Na następnej stronie:
    1. Kliknij przycisk **Connect** znajdujący się obok **subskrypcji platformy Azure stosu** pole.
        - Jeśli używasz usługi Azure Active Directory (Azure AD), wprowadź konto administratora usługi Azure AD i hasło podane podczas wdrażania usługi Azure stosu. Kliknij przycisk **Zaloguj**.
        - Jeśli używasz programu Active Directory Federation Services (AD FS), podaj konto administratora. Na przykład cloudadmin@azurestack.local. Wprowadź hasło, a następnie kliknij przycisk **logowania**.
    2. W **subskrypcji platformy Azure stosu** Wybierz subskrypcję.
    3. W **lokalizacji stosu Azure** wybierz lokalizację, do której odnosi się do regionu jest wdrażany do. Na przykład wybierz **lokalnego** Jeśli wdrażanie Azure stosu Development Kit.
    4. Wprowadź **Nazwa grupy zasobów** wdrożenia usługi aplikacji. Domyślnie jest ustawiona **APPSERVICE lokalnego**.
    5. Wprowadź **nazwy konta magazynu** , które mają usługi aplikacji, aby utworzyć jako część instalacji. Domyślnie jest ustawiona **appsvclocalstor**.
    6. Kliknij przycisk **Dalej**.

    ![Instalator usługi aplikacji](media/azure-stack-app-service-deploy/image03.png)

8. Wprowadź informacje dotyczące udziału plików, a następnie kliknij przycisk **dalej**. Adres udziału plików musi używać w pełni kwalifikowanej nazwy domeny serwera plików, na przykład \\\appservicefileserver.local.cloudapp.azurestack.external\websites lub adres IP, na przykład \\\10.0.0.1\websites.

    ![Instalator usługi aplikacji](media/azure-stack-app-service-deploy/image04.png)

9. Na następnej stronie:
    1. W **identyfikator aplikacji tożsamości** wprowadź identyfikator GUID dla aplikacji, używane dla tożsamości.
    2. W **pliku certyfikatu tożsamości aplikacji** wprowadź (lub przejdź do) lokalizację pliku certyfikatu.
    3. W **hasło certyfikatu tożsamości aplikacji** wprowadź hasło dla certyfikatu. To hasło jest wprowadzone należy wziąć pod uwagę, gdy skrypt jest używany do tworzenia certyfikatów.
    4. W **plik certyfikatu głównego usługi Azure Resource Manager** wprowadź (lub przejdź do) lokalizację pliku certyfikatu.
    5. Kliknij przycisk **Dalej**.

    ![Instalator usługi aplikacji](media/azure-stack-app-service-deploy/image05.png)

10. Dla każdego z trzech pól pliku certyfikatu, kliknij przycisk **Przeglądaj** i przejdź do pliku odpowiedniego certyfikatu i wpisz hasło. Te certyfikaty są tymi, które zostały utworzone w [tworzenia wymaganych certyfikatów krok](azure-stack-app-service-deploy.md). Kliknij przycisk **dalej** po wprowadzeniu wszystkich informacji.

    | Box | Przykład nazwy pliku certyfikatu |
    | --- | --- |
    | **Plik certyfikatu SSL domyślne usługi aplikacji** | \_.appservice.local.AzureStack.external.pfx |
    | **Plik certyfikatu SSL usługi aplikacji interfejsu API** | api.appservice.local.AzureStack.external.pfx |
    | **Plik certyfikatu SSL wydawcy usługi aplikacji** | ftp.appservice.local.AzureStack.external.pfx |

    Jeśli sufiks domeny różnych są używane podczas tworzenia certyfikatów, nie używaj nazwy pliku certyfikatu *lokalnego. AzureStack.external*. Zamiast tego należy używać Twoich informacji domeny niestandardowej.

    ![Instalator usługi aplikacji](media/azure-stack-app-service-deploy/image06.png)    

11. Podaj szczegóły programu SQL Server dla wystąpienia serwera, używane do obsługi baz danych dostawcy zasobów usługi aplikacji, a następnie kliknij przycisk **dalej**. Instalator sprawdza właściwości połączenia SQL.

    ![Instalator usługi aplikacji](media/azure-stack-app-service-deploy/image07.png)    

12. Przejrzyj opcje jednostki SKU i wystąpienia roli. Wartości domyślne są wypełniane przy użyciu minimalną liczbę wystąpień i minimalna jednostki SKU dla każdej roli we wdrożeniu ASDK. Aby ułatwić planowanie rozmieszczenia udostępniane jest podsumowanie wymagań vCPU i pamięci. Po dokonaniu wyboru kliknij przycisk **dalej**.

     > [!NOTE]
     > W przypadku wdrożeń produkcyjnych, postępując zgodnie ze wskazówkami w [zaplanować role serwera usługi Azure App Service w stosie Azure wydajność](azure-stack-app-service-capacity-planning.md).
     >
     >

    | Rola | Minimalna wystąpień | Minimalna jednostki SKU | Uwagi |
    | --- | --- | --- | --- |
    | Kontroler | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Zarządza i obsługuje kondycji chmury usługi aplikacji. |
    | Zarządzanie | 1 | Standard_A2 - (2 Vcpu, 3584 MB) | Zarządza punktów końcowych aplikacji usługi Azure Resource Manager i interfejsu API, rozszerzenia portalu (administratora, dzierżawcy, funkcje portalu) i usługi danych. Do obsługi trybu failover, zwiększyć zalecane wystąpień na 2. |
    | Wydawca | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Publikowanie zawartości za pośrednictwem sieci web i FTP wdrożenia. |
    | FrontEnd | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Kieruje żądania do aplikacji usługi aplikacji. |
    | Udostępnionego procesu roboczego | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Hosty aplikacji sieci web lub aplikacji interfejsu API i usługi Azure Functions. Można dodać więcej wystąpień. Uprawnienia operatora można zdefiniować Twojej oferty i wybierz warstwę wszystkie jednostki SKU. Warstw musi mieć co najmniej jeden vCPU. |

    ![Instalator usługi aplikacji](media/azure-stack-app-service-deploy/image08.png)    

    > [!NOTE]
    > **Podstawowy Windows Server 2016 nie jest obrazem obsługiwanych platform do użycia z usługi Azure App Service na stosie Azure**.

13. W **wybierz obrazu platformy** wybierz obraz maszyny wirtualnej wdrażania systemu Windows Server 2016 z tych, które są dostępne w dostawcy zasobów obliczeniowych w chmurze usługi aplikacji. Kliknij przycisk **Dalej**.

14. Na następnej stronie:
     1. Wprowadź hasło i nazwa użytkownika administratora maszyny wirtualnej w roli procesu roboczego.
     2. Wprowadź nazwę użytkownika administratora maszyny wirtualnej inne role i hasło.
     3. Kliknij przycisk **Dalej**.

    ![Instalator usługi aplikacji](media/azure-stack-app-service-deploy/image09.png)    

15. Na stronie Podsumowanie:
    1. Sprawdź wybrane opcje wprowadzone. Aby wprowadzić zmiany, należy użyć **Wstecz** przycisków, aby przejść do poprzedniej strony.
    2. Jeśli konfiguracje są poprawne, zaznacz pole wyboru.
    3. Aby uruchomić wdrożenie, kliknij przycisk **dalej**.

    ![Instalator usługi aplikacji](media/azure-stack-app-service-deploy/image10.png)    

16. Na następnej stronie:
    1. Śledź postęp instalacji. Usługi aplikacji Azure stosu trwa około 60 minut do wdrożenia na podstawie wybranych domyślne.
    2. Po pomyślnym zakończeniu Instalatora, kliknij przycisk **zakończenia**.

    ![Instalator usługi aplikacji](media/azure-stack-app-service-deploy/image11.png)    


## <a name="validate-the-app-service-on-azure-stack-installation"></a>Sprawdź poprawność usługi aplikacji Azure stosu instalacji

1. W portalu administracyjnym stosu Azure, przejdź do **Administracja - App Service**.

2. W przeglądzie w stanie, sprawdź, który **stan** pokazuje **wszystkie role są gotowe**.

    ![Zarządzanie usługami aplikacji](media/azure-stack-app-service-deploy/image12.png)    


## <a name="test-drive-app-service-on-azure-stack"></a>Przetestuj aplikację usługi Azure stosu

Po wdrażanie i rejestrowanie dostawcy zasobów usługi aplikacji, przetestować go, aby upewnić się, że użytkownicy będą mogli wdrażać sieci web i aplikacji API apps.

> [!NOTE]
> Należy utworzyć ofertę, która ma przestrzeń nazw Microsoft.Web w planie. Następnie należy mieć subskrypcję dzierżawy, która subskrybuje tej oferty. Aby uzyskać więcej informacji, zobacz [Utwórz oferty](azure-stack-create-offer.md) i [Utwórz plan](azure-stack-create-plan.md).
>
Możesz *musi* mających subskrypcję dzierżawy do tworzenia aplikacji korzystających z usługi aplikacji Azure stosu. Tylko możliwości, które administrator usługi można wykonać w portalu administracyjnym dotyczących administracji dostawcy zasobów usługi App Service. Te funkcje obejmują dodanie pojemności, konfigurowanie źródła wdrożenia i dodawanie warstwy procesu roboczego i jednostki SKU.
>
Począwszy od trzeciego technical preview można utworzyć sieci web, interfejsu API i Azure funkcje aplikacji, należy użyć portalu dzierżawcy i ma subskrypcję dzierżawy.

1. W portalu Azure stosu dzierżawy kliknij **nowy** > **sieci Web i mobilność** > **aplikacji sieci Web**.

2. Na **aplikacji sieci Web** bloku, wpisz nazwę w **aplikacji sieci Web** pole.

3. W obszarze **grupy zasobów**, kliknij przycisk **nowy**. Wpisz nazwę w **grupy zasobów** pole.

4. Kliknij pozycję **Plan/lokalizacja usługi App Service** > **Utwórz nowy**.

5. Na **planu usługi aplikacji** bloku, wpisz nazwę w **planu usługi aplikacji** pole.

6. Kliknij przycisk **warstwa cenowa** > **Shared wolne** lub **Shared Shared** > **wybierz**  >   **OK** > **utworzyć**.

7. W obszarze minuty kafelków dla nowej aplikacji sieci web pojawi się na pulpicie nawigacyjnym. Kliknij Kafelek.

8. Na **aplikacji sieci Web** bloku, kliknij przycisk **Przeglądaj** można wyświetlić domyślną witrynę sieci Web dla tej aplikacji.

## <a name="deploy-a-wordpress-dnn-or-django-website-optional"></a>Wdrażanie witryny WordPress, DNN lub Django (opcjonalnie)

1. W portalu Azure stosu dzierżawy kliknij  **+** , przejdź do portalu Azure Marketplace, wdrożyć witrynę sieci Web Django i poczekaj na pomyślne zakończenie. Platforma sieci web Django korzysta z bazy danych w oparciu o system plików. Nie wymaga żadnych dostawców dodatkowych zasobów, takich jak SQL lub MySQL.

2. Jeśli wdrożono również dostawcy zasobów MySQL, można wdrażać witryny sieci Web WordPress z poziomu portalu Marketplace. Gdy zostanie wyświetlony monit o parametry bazy danych, wprowadź nazwę użytkownika jako  *User1@Server1* , z nazwą użytkownika i nazwę serwera.

3. Jeśli wdrożono również dostawcy zasobów programu SQL Server, można wdrożyć DNN witryny sieci Web z witryny Marketplace. Gdy zostanie wyświetlony monit o parametry bazy danych, wybierz bazę danych na komputerze z uruchomionym programem SQL Server, połączoną z dostawcą zasobów.

## <a name="next-steps"></a>Kolejne kroki

Możesz również wypróbować innych [platforma jako usługa (PaaS) usługi](azure-stack-tools-paas-services.md).

- [Dostawcy zasobów programu SQL Server](azure-stack-sql-resource-provider-deploy.md)
- [Dostawca zasobów MySQL](azure-stack-mysql-resource-provider-deploy.md)

<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[App_Service_Deployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525
