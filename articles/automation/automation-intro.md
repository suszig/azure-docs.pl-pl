---
title: "Co to jest usługa Azure Automation | Microsoft Docs"
description: "Dowiedz się, jakie korzyści oferuje usługa Azure Automation, i uzyskaj odpowiedzi na typowe pytania, aby móc rozpocząć tworzenie przy użyciu elementów Runbook i konfiguracji DSC usługi Azure Automation."
services: automation
documentationcenter: 
author: eslesar
manager: jwhit
editor: 
keywords: "co to jest usługa automation, azure automation, przykłady usługi azure automation"
ms.assetid: 0cf1f3e8-dd30-4f33-b52a-e148e97802a9
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/10/2016
ms.author: magoedte;bwren
ms.openlocfilehash: c9f5c346350a62540ba29444b1b00cc9dc398d56
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-automation-overview"></a>Omówienie usługi Azure Automation
Usługa Microsoft Azure Automation umożliwia użytkownikom automatyzowanie wykonywanych ręcznie, czasochłonnych, podatnych na błędy i często powtarzanych zadań, które najczęściej są wykonywane w chmurze i środowisku przedsiębiorstwa. Można dzięki niej zaoszczędzić czas i zwiększyć niezawodność regularnie wykonywanych zadań administracyjnych, a nawet zaplanować ich wykonywanie w regularnych odstępach czasu. Można automatyzować procesy za pomocą elementów Runbook lub automatyzować zarządzanie konfiguracją za pomocą konfiguracji żądanego stanu. Ten artykuł zawiera krótkie omówienie usługi Azure Automation i odpowiedzi na często zadawane pytania. Aby uzyskać szczegółowe informacje dotyczące innych tematów, możesz zapoznać się z innymi artykułami w tej bibliotece.

## <a name="automating-processes-with-runbooks"></a>Automatyzacja procesów przy użyciu elementów Runbook
Element Runbook to zestaw zadań wykonujących niektóre zautomatyzowane procesy w usłudze Azure Automation. Może być to prosty proces, taki jak uruchamianie maszyny wirtualnej i tworzenie wpisu dziennika. Możesz również korzystać ze złożonego elementu Runbook, który łączy mniejsze elementy Runbook w celu wykonania złożonego procesu dotyczącego wielu zasobów, a nawet wielu chmur i środowisk lokalnych.  

Możesz na przykład korzystać z ręcznego procesu obcinania rozmiaru bazy danych SQL w przypadku zbliżania się do rozmiaru maksymalnego. Proces ten składa się z wielu kroków, takich jak łączenie z serwerem, łączenie z bazą danych, pobieranie bieżącego rozmiaru bazy danych, sprawdzanie, czy próg został przekroczony, oraz powiadamianie użytkownika. Zamiast ręcznie wykonywać każdy z tych kroków, można utworzyć element Runbook, który umożliwi wykonanie wszystkich tych zadań w ramach pojedynczego procesu. Wystarczy uruchomić element Runbook, podać wymagane informacje, takie jak nazwa serwera SQL, nazwa bazy danych i adres e-mail odbiorcy, a potem czekać na zakończenie procesu. 

## <a name="what-can-runbooks-automate"></a>Co można automatyzować za pomocą elementów Runbook?
Elementy Runbook w usłudze Azure Automation są oparte na programie Windows PowerShell lub przepływie pracy programu Windows PowerShell, dlatego mogą wykonywać wszystkie czynności programu PowerShell. Jeśli aplikacja lub usługa ma interfejs API, element Runbook może z nim współpracować. Jeśli masz moduł programu PowerShell dla aplikacji, możesz załadować ten moduł do usługi Azure Automation i uwzględnić polecenia cmdlet w elemencie Runbook. Elementy Runbook usługi Azure Automation działają w chmurze platformy Azure i mają dostęp do wszelkich zasobów w chmurze lub zasobów zewnętrznych dostępnych z chmury. Za pomocą [hybrydowemu procesowi roboczemu elementu Runbook](automation-hybrid-runbook-worker.md) elementy Runbook można uruchamiać w lokalnym centrum danych w celu zarządzania zasobami lokalnymi. 

## <a name="getting-runbooks-from-the-community"></a>Uzyskiwanie elementów Runbook od społeczności
[Galeria elementów Runbook](automation-runbook-gallery.md#runbooks-in-runbook-gallery) zawiera elementy Runbook udostępniane przez firmę Microsoft oraz przez społeczność. Można je stosować bez zmian w środowisku lub dostosowywać do własnych celów. Są one również przydatne jako odwołania do informacji o sposobie tworzenia własnych elementów Runbook. Możesz nawet przekazywać własne elementy Runbook do galerii, jeśli uważasz, że mogą się przydać innym użytkownikom. 

## <a name="creating-runbooks-with-azure-automation"></a>Tworzenie elementów Runbook przy użyciu usługi Azure Automation
Możesz [tworzyć własne elementy Runbook](automation-creating-importing-runbook.md) od początku lub modyfikować elementy z [galerii elementów Runbook](http://msdn.microsoft.com/library/azure/dn781422.aspx) zgodnie z własnymi wymaganiami. Istnieją cztery [typy elementów Runbook](automation-runbook-types.md), które można wybierać w oparciu o własne wymagania oraz program PowerShell. Jeśli wolisz pracować bezpośrednio z kodem programu PowerShell, możesz użyć [elementu Runbook programu PowerShell](automation-runbook-types.md#powershell-runbooks) lub [elementu Runbook przepływu pracy programu PowerShell](automation-runbook-types.md#powershell-workflow-runbooks) edytowanego w trybie offline lub w [edytorze tekstów](http://msdn.microsoft.com/library/azure/dn879137.aspx) w portalu Azure. Jeśli wolisz edytować element Runbook bez ujawniania w kodzie podstawowym, możesz utworzyć [graficzny element Runbook](automation-runbook-types.md#graphical-runbooks) przy użyciu [edytora graficznego](automation-graphical-authoring-intro.md) w portalu Azure. 

Wolisz obejrzeć film niż przeczytać artykuł? Obejrzyj poniższy film z nagraniem sesji konferencji Microsoft Ignite, która odbyła się w maju 2015 r. Uwaga: pojęcia i funkcje omówione w tym filmie są poprawne, ale od momentu jego nagrania usługa Azure Automation znacznie się rozwinęła i ma teraz obszerniejszy interfejs użytkownika w portalu Azure oraz obsługuje dodatkowe funkcje.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3451/player]
> 
> 

## <a name="automating-configuration-management-with-desired-state-configuration"></a>Automatyzacja zarządzania konfiguracją za pomocą konfiguracji żądanego stanu
[PowerShell DSC](https://technet.microsoft.com/library/dn249912.aspx) to platforma zarządzania, która umożliwia wdrażanie i wymuszanie konfiguracji dla hostów fizycznych i maszyn wirtualnych oraz zarządzanie nimi przy użyciu deklaratywnej składni programu PowerShell. Konfiguracje można definiować na centralnym serwerze ściągania konfiguracji DSC, a maszyny docelowe mogą je pobierać i stosować. Konfiguracja DSC udostępnia zestaw poleceń cmdlet programu PowerShell, które służą do zarządzania konfiguracjami i zasobami.  

[Konfiguracja DSC usługi Azure Automation](automation-dsc-overview.md) to oparte na chmurze rozwiązanie dla konfiguracji PowerShell DSC, które świadczy usługi wymagane w środowiskach przedsiębiorstw.  Możesz zarządzać zasobami konfiguracji DSC w usłudze Azure Automation i stosować konfiguracje na maszynach wirtualnych lub fizycznych, które pobierają je z serwera ściągania konfiguracji DSC w chmurze platformy Azure.  Rozwiązanie to udostępnia również usługi raportowania, które informują Cię o ważnych zdarzeniach, takich jak odchylenie węzłów od ich przypisanej konfiguracji lub zastosowanie nowej konfiguracji. 

## <a name="creating-your-own-dsc-configurations-with-azure-automation"></a>Tworzenie własnych konfiguracji DSC przy użyciu funkcji Azure Automation
[Konfiguracje DSC](automation-dsc-overview.md) określają żądany stan węzła.  Można stosować taką samą konfigurację w wielu węzłach, by dzięki temu zyskać pewność, że będą mieć identyczny stan.  Konfigurację można utworzyć przy użyciu dowolnego edytora tekstów na komputerze lokalnym, a następnie zaimportować ją do usługi Azure Automation w celu jej skompilowania i zastosowania w węzłach.

## <a name="getting-modules-and-configurations"></a>Pobieranie modułów i konfiguracji
[Moduły programu PowerShell](automation-runbook-gallery.md#modules-in-powershell-gallery) zawierające polecenia cmdlet do użycia w elementach Runbook i konfiguracjach DSC można pobrać z [galerii programu PowerShell](http://www.powershellgallery.com/). Tę galerię można uruchomić w portalu Azure i zaimportować moduły bezpośrednio do usługi Azure Automation albo pobrać i zaimportować je ręcznie. Nie można zainstalować modułów bezpośrednio z portalu Azure, ale można je pobrać i zainstalować tak jak każdy inny moduł. 

## <a name="example-practical-applications-of-azure-automation"></a>Przykłady praktycznych zastosowań usługi Azure Automation
Poniżej przedstawiono kilka przykładów scenariuszy automatyzacji przy użyciu usługi Azure Automation. 

* Tworzenie i kopiowanie maszyn wirtualnych w ramach różnych subskrypcji platformy Azure. 
* Planowanie tworzenia kopii plików z komputera lokalnego w kontenerze usługi Azure Blob Storage. 
* Automatyzowanie funkcji zabezpieczeń, takich jak odrzucanie żądań od klientów po wykryciu ataku typu „odmowa usługi”. 
* Sprawdzanie, czy maszyny działają zgodnie ze skonfigurowanymi zasadami zabezpieczeń.
* Zarządzanie ciągłym wdrażaniem kodu aplikacji w chmurze i infrastrukturze lokalnej. 
* Kompilowanie lasu usługi Active Directory na platformie Azure na potrzeby środowiska laboratoryjnego. 
* Obcinanie tabeli w bazie danych SQL, jeśli rozmiar bazy danych zbliża się do wartości maksymalnej. 
* Zdalne aktualizowanie ustawień środowiska dla witryny sieci Web platformy Azure. 

## <a name="how-does-azure-automation-relate-to-other-automation-tools"></a>Jaki związek ma usługa Azure Automation z innymi narzędziami do automatyzacji?
Usługa [Service Management Automation (SMA)](http://technet.microsoft.com/library/dn469260.aspx) jest przeznaczona do automatyzacji zadań zarządzania w chmurze prywatnej. Jest ona instalowana lokalnie w centrum danych klienta jako składnik [pakietu Microsoft Azure Pack](https://www.microsoft.com/en-us/server-cloud/). Usługi SMA i Azure Automation korzystają z tego samego formatu elementu Runbook opartego na programie Windows PowerShell i przepływie pracy programu Windows PowerShell, ale usługa SMA nie obsługuje [graficznych elementów Runbook](automation-graphical-authoring-intro.md).  

Program [System Center 2012 Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) jest przeznaczony do automatyzacji zasobów lokalnych. Używa on innego formatu elementów Runbook niż usługi Azure Automation i Service Management Automation oraz ma interfejs graficzny umożliwiający tworzenie elementów Runbook bez konieczności pisania skryptów. Jego elementy Runbook składają się z działań z pakietów integracyjnych napisanych specjalnie dla programu Orchestrator. 

## <a name="where-can-i-get-more-information"></a>Gdzie można uzyskać więcej informacji?
Aby dowiedzieć się więcej na temat usługi Azure Automation i tworzenia własnych elementów Runbook, możesz skorzystać z różnych zasobów. 

* **Biblioteka usługi Azure Automation** to miejsce, w którym znajdujesz się w tej chwili. Artykuły w tej bibliotece stanowią pełną dokumentację konfigurowania usługi Azure Automation i administrowania nią, a także tworzenia własnych elementów Runbook. 
* [Polecenia cmdlet programu Azure PowerShell](http://msdn.microsoft.com/library/jj156055.aspx) oferują informacje wymagane do automatyzacji operacji platformy Azure wykonywanych za pomocą programu Windows PowerShell. Elementy Runbook używają tych poleceń cmdlet do pracy z zasobami platformy Azure. 
* [Blog dotyczący zarządzania](https://azure.microsoft.com/blog/tag/azure-automation/) zawiera najnowsze informacje o usłudze Azure Automation i innych technologiach zarządzania oferowanych przez firmę Microsoft. Jeśli chcesz być na bieżąco z najnowszymi informacjami od zespołu ds. usługi Azure Automation, subskrybuj ten blog. 
* [Forum usługi Automation](http://go.microsoft.com/fwlink/p/?LinkId=390561) pozwala na publikowanie pytań dotyczących usługi Azure Automation, skierowanych do firmy Microsoft i społeczności usługi Automation. 
* [Polecenia cmdlet usługi Azure Automation](https://msdn.microsoft.com/library/mt244122.aspx) oferują informacje dotyczące automatyzacji zadań administracyjnych. Są to polecenia cmdlet służące do zarządzania kontami usługi Automation, elementami zawartości, elementami Runbook i konfiguracjami DSC.

## <a name="can-i-provide-feedback"></a>Czy mogę przekazać opinię?
**Przekaż nam swoją opinię.** Jeśli szukasz rozwiązania dotyczącego elementów Runbook usługi Azure Automation lub modułu integracji, opublikuj żądanie skryptu w Centrum skryptów. Jeśli chcesz przekazać opinię na temat żądań funkcji usługi Azure Automation, opublikuj ją w witrynie [User Voice](http://feedback.windowsazure.com/forums/34192--general-feedback). Dziękujemy. 

