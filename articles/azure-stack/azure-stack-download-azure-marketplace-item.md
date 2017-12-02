---
title: "Pobieranie elementów marketplace z platformy Azure | Dokumentacja firmy Microsoft"
description: "Można pobrać elementów marketplace z platformy Azure do mojego wdrożenia stosu Azure."
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/30/2017
ms.author: erikje
ms.openlocfilehash: 33b7be4a85723ab03e4c656a8dd28632ad854e29
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/02/2017
---
# <a name="download-marketplace-items-from-azure-to-azure-stack"></a>Pobieranie elementów marketplace z platformy Azure do stosu Azure

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Jak zdecydujesz zawartość do uwzględnienia w Twojej stosu Azure marketplace, należy wziąć pod uwagę zawartości z portalu Azure marketplace. Możesz pobrać wyselekcjonowanych listy elementów witrynę Azure marketplace, które zostały przetestowane na stosie Azure. Nowe elementy często są dodawane do tej listy, dlatego upewnij się, że sprawdzanie dostępności nową zawartość.

## <a name="download-marketplace-items-in-a-connected-scenario-with-internet-connectivity"></a>Pobieranie elementów marketplace w przypadku połączonych (z połączeniem internetowym)

1. Aby pobrać elementów portalu marketplace, należy najpierw [zarejestrować stosu Azure w usłudze Azure](azure-stack-register.md). 
2. Zaloguj się do portalu administratora platformy Azure stosu (https://portal.local.azurestack.external).
3. Niektóre elementy marketplace mogą być duże. Upewnij się, że masz wystarczającą ilość miejsca w systemie, klikając **dostawców zasobów** > **magazynu**.

    ![](media/azure-stack-download-azure-marketplace-item/image01.png)

4. Kliknij przycisk **więcej usług** > **zarządzania Marketplace**.

    ![](media/azure-stack-download-azure-marketplace-item/image02.png)

4. Kliknij przycisk **Dodaj z platformy Azure** umożliwia wyświetlenie listy dostępnych do pobrania elementów. Możesz kliknąć na każdy element na liście, aby wyświetlić jej opis i rozmiar pliku.

    ![](media/azure-stack-download-azure-marketplace-item/image03.png)

5. Wybierz element na liście, a następnie kliknij przycisk **Pobierz**. Spowoduje to uruchomienie pobierania obrazu maszyny Wirtualnej dla wybranego elementu. Czasu pobierania różnią się.

    ![](media/azure-stack-download-azure-marketplace-item/image04.png)

6. Po zakończeniu pobierania, można wdrożyć z nowy element marketplace jako operator stosu Azure lub użytkownika. Kliknij przycisk **+ nowy**, przeszukiwanie kategorie nowy element marketplace, a następnie wybierz element.
7. Kliknij przycisk **Utwórz** otwarcie środowisko tworzenia dla nowo pobrany element. Postępuj zgodnie z instrukcjami krok po kroku w celu wdrożenia tego elementu.

## <a name="download-marketplace-items-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity"></a>Pobieranie elementów marketplace w odłączone lub częściowo połączonego scenariusza (ograniczone z łącznością z Internetem)

Podczas wdrażania usługi Azure stosu w trybie rozłączonym (bez żadnych łączności z Internetem), nie można pobrać elementów marketplace przy użyciu portalu Azure stosu. Jednak można pobrać elementów marketplace na komputerze, na którym ma połączenie z Internetem za pomocą narzędzia zespolonego marketplace i przesyła je do środowiska Azure stosu. 

### <a name="prerequisites"></a>Wymagania wstępne
Przed użyciem narzędzia zespolonego marketplace, upewnij się, że masz [zarejestrowany stosu Azure w ramach subskrypcji Azure](azure-stack-register.md).  

Na komputerze, który ma łączność z Internetem wykonaj następujące kroki, aby pobrać elementów marketplace wymagane:

1. Otwórz konsolę programu PowerShell jako administrator i [zainstalować określone moduły programu PowerShell Azure stosu](azure-stack-powershell-install.md). Upewnij się, że instalujesz program **środowiska PowerShell w wersji 1.2.11 lub nowszej**.  

2. Dodaj konto platformy Azure, używany do rejestrowania stosu Azure. Aby to zrobić, uruchom **Add-AzureRmAccount** polecenia cmdlet bez parametrów. Zostanie wyświetlony monit o wprowadzenie poświadczeń konta platformy Azure i może być konieczne użycie uwierzytelniania wieloskładnikowego 2 na podstawie konfiguracji Twoje konto.  

3. Jeśli masz wiele subskrypcji, uruchom następujące polecenie, aby wybrać używanego do rejestracji:  

   ```powershell
   Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   $AzureContext = Get-AzureRmContext
   ```

4. Pobierz najnowszą wersję narzędzia zespolonego marketplace przy użyciu następującego skryptu:  

   ```PowerShell
   # Download the tools archive.
   invoke-webrequest https://github.com/Azure/AzureStack-Tools/archive/vnext.zip `
     -OutFile vnext.zip

   # Expand the downloaded files.
   expand-archive vnext.zip `
     -DestinationPath . `
     -Force

   # Change to the tools directory.
   cd \AzureStack-Tools-vnext

   ```

5. Zaimportuj moduł zespolonego i uruchom narzędzie, uruchamiając następujące polecenia:  

   ```powershell
   Import-Module .\ Syndication\AzureStack.MarketplaceSyndication.psm1

   Sync-AzSOfflineMarketplaceItem `
     -destination “<Destination folder path>” `
     -AzureTenantID $AzureContext.Tenant.TenantId `
     -AzureSubscriptionId $AzureContext.Subscription.Id  
   ```

6. Po uruchomieniu zostanie wyświetlony monit o podanie poświadczeń konta platformy Azure. Zaloguj się do konta platformy Azure, używany do rejestrowania stosu Azure. Po logowanie zakończy się pomyślnie, powinien zostać wyświetlony następujący ekran z listy elementów marketplace dostępne.  

   ![Azure podręcznego elementów Marketplace](./media/azure-stack-download-azure-marketplace-item/image05.png)

7. Wybierz obraz, który chcesz pobrać (możesz wybrać wiele obrazów, przytrzymując klawisz Ctrl) i zanotuj wersję obrazu, ta wersja zostanie użyty do zaimportowania obrazu w następnej sekcji > kliknij **Ok** > Zaakceptuj postanowienia prawne klikając **tak**. Można również filtrować listy obrazów za pomocą **Dodaj kryteria** opcji. Pobieranie zajmie trochę czasu w zależności od rozmiaru obrazu. Raz do pobrania obrazu jest dostępny w ścieżce docelowej podane wcześniej. Pobieranie zawiera elementy plików i galerii wirtualnego dysku twardego w formacie Azpkg.  

### <a name="import-the-image-and-publish-it-to-azure-stack-marketplace"></a>Importowanie obrazu i opublikować ją w stosie Azure marketplace

1. Po pobraniu pakietu obrazu & galerii, zapisz je i zawartość w folderze vnext-AzureStack — narzędzia do stacji dysków wymiennych i skopiować go do środowiska Azure stosu (możesz skopiować go do lokalnie do dowolnej lokalizacji takich jak: "C:\MarketplaceImages".)   

2. Przed zaimportowaniem obrazu, musisz połączyć środowiska operator stosu Azure przy użyciu procedury opisanej w [konfigurowania środowiska PowerShell Azure stosu operator](azure-stack-powershell-configure-admin.md).  

3. Zaimportuj obraz do stosu Azure za pomocą polecenia cmdlet Add-AzsVMImage. Korzystając z tego polecenia cmdlet, upewnij się zastąpić wydawcy, oferty i inne wartości parametru z wartościami właściwości obrazu, który jest importowany. Możesz uzyskać "publisher", "oferty" i "sku" wartości obrazu z elementu imageReference obiektu pobranego wcześniej pliku Azpkg i wartość "version" w kroku 6 w poprzedniej sekcji.

   ```json
   "imageReference": {
      "publisher": "MicrosoftWindowsServer",
      "offer": "WindowsServer",
      "sku": "2016-Datacenter-Server-Core"
    }
   ```

   Zastąp wartości parametru, a następnie uruchom następujące polecenie:

   ```powershell
   Import-Module .\ComputeAdmin\AzureStack.ComputeAdmin.psm1

   Add-AzsVMImage `
    -publisher "MicrosoftWindowsServer" `
    -offer "WindowsServer" `
    -sku "2016-Datacenter-Server-Core" `
    -osType Windows `
    -Version "2017.09.25" `
    -OsDiskLocalPath "C:\AzureStack-Tools-master\Syndication\Microsoft.WindowsServer2016DatacenterServerCore-ARM-Eval.2017.09.25.vhd" `
    -CreateGalleryItem $False `
    -Location Local 
   ```

4. Użyj portalu można przekazać elementu z witryny Marketplace (. Azpkg) do magazynu obiektów Blob platformy Azure stosu. Można przekazać do lokalnego magazynu Azure stosu lub Przekaż do magazynu Azure. (Jest tymczasowej lokalizacji pakietu). Upewnij się, że obiekt blob jest publicznie i Zanotuj identyfikator URI.  

5. Publikowanie elementu portalu marketplace stos Azure przy użyciu **AzureRMGalleryItem Dodaj**. Na przykład:

   ```powershell
   Add-AzsGalleryItem `
     -GalleryItemUri "https://mystorageaccount.blob.local.azurestack.external/cont1/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.2.azpkg" `
     –Verbose
   ```

6. Po opublikowaniu elementu galerii, możesz je wyświetlić z **nowy** > **Marketplace** okienka.  

   ![Portal Marketplace](./media/azure-stack-download-azure-marketplace-item/image06.png)

## <a name="next-steps"></a>Następne kroki

[Tworzenie i publikowanie elementu portalu Marketplace](azure-stack-create-and-publish-marketplace-item.md)
