---
title: "Za pomocą programu Visual Studio Azure Kreator publikowania aplikacji | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować różne ustawienia w programie Visual Studio Azure Kreator publikowania aplikacji"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 7d8f1ac9-e439-47e0-a183-0642c4ea1920
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 25b3ca9af2639860d9cfcb1492aef745fb47beb9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="using-the-visual-studio-publish-azure-application-wizard"></a>Za pomocą programu Visual Studio Azure Kreator publikowania aplikacji
Po opracowywanie aplikacji sieci web w programie Visual Studio, można opublikować tę aplikację do usługi w chmurze platformy Azure przy użyciu **publikowanie aplikacji platformy Azure** kreatora. 

> [!NOTE]
> Ten temat dotyczy wdrażania usługi w chmurze, nie do witryn sieci web. Aby uzyskać informacje o wdrażaniu do witryn sieci web, zobacz [jak wdrożyć witrynę sieci Web Azure](https://social.msdn.microsoft.com/Search/windowsazure?query=How%20to%20Deploy%20an%20Azure%20Web%20Site&Refinement=138&ac=4#refinementChanges=117&pageNumber=1&showMore=false).
> 
> 

## <a name="accessing-the-publish-azure-application-wizard"></a>Uzyskiwanie dostępu do Kreatora publikowania aplikacji Azure

Można uzyskać dostępu do kreatora Publikowanie aplikacji platformy Azure na dwa sposoby w zależności od typu projektu Visual Studio, do których masz.

**Jeśli masz projekt usługi w chmurze platformy Azure:**

1. Utwórz lub Otwórz projekt usługi w chmurze platformy Azure w programie Visual Studio.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz z menu kontekstowego **publikowania**.

**Jeśli masz projektu aplikacji sieci web, która nie jest włączona dla platformy Azure:**

1. Utwórz lub Otwórz projekt usługi w chmurze platformy Azure w programie Visual Studio.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz z menu kontekstowego **przekonwertować** > **Konwertuj na projekt usługi w chmurze Azure**. 

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nowo utworzonego projektu platformy Azure i z menu kontekstowego wybierz **publikowania**.

## <a name="sign-in-page"></a>Strona logowania

![Strona logowania](./media/vs-azure-tools-publish-azure-application-wizard/sign-in.png)

**Konto** — wybierz konto lub **Dodaj konto** na liście rozwijanej konta.

**Wybierz subskrypcję** — Wybierz subskrypcję do użycia na potrzeby wdrożenia.
   
## <a name="settings-page---common-settings-tab"></a>Ustawienia strony — karta typowe ustawienia   

![Typowe ustawienia](./media/vs-azure-tools-publish-azure-application-wizard/settings-common-settings.png)

**Usługi w chmurze** — za pomocą listy rozwijanej, albo wybierz istniejący chmurę usługi lub wybierz  **&lt;Utwórz nowy >**i Utwórz usługę w chmurze. Centrum danych są wyświetlane w nawiasach dla każdej usługi w chmurze. Zaleca się, że centrum danych lokalizacji usługi w chmurze taka sama jak lokalizacja centrum danych dla konta magazynu (Zaawansowane ustawienia).  

**Środowisko** — wybierz opcję **produkcji** lub **przemieszczania**. Wybierz środowisko przejściowe, jeśli chcesz wdrożyć aplikację w środowisku testowym. 

**Konfiguracja kompilacji** — wybierz opcję **debugowania** lub **wersji**.

**Konfiguracja usługi** — wybierz opcję **chmury** lub **lokalnego**.
   
**Włącz pulpit zdalny dla wszystkich ról** -zaznacz tę opcję, jeśli chcesz umożliwić zdalne połączenia z usługą. Ta opcja jest używana głównie do rozwiązywania problemów. Po zaznaczeniu tego pola wyboru **konfigurację usług pulpitu zdalnego** zostanie wyświetlone okno dialogowe. Wybierz **ustawienia** łącze, aby zmienić konfigurację.
   
**Włącz dla wszystkich ról w sieci web narzędzia Web Deploy** -zaznacz tę opcję, aby umożliwić wdrożenie w usłudze sieci web. Musisz wybrać **Włącz pulpit zdalny dla wszystkich ról** opcję, aby użyć tej funkcji. Aby uzyskać więcej informacji, zobacz [ [publikowania usługi w chmurze platformy Azure przy użyciu programu Visual Studio](https://msdn.microsoft.com/library/azure/ff683672.aspx)](https://msdn.microsoft.com/library/azure/ff683672.aspx). 

## <a name="settings-page---advanced-settings-tab"></a>Ustawienia strony — karta Zaawansowane ustawienia

![Ustawienia zaawansowane](./media/vs-azure-tools-publish-azure-application-wizard/settings-advanced-settings.png)

**Etykieta wdrożenia** -albo zaakceptuj nazwę domyślną lub wprowadź nazwę. Aby dołączyć daty do etykiety wdrożenia, pozostaw zaznaczone pole wyboru. 
   
**Konto magazynu** — wybierz konto magazynu do użycia dla tego wdrożenia, **&lt;Utwórz nowy > Aby utworzyć konto magazynu. Centrum danych są wyświetlane w nawiasach dla każdego konta magazynu. Zaleca się, że centrum danych lokalizacji dla konta magazynu być taka sama jak lokalizacja centrum danych dla usługi w chmurze (typowe ustawienia).  
   
Konto magazynu Azure przechowuje pakietu dla wdrożenia aplikacji. Po wdrożeniu aplikacji, pakiet zostanie usunięty z konta magazynu.

**Usuń wdrożenie w przypadku niepowodzenia** — wybierz tę opcję, aby wdrożenie usunięte, jeśli nie zostaną napotkane błędy podczas publikowania. To powinno być niezaznaczone, jeśli chcesz zachować stałego wirtualnego adresu IP dla usługi w chmurze.

**Wdrożenia aktualizacji** — wybierz tę opcję, jeśli chcesz wdrożyć tylko zaktualizowane składniki. Ten typ wdrożenia może być szybsza niż pełne wdrożenie. To sprawdzić, czy chcesz zachować stałego wirtualnego adresu IP dla usługi w chmurze. 

**Wdrożenia aktualizacji - ustawienia** — to okno dialogowe służy do dalszego określ w jaki sposób ról do zaktualizowania. Jeśli wybierzesz **aktualizacji przyrostowej**, każde wystąpienie aplikacji jest aktualizowany po kolei, tak, aby aplikacja jest zawsze dostępna. Jeśli wybierzesz **jednoczesne aktualizowanie**, wszystkich wystąpień aplikacji są aktualizowane w tym samym czasie. Jednoczesne aktualizowanie jest szybsze, ale z usługą mogą być niedostępne podczas procesu aktualizacji. 

![Ustawienia wdrożenia](./media/vs-azure-tools-publish-azure-application-wizard/deployment-settings.png)

**Włącz IntelliTrace** — Określ, czy chcesz włączyć funkcji IntelliTrace. Przy użyciu funkcji IntelliTrace można rejestrować szeroką gamę informacji o debugowaniu dla wystąpienia roli, po uruchomieniu na platformie Azure. Jeśli potrzebujesz ustalić przyczynę problemu, można użyć dzienniki IntelliTrace do wykonania kroków opisanych kodu w programie Visual Studio, tak jakby były uruchomione w systemie Azure. Aby uzyskać więcej informacji o korzystaniu z funkcji IntelliTrace, zobacz [debugowania usługi opublikowana chmura Azure za pomocą programu Visual Studio i IntelliTrace](./vs-azure-tools-intellitrace-debug-published-cloud-services.md). 

**Włącz profilowanie** — Określ, czy włączyć profilowanie wydajności. Profilera Visual Studio pozwala uzyskać dokładnych analiz obliczeniową aspektów sposób uruchamiania usługi w chmurze. Aby uzyskać więcej informacji na temat używania profilera Visual Studio, zobacz [przetestować wydajność usługi w chmurze Azure](./vs-azure-tools-performance-profiling-cloud-services.md).

**Włącz zdalny debuger dla wszystkich ról** — Określ, czy włączyć debugowanie zdalne. Aby uzyskać więcej informacji dotyczących debugowania usługi w chmurze przy użyciu programu Visual Studio, zobacz [debugowania usługi w chmurze Azure lub maszyny wirtualnej w programie Visual Studio](./vs-azure-tools-debug-cloud-services-virtual-machines.md).

## <a name="diagnostics-settings-page"></a>Strona Ustawienia diagnostyki

![Ustawienia diagnostyczne](./media/vs-azure-tools-publish-azure-application-wizard/diagnostic-settings.png)

Diagnostyka umożliwia rozwiązywanie problemów z usługą w chmurze Azure (lub maszyny wirtualnej platformy Azure). Aby uzyskać informacje diagnostyczne, zobacz [Konfigurowanie diagnostyki dla usług Azure Cloud Services i maszyn wirtualnych](./vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md). Uzyskać informacji na temat usługi Application Insights, zobacz [co to jest usługa Application Insights?](./application-insights/app-insights-overview.md).

## <a name="summary-page"></a>Strona podsumowania

![Podsumowanie](./media/vs-azure-tools-publish-azure-application-wizard/summary.png)

**Docelowa profilu** — użytkownik może utworzyć profil publikowania z wybranych ustawień. Na przykład utworzyć jeden profil dla środowiska testowego i drugą w środowisku produkcyjnym. Aby zapisać ten profil, wybierz **zapisać** ikony. Kreator tworzy profil i zapisuje go w projekcie programu Visual Studio. Aby zmodyfikować nazwę profilu, otwórz **docelowego profilu** listy, a następnie wybierz pozycję **< Zarządzaj >**.
   
   > [!NOTE]
   > Profil publikowania zostanie wyświetlony w Eksploratorze rozwiązań w programie Visual Studio, a ustawienia profilu są zapisywane w pliku z rozszerzeniem .azurePubxml. Ustawienia są zapisywane jako atrybutami tagów XML.
   > 
   > 

## <a name="publishing-your-application"></a>Publikowanie aplikacji

Po skonfigurowaniu wszystkich ustawień dla projektu wdrażania wybierz **publikowania** w dolnej części okna dialogowego. Można monitorować stan procesu w **dane wyjściowe** okna w programie Visual Studio.

## <a name="next-steps"></a>Następne kroki
- [Migrowanie i publikowanie aplikacji sieci Web do usługi w chmurze platformy Azure w programie Visual Studio](./vs-azure-tools-migrate-publish-web-app-to-cloud-service.md)
- [Dowiedz się, jak opublikować usługa w chmurze Azure za pomocą programu Visual Studio](./vs-azure-tools-publishing-a-cloud-service.md)
- [Debugowanie usługi opublikowana chmura Azure za pomocą programu Visual Studio i IntelliTrace](./vs-azure-tools-intellitrace-debug-published-cloud-services.md)
- [Testowanie wydajności usługi w chmurze Azure](./vs-azure-tools-performance-profiling-cloud-services.md)
- [Konfigurowanie diagnostyki dla usług w chmurze Azure i maszyn wirtualnych](./vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md). 
- [Co to jest Application Insights?](./application-insights/app-insights-overview.md)