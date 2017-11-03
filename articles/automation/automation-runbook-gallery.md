---
title: "Galerie elementów Runbook i modułów w automatyzacji Azure | Dokumentacja firmy Microsoft"
description: "Elementy Runbook i modułów firmy Microsoft i społecznością są dostępne, można zainstalować i używać w środowisku usługi Automatyzacja Azure.  W tym artykule opisano, jak można dostęp do tych zasobów i do ich współtworzenia elementy runbook do galerii."
services: automation
documentationcenter: 
author: eslesar
manager: jwhit
editor: tysonn
ms.assetid: d3fee7b4-630a-4c10-8425-9bf51d7c9e58
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/29/2017
ms.author: magoedte;bwren
ms.openlocfilehash: e46c09a0cfecf07514f85b5801129fbd8162510f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="runbook-and-module-galleries-for-azure-automation"></a>Galeria elementów Runbook i modułów dla usługi Automatyzacja Azure
Zamiast tworzenia własnych elementów runbook i modułów w automatyzacji Azure, można uzyskać dostępu do szerokiej gamy scenariuszy, które zostały już utworzone przez firmę Microsoft i społecznością.  Możesz użyć tych scenariuszy bez żadnych modyfikacji lub można ich używać jako punktu wyjścia i edytować je do swoich specyficznych wymagań.

Można pobrać elementów runbook z [galerię elementów Runbook](#runbooks-in-runbook-gallery) i modułów [galerii programu PowerShell](#modules-in-powerShell-gallery).  Również może przyczynić się do społeczności za pomocą udostępniania scenariusze, które można rozwijać.

## <a name="runbooks-in-runbook-gallery"></a>Elementy Runbook w galerii elementu Runbook
[Galerię elementów Runbook](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=RootCategory&f\[0\].Value=WindowsAzure&f\[1\].Type=SubCategory&f\[1\].Value=WindowsAzure_automation&f\[1\].Text=Automation) z firmy Microsoft oraz społeczności, który można zaimportować do usługi Automatyzacja Azure jest dostępnych wiele elementów runbook. Możesz pobrać elementu runbook z galerii, który jest obsługiwany w [Centrum skryptów w witrynie TechNet](https://gallery.technet.microsoft.com/scriptcenter/site/upload), lub bezpośrednio można zaimportować elementy runbook z galerii z klasycznego portalu Azure lub w portalu Azure.

Można importować tylko bezpośrednio z poziomu galerii elementu Runbook za pomocą portalu Azure lub klasycznego portalu Azure. Nie można wykonać tej funkcji za pomocą środowiska Windows PowerShell.

> [!NOTE]
> Należy sprawdzić, czy zawartość żadnych elementów runbook uzyskanie galerię elementów Runbook i należy zachować szczególną ostrożność przy instalacji i uruchamiania ich w środowisku produkcyjnym. |
> 
> 

### <a name="to-import-a-runbook-from-the-runbook-gallery-with-the-azure-classic-portal"></a>Aby zaimportować element runbook z galerii elementu Runbook z klasycznego portalu Azure
1. W portalu Azure kliknij przycisk, **nowy**, **usługi aplikacji**, **automatyzacji**, **Runbook**, **z galerii**.
2. Wybierz kategorię, aby wyświetlić powiązane elementy runbook, a następnie wybierz element runbook, aby wyświetlić jego szczegóły. Po wybraniu elementu runbook, który ma przycisk strzałki w prawo.
   
    ![Galeria elementów Runbook](media/automation-runbook-gallery/runbook-gallery.png)
3. Przejrzyj zawartość elementu runbook i zapisz wszelkie wymagania w opisie. Gdy wszystko będzie gotowe, kliknij przycisk strzałki w prawo.
4. Wprowadź szczegóły elementu runbook, a następnie kliknij przycisk znacznika wyboru. Nazwa elementu runbook są już wypełnione.
5. Element runbook jest wyświetlany na **elementów Runbook** karty dla konta automatyzacji.

### <a name="to-import-a-runbook-from-the-runbook-gallery-with-the-azure-portal"></a>Aby zaimportować element runbook z galerii elementu Runbook w portalu Azure
1. W witrynie Azure Portal otwórz konto usługi Automation.
2. Kliknij kafelek **Elementy Runbook**, aby otworzyć listę elementów Runbook.
3. Kliknij przycisk **galerii przeglądania** przycisku.
   
    ![Galeria przycisk Przeglądaj](media/automation-runbook-gallery/browse-gallery-button.png)
4. Zlokalizuj mają i zaznacz go, aby wyświetlić jego szczegóły elementu galerii.
   
    ![Przejrzyj galerię](media/automation-runbook-gallery/browse-gallery.png)
5. Polecenie **widoku Projekt źródłowy** do wyświetlania elementu w [Centrum skryptów w witrynie TechNet](http://gallery.technet.microsoft.com/).
6. Aby zaimportować element, kliknij go, aby wyświetlić jego szczegóły, a następnie kliknij przycisk **zaimportować** przycisku.
   
    ![Przycisk Importuj](media/automation-runbook-gallery/gallery-item-detail.png)
7. Opcjonalnie można zmienić nazwy elementu runbook, a następnie kliknij przycisk **OK** można zaimportować elementu runbook.
8. Element runbook jest wyświetlany na **elementów Runbook** karty dla konta automatyzacji.

### <a name="adding-a-runbook-to-the-runbook-gallery"></a>Dodawanie elementu runbook do galerii elementu runbook
Firma Microsoft zaleca dodanie elementów runbook do galerii elementu Runbook, które uważasz, że powinien być przydatny dla innych klientów.  Można dodać elementu runbook przez [przekazać go do Centrum skryptów](http://gallery.technet.microsoft.com/site/upload) biorąc pod uwagę następujące informacje.

* Należy określić *systemu Windows Azure* dla **kategorii** i *automatyzacji* dla **podkategorii** dla elementu runbook, które mają być wyświetlane w Kreator.  
* Przekazywanie musi być jednego pliku .ps1 lub graphrunbook.  Jeśli element runbook wymaga żadnych modułów podrzędnych elementów runbook i zasoby, następnie dodaj w opisie przesyłania, a także w sekcji komentarzy elementu runbook.  Jeśli scenariusz wymagające wielu elementów runbook, Przekaż każdego oddzielnie i wyświetlić listę nazw powiązanych elementów runbook w każdym z opisami. Należy upewnić się, że te same znaczniki, aby one widoczne w tej samej kategorii. Użytkownik będzie musiał opis, aby sprawdzić, czy inne elementy runbook są wymagane do odczytu się zrealizować scenariusz.
* Dodaj tag "GraphicalPS", w przypadku publikowania **graficznym elementem runbook** (nie graficzny przepływ pracy). 
* Włóż wstawki kodu programu PowerShell lub przepływ pracy programu PowerShell przy użyciu opisu **Wstawianie kodu sekcji** ikony.
* Podsumowanie dla przekazywania jest wyświetlana w wynikach galerię elementów Runbook, należy podać szczegółowe informacje, które pomaga w identyfikacji funkcji elementu runbook użytkownika.
* Jednej do trzech z następujących tagów, należy przypisać do przekazywania.  Element runbook jest wyświetlana w Kreatorze w kategoriach, spełniających jego tagów.  Dowolne tagi nie ma na liście, są ignorowane przez kreatora. Jeśli nie określono żadnych pasujących tagów, elementu runbook znajduje się w innych kategorii.
  
  * Tworzenie kopii zapasowych
  * Zarządzanie pojemnością
  * Zmiana kontroli
  * Zgodność
  * Deweloperów i testowania środowisk
  * Odzyskiwanie po awarii
  * Monitorowanie
  * Stosowanie poprawek
  * Inicjowanie obsługi
  * Korygowania
  * Zarządzanie cyklem życia maszyny Wirtualnej
* Automatyzacja aktualizuje galerii raz na godzinę, nie będą widzieć Twój wkład natychmiast.

## <a name="modules-in-powershell-gallery"></a>Moduły w galerii programu PowerShell
Moduły programu PowerShell zawierają polecenia cmdlet, których można używać w elementach runbook, a istniejące modułów, które można zainstalować w automatyzacji Azure są dostępne w [galerii programu PowerShell](http://www.powershellgallery.com).  Można uruchomić tej galerii z portalu Azure i zainstaluj je bezpośrednio do usługi Automatyzacja Azure, lub można je pobrać i zainstalować je ręcznie.  Nie można zainstalować moduły bezpośrednio z klasycznego portalu Azure, ale można je pobrać je zainstalować, jak w przypadku innych modułów.

### <a name="to-import-a-module-from-the-automation-module-gallery-with-the-azure-portal"></a>Aby zaimportować moduł z galerii moduł usługi Automatyzacja z portalu Azure
1. W witrynie Azure Portal otwórz konto usługi Automation.
2. Wybierz **modułów** w obszarze **udostępnione zasoby** aby otworzyć listę modułów.
4. Kliknij przycisk **galerii przeglądania** w górnej części strony.
   
    ![Galeria modułów](media/automation-runbook-gallery/modules-blade.png) <br>
5. Na **galerii przeglądania** strony, można wyszukiwać według następujących pól:
   
   * Nazwa modułu
   * Tagi
   * Autor
   * Nazwa zasobu polecenia cmdlet/DSC
6. Znajdź moduł, który chcesz i zaznacz go, aby wyświetlić jego szczegóły.  
   Podczas przechodzenia do określonego modułu można wyświetlić więcej informacji o module łącze do galerii programu PowerShell, w tym wszystkie wymagane zależności i wszystkich poleceń cmdlet i/lub DSC zasobów, które zawiera moduł.
   
    ![Szczegóły modułu programu PowerShell](media/automation-runbook-gallery/gallery-item-details-blade.png) <br>
7. Aby zainstalować moduł bezpośrednio do usługi Automatyzacja Azure, kliknij przycisk **importu** przycisku.
   
    ![Przycisk Importuj moduł](media/automation-runbook-gallery/module-import-button.png)
8. Po kliknięciu przycisk Importuj **zaimportować** okienku, zobacz nazwy modułu, które chcesz zaimportować. Jeśli wszystkie zależności są zainstalowane, **OK** przycisk jest aktywny. Jeśli brakuje zależności, należy zaimportować te przed zaimportowaniem tego modułu.
9. Kliknij przycisk **OK** zaimportować modułu. Podczas automatyzacji Azure importuje moduł do swojego konta, wyodrębnianie metadanych o module i polecenia cmdlet.
   
    ![Importowanie modułu strony](media/automation-runbook-gallery/module-import-blade.png)
   
    Może to potrwać kilka minut, ponieważ każde działanie ma zostać wyodrębniony.
10. Pojawi się początkowym powiadomieniu, czy moduł jest wdrażany i kolejne powiadomienie po zakończeniu.
11. Po zaimportowaniu modułu widać działań dostępnych i użyciem zasobów w elementy runbook i konfiguracji żądanego stanu.

## <a name="requesting-a-runbook-or-module"></a>Żądanie runbook lub modułu
Możesz wysłać żądania do [User Voice](https://feedback.azure.com/forums/246290-azure-automation/).  Jeśli konieczne pomocy zapisywania elementu runbook lub masz pytania dotyczące programu PowerShell, Zadaj pytanie na naszych [forum](http://social.msdn.microsoft.com/Forums/windowsazure/en-US/home?forum=azureautomation&filter=alltypes&sort=lastpostdesc).

## <a name="next-steps"></a>Następne kroki
* Aby rozpocząć pracę z elementami runbook, zobacz [Tworzenie lub importowanie elementu runbook automatyzacji Azure](automation-creating-importing-runbook.md)
* Aby poznać różnice między środowiska PowerShell i przepływ pracy programu PowerShell z elementami runbook, zobacz [przepływu pracy programu PowerShell Learning](automation-powershell-workflow.md)

