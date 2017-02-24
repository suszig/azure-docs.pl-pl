---
title: "Wprowadzenie do obszaru roboczego usługi Azure Log Analytics | Microsoft Docs"
description: "Pracę z obszarem roboczym usługi Log Analytics można rozpocząć w kilka minut."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 508716de-72d3-4c06-9218-1ede631f23a6
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/08/2017
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: f75386f970aeb5694d226cfcd569b8c04a253191
ms.openlocfilehash: 0f418af5728b6a156ebc72fb99a3d16d559654ed


---
# <a name="get-started-with-a-log-analytics-workspace"></a>Wprowadzenie do obszaru roboczego usługi Log Analytics
Możesz szybko rozpocząć pracę z usługą Azure Log Analytics, która pomaga oceniać analizę operacyjną zbieraną z infrastruktury IT. Dzięki informacjom zawartym w tym artykule można łatwo i *bezpłatnie* rozpocząć eksplorowanie i analizowanie zbieranych danych oraz wykonywanie na nich działań.

Ten artykuł służy jako wprowadzenie do usługi Log Analytics — do tego celu zostanie użyty krótki samouczek, w którym opisano podstawowe wdrożenie na platformie Azure w celu umożliwienia korzystania z usługi. Logiczny kontener służący do przechowywania danych zarządzania na platformie Azure jest nazywany obszarem roboczym. Po przejrzeniu tych informacji i zakończeniu oceny możesz usunąć obszar roboczy dotyczący oceny. Ponieważ ten artykuł jest samouczkiem, nie zawiera on informacji dotyczących wymagań biznesowych i planowania ani wskazówek dotyczących architektury.

>[!NOTE]
>Jeśli używasz chmury platformy Microsoft Azure Government, zamiast tego użyj [dokumentacji dotyczącej monitorowania platformy Azure Government i zarządzania nią](https://review.docs.microsoft.com/azure/azure-government/documentation-government-services-monitoringandmanagement#log-analytics).

Oto krótki przegląd procesu umożliwiającego rozpoczęcie pracy:

![diagram procesu](./media/log-analytics-get-started/onboard-oms.png)

## <a name="1-create-an-azure-account-and-sign-in"></a>1. Tworzenie konta platformy Azure i logowanie

Jeśli nie masz jeszcze konta platformy Azure, należy utworzyć je, aby móc używać usługi Log Analytics. Możesz utworzyć [bezpłatne konto](https://azure.microsoft.com/free/), które jest ważne przez 30 dni i umożliwia dostęp do dowolnej usługi Azure.

### <a name="to-create-a-free-account-and-sign-in"></a>Aby utworzyć bezpłatne konto i zalogować się
1. Postępuj zgodnie z instrukcjami znajdującymi się w artykule [Utwórz bezpłatne konto platformy Azure](https://azure.microsoft.com/free/).
2. Przejdź do witryny [Azure Portal](https://portal.azure.com) i zaloguj się.

## <a name="2-create-a-workspace"></a>2. Tworzenie obszaru roboczego

Następnym krokiem jest utworzenie obszaru roboczego.

1. W witrynie Azure Portal wyszukaj listę usług w portalu Marketplace dla usługi *Log Analytics*, a następnie wybierz pozycję **Log Analytics**.  
    ![Azure Portal](./media/log-analytics-get-started/log-analytics-portal.png)
2. Kliknij pozycję **Utwórz**, a następnie wybierz opcje dla następujących elementów:
   * **Obszar roboczy pakietu OMS** — wpisz nazwę obszaru roboczego.
   * **Subskrypcja** — Jeśli masz wiele subskrypcji, wybierz tę, którą chcesz skojarzyć z nowym obszarem roboczym.
   * **Grupa zasobów**
   * **Lokalizacja**
   * **Warstwa cenowa**  
       ![szybkie tworzenie](./media/log-analytics-get-started/oms-onboard-quick-create.png)
3. Kliknij przycisk **OK**, aby wyświetlić listę swoich obszarów roboczych.
4. Wybierz obszar roboczy, aby wyświetlić jego szczegóły w witrynie Azure Portal.       
    ![szczegóły obszaru roboczego](./media/log-analytics-get-started/oms-onboard-workspace-details.png)         

## <a name="3-add-solutions-and-solution-offerings"></a>3. Dodawanie rozwiązań i ofert rozwiązań

Następnie dodaj rozwiązania do zarządzania i ofert rozwiązań. Rozwiązania to zestawy reguł logiki, wizualizacji i gromadzenia danych zawierające metryki dotyczące określonego obszaru problemów. Oferta rozwiązań to pakiet rozwiązań do zarządzania.

Dodawanie rozwiązań do obszaru roboczego umożliwia usłudze Log Analytics zbieranie różnych rodzajów danych z komputerów, które są połączone z obszarem roboczym przy użyciu agentów. Informacje dotyczące dodawania agentów zostaną przedstawione później.

### <a name="to-add-solutions-and-solution-offerings"></a>Aby dodać rozwiązania i oferty rozwiązań

1. W witrynie Azure Portal kliknij pozycję **Nowe**, a następnie w polu **Wyszukaj w portalu Marketplace** wpisz **Activity Log Analytics** i naciśnij klawisz ENTER.
2. W bloku Wszystko wybierz pozycję **Activity Log Analytics**, a następnie kliknij pozycję **Utwórz**.  
    ![Activity Log Analytics](./media/log-analytics-get-started/activity-log-analytics.png)  
3. W bloku *nazwy rozwiązania do zarządzania* wybierz obszar roboczy, który chcesz skojarzyć z rozwiązaniem do zarządzania.
4. Kliknij przycisk **Utwórz**.  
    ![obszar roboczy rozwiązania](./media/log-analytics-get-started/solution-workspace.png)  
5. Powtórz kroki 1–4, aby dodać:
    - Ofertę usług ** Security & Compliance** wraz z rozwiązaniem do oceny ochrony przed złośliwym oprogramowaniem oraz rozwiązaniem zabezpieczeń i inspekcji.
    - Ofertę usług **Automation & Control** wraz z rozwiązaniem dotyczącym hybrydowego procesu roboczego oraz rozwiązaniami do śledzenia zmian i oceny aktualizacji systemu (nazywanym również rozwiązaniem do zarządzania aktualizacjami). Podczas dodawania oferty rozwiązań należy utworzyć konto usługi Automation.  
        ![Konto usługi Automation](./media/log-analytics-get-started/automation-account.png)  
6. Rozwiązania do zarządzania dodane do obszaru roboczego można wyświetlić, przechodząc do obszaru **Log Analytics** > **Subskrypcje** > ***nazwa obszaru roboczego*** > **Przegląd**. Zostaną wyświetlone kafelki dla dodanych rozwiązań do zarządzania.  
    >[!NOTE]
    >Ponieważ z obszarem roboczymi nie zostali jeszcze połączeni żadni agenci, dla dodanych rozwiązań nie będą wyświetlane żadne dane.  

    ![kafelki rozwiązania bez danych](./media/log-analytics-get-started/solutions-no-data.png)

## <a name="4-create-a-vm-and-onboard-an-agent"></a>4. Tworzenie maszyny wirtualnej i dodawanie agenta

Następnie należy utworzyć prostą maszynę wirtualną na platformie Azure. Po utworzeniu maszyny wirtualnej należy dodać agenta pakietu OMS w celu jego włączenia. Włączenie agenta spowoduje rozpoczęcie zbierania danych z maszyny wirtualnej i wysyłanie ich do usługi Log Analytics.

### <a name="to-create-a-virtual-machine"></a>Aby utworzyć maszynę wirtualną

- Postępuj zgodnie z instrukcjami znajdującymi się w artykule [Tworzenie pierwszej maszyny wirtualnej z systemem Windows w witrynie Azure Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md), a następnie uruchom nową maszynę wirtualną.

### <a name="connect-the-virtual-machine-to-log-analytics"></a>Łączenie maszyny wirtualnej z usługą Log Analytics

- Postępuj zgodnie z instrukcjami znajdującymi się w artykule [Connect Azure virtual machines to Log Analytics](log-analytics-azure-vm-extension.md) (Łączenie maszyn wirtualnych platformy Azure z usługą Log Analytics), aby połączyć maszynę wirtualną z usługą Log Analytics przy użyciu witryny Azure Portal.

## <a name="5-view-and-act-on-data"></a>5. Wyświetlanie danych i działanie na nich

Wcześniej włączone zostało rozwiązanie Activity Log Analytics i oferty rozwiązań Security & Compliance oraz Automation & Control. Następnie można rozpocząć analizę danych zebranych przez te rozwiązania oraz wyników w wyszukiwaniach w dziennikach.

Na początku należy przyjrzeć się danym wyświetlanym w ramach rozwiązań. Następnie należy zapoznać się z wyszukiwaniami w dziennikach. Wyszukiwania w dziennikach umożliwiają łączenie i korelowanie danych dowolnej maszyny z wielu źródeł w danym środowisku. Aby uzyskać więcej informacji, zobacz [Log searches in Log Analytics](log-analytics-log-searches.md) (Wyszukiwania w dziennikach w usłudze Log Analytics). Na koniec należy wykonać działania na danych wyszukanych za pomocą portalu pakietu OMS, który znajduje się poza witryną Azure Portal.

### <a name="to-view-antimalware-data"></a>Aby wyświetlić dane oprogramowania chroniącego przed złośliwym kodem

1. W witrynie Azure Portal przejdź do obszaru **Log Analytics** > ***Twój obszar roboczy***.
2. W bloku dla Twojego obszaru roboczego w obszarze **Ogólne** kliknij pozycję **Przegląd**.  
    ![Omówienie](./media/log-analytics-get-started/overview.png)
3. Kliknij kafelek **Ocena oprogramowania chroniącego przed złośliwym kodem**. W tym przykładzie można zobaczyć, że usługa Windows Defender została zainstalowana na jednym komputerze, ale jej sygnatura jest nieaktualna.  
    ![Oprogramowanie chroniące przed złośliwym kodem](./media/log-analytics-get-started/solution-antimalware.png)
4. Na potrzeby tego przykładu w obszarze **Stan ochrony** kliknij pozycję **Nieaktualna sygnatura**, aby otworzyć wyszukiwanie w dzienniku i wyświetlić szczegółowe informacje dotyczące komputerów, które mają nieaktualne sygnatury. W tym przykładzie należy pamiętać, że komputer ma nazwę *getstarted*. Jeśli istnieją inne komputery z nieaktualnymi sygnaturami, również zostaną one wyświetlone w wynikach wyszukiwania w dzienniku.  
    ![Oprogramowanie chroniące przed złośliwym kodem jest nieaktualne](./media/log-analytics-get-started/antimalware-search.png)

### <a name="to-view-security-and-audit-data"></a>Aby wyświetlić dane dotyczące zabezpieczeń i inspekcji

1. W bloku dla Twojego obszaru roboczego w obszarze **Ogólne** kliknij pozycję **Przegląd**.  
2. Kliknij kafelek **Zabezpieczenia i inspekcja**. W tym przykładzie można zobaczyć, że występują dwa istotne problemy: na jednym komputerze brak jest krytycznych aktualizacji, a inny komputer nie ma wystarczającej ochrony.  
    ![Zabezpieczenia i inspekcja](./media/log-analytics-get-started/security-audit.png)
3. Na potrzeby tego przykładu w obszarze **Problemy godne uwagi** kliknij pozycję **Komputery bez krytycznych aktualizacji**, aby otworzyć wyszukiwanie w dzienniku i wyświetlić szczegółowe informacje o komputerach, na których brak jest krytycznych aktualizacji. W tym przykładzie brak jest jednej aktualizacji krytycznej i 63 innych aktualizacji.  
    ![Wyszukiwanie w dzienniku zabezpieczeń i inspekcji](./media/log-analytics-get-started/security-audit-log-search.png)

### <a name="to-view-and-act-on-system-update-data"></a>Aby wyświetlić dane aktualizacji systemu i działać na nich

1. W bloku dla Twojego obszaru roboczego w obszarze **Ogólne** kliknij pozycję **Przegląd**.  
2. Kliknij kafelek **Ocena aktualizacji systemu**. W tym przykładzie można zobaczyć, że istnieje jeden komputer z systemem Windows o nazwie *getstarted* wymagający aktualizacji krytycznych i jeden komputer wymagający aktualizacji definicji.  
    ![Aktualizacje systemu](./media/log-analytics-get-started/system-updates.png)
3. Na potrzeby tego przykładu w obszarze **Brakujące aktualizacje** kliknij pozycję **Aktualizacje krytyczne**, aby otworzyć wyszukiwanie w dzienniku i wyświetlić szczegółowe informacje o komputerach, na których brak jest krytycznych aktualizacji. W tym przykładzie brak jest jednej aktualizacji i istnieje jedna wymagana aktualizacja.  
    ![Wyszukiwanie w dzienniku aktualizacji systemu](./media/log-analytics-get-started/system-updates-log-search.png)
4. Przejdź do witryny sieci Web pakietu [Operations Management Suite](http://microsoft.com/oms) i zaloguj się przy użyciu konta platformy Azure. Należy zauważyć, że po zalogowaniu informacje o rozwiązaniu są podobne do tych, które można zobaczyć w witrynie Azure Portal.  
    ![Portal pakietu OMS](./media/log-analytics-get-started/oms-portal.png)
5. Kliknij kafelek **Zarządzanie aktualizacjami**.
6. Na pulpicie nawigacyjnym zarządzania aktualizacjami można zauważyć, że informacje o aktualizacji systemu są podobne do analogicznych informacji wyświetlanych w witrynie Azure Portal. Kafelek **Zarządzanie wdrożeniami aktualizacji** jest natomiast nowy. Kliknij kafelek **Zarządzanie wdrożeniami aktualizacji**.  
    ![Kafelek Zarządzanie aktualizacjami](./media/log-analytics-get-started/update-management.png)
7. Na stronie **Wdrożenia aktualizacji** kliknij pozycję **Dodaj**, aby utworzyć *przebieg aktualizacji*.  
    ![Wdrożenia aktualizacji](./media/log-analytics-get-started/update-management-update-deployments.png)
8.  Na stronie **Nowe wdrożenie aktualizacji** wpisz nazwę wdrożenia aktualizacji, wybierz komputery do zaktualizowania (w tym przykładzie *getstarted*), wybierz harmonogram, a następnie kliknij pozycję **Zapisz**.  
    ![Nowe wdrożenie](./media/log-analytics-get-started/new-deployment.png)  
    Po zapisaniu wdrożenia aktualizacji zostanie wyświetlona zaplanowana aktualizacja.  
    ![zaplanowana aktualizacja](./media/log-analytics-get-started/scheduled-update.png)  
    Po zakończeniu przebiegu aktualizacji stan jest wyświetlany jako **Zakończony**.
    ![zakończona aktualizacja](./media/log-analytics-get-started/completed-update.png)
9. Po zakończeniu przebiegu aktualizacji można wyświetlić, czy przebieg został zakończony pomyślnie, czy nie. Można także wyświetlić szczegółowe informacje dotyczące zastosowanych aktualizacji.

## <a name="after-evaluation"></a>Po dokonaniu oceny

W tym samouczku pokazano, jak zainstalować agenta na maszynie wirtualnej i szybko rozpocząć pracę. Przedstawione w nim kroki były proste i można je było szybko wykonać. Jednak większość dużych organizacji i przedsiębiorstw ma złożone lokalne infrastruktury IT. Zbieranie danych z tych złożonych środowisk wymaga zatem dodatkowego planowania i nakładu pracy w porównaniu z tym, co zostało przedstawione w ramach tego samouczka. Przejrzyj informacje w poniższej sekcji Następne kroki, aby uzyskać linki do przydatnych artykułów.

Opcjonalnie można usunąć obszar roboczy utworzony za pomocą tego samouczka.

## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej o łączeniu [agentów systemu Windows](log-analytics-windows-agents.md) z usługą Log Analytics.
* Dowiedz się więcej o łączeniu [agentów programu Operations Manager](log-analytics-om-agents.md) z usługą Log Analytics.
* [Dodaj rozwiązania Log Analytics z galerii rozwiązań](log-analytics-add-solutions.md), aby dodać funkcje i zebrać dane.
* Zapoznaj się z [wyszukiwaniem w dziennikach](log-analytics-log-searches.md), aby wyświetlić szczegółowe informacje zebrane przez rozwiązania.



<!--HONumber=Feb17_HO2-->


