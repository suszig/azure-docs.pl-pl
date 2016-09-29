<properties
    pageTitle="Wprowadzenie do usługi Log Analytics | Microsoft Azure"
    description="Możesz rozpocząć pracę z usługą Log Analytics w pakiecie Microsoft Operations Management Suite (OMS) w ciągu kilku minut."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/09/2016"
    ms.author="banders"/>



# Wprowadzenie do usługi Log Analytics

Możesz rozpocząć pracę z usługą Log Analytics w pakiecie Microsoft Operations Management Suite (OMS) w ciągu kilku minut. Podobnie jak w przypadku konta, obszar roboczy OMS można utworzyć na dwa sposoby:

- Witryna sieci Web pakietu Microsoft Operations Management Suite
- Subskrypcja platformy Microsoft Azure

Bezpłatny obszar roboczy OMS można utworzyć za pomocą witryny sieci Web pakietu OMS. Obszar roboczy OMS można też utworzyć, korzystając z subskrypcji platformy Microsoft Azure. Oba obszary robocze są funkcjonalnie równoważne z tą różnicą, że z bezpłatnego obszaru roboczego OMS można wysłać do pakietu OMS tylko 500 MB danych dziennie. Jeśli używasz subskrypcji platformy Azure, możesz także użyć tej subskrypcji w celu uzyskania dostępu do innych usług Azure. Niezależnie od metody użytej do utworzenia obszaru roboczego, utworzysz obszar roboczy z kontem Microsoft lub kontem organizacyjnym.

Oto diagram tego procesu:

![diagram dołączania](./media/log-analytics-get-started/oms-onboard-diagram.png)

## Wymagania wstępne dla usługi Log Analytics i zagadnienia dotyczące wdrażania

- Aby móc w pełni korzystać z usługi Log Analytics, należy mieć płatną subskrypcję platformy Microsoft Azure. Jeśli nie masz subskrypcji platformy Azure, utwórz [bezpłatne konto](https://azure.microsoft.com/free/) umożliwiające dostęp do dowolnej usługi Azure. Możesz też utworzyć bezpłatne konto OMS w witrynie sieci Web pakietu [Operations Management Suite](http://microsoft.com/oms) i kliknąć pozycję **Wypróbuj bezpłatnie**.
- Obszar roboczy OMS
- Każdy komputer z systemem Windows, z którego mają być zbierane dane, musi działać pod kontrolą systemu Windows Server 2008 z dodatkiem SP1 lub nowszym.
- Dostęp przez [zaporę](log-analytics-proxy-firewall.md) do adresów usługi sieci Web pakietu OMS
- Serwer [usługi przesyłania dalej dla usługi Log Analytics pakietu OMS](https://blogs.technet.microsoft.com/msoms/2016/03/17/oms-log-analytics-forwarder) (bramy) do przesyłania dalej ruchu sieciowego z serwerów do pakietu OMS, jeśli z komputerów nie ma dostępu do Internetu.
- Jeśli korzystasz z programu Operations Manager, usługa Log Analytics obsługuje program Operations Manager 2012 SP1 UR6 i nowszy oraz program Operations Manager 2012 R2 UR2 i nowszy. Obsługa serwera proxy została dodana w programach Operations Manager 2012 SP1 UR7 i Operations Manager 2012 R2 UR3. Określ sposób integracji tego programu z pakietem OMS.
- Określ, czy komputery mają bezpośredni dostęp do Internetu. Jeśli nie, to potrzebują one serwera bramy na potrzeby uzyskiwania dostępu do witryn usługi sieci Web pakietu OMS. Wszelki dostęp jest realizowany za pośrednictwem protokołu HTTPS.
- Określ, z jakich technologii i serwerów będziesz korzystać przy wysyłaniu danych do pakietu OMS. Są to na przykład kontrolery domeny, oprogramowanie SQL Server itd.
- Przyznaj uprawnienie użytkownikom w pakiecie OMS i na platformie Azure.
- Jeśli wykorzystanie danych jest sprawą istotną, wdróż każde rozwiązanie oddzielnie i przed dodaniem kolejnych rozwiązań przetestuj jego wpływ na wydajność.
- Obserwuj wykorzystanie danych i wydajność podczas dodawania rozwiązań i funkcji do usługi Log Analytics. Obejmuje to zbieranie zdarzeń, zbieranie danych dziennika, zbieranie danych wydajności itd. Na początek zaleca się użycie minimalnej kolekcji do czasu określenia wpływu na wykorzystanie danych i wydajność.
- Upewnij się, że agenci dla systemu Windows nie są także zarządzani przy użyciu programu Operations Manager, ponieważ może to spowodować zduplikowanie danych. Dotyczy to również agentów opartych na platformie Azure, w przypadku których włączono usługę Diagnostyka Azure.
- Po zainstalowaniu agentów upewnij się, że działają prawidłowo. Jeśli tak nie jest, sprawdź, czy izolacja klucza CNG (Cryptography API: Next Generation) nie została wyłączona za pomocą zasad grupy.
- Niektóre rozwiązania usługi Log Analytics mają dodatkowe wymagania.



## Tworzenie konta w 3 krokach przy użyciu pakietu Operations Management Suite

1. Przejdź do witryny sieci Web pakietu [Operations Management Suite](http://microsoft.com/oms) i kliknij pozycję **Wypróbuj bezpłatnie**. Zaloguj się za pomocą konta Microsoft, takiego jak konto usługi Outlook.com, lub za pomocą konta organizacyjnego udostępnionego przez Twoją firmę bądź instytucję edukacyjną na potrzeby korzystania z usługi Office 365 lub innych usług firmy Microsoft.
2. Podaj unikatową nazwę obszaru roboczego. Obszar roboczy to logiczny kontener do przechowywania danych zarządzania. Za jego pomocą można rozdzielić dane między różne zespoły w organizacji, ponieważ dane są przypisane do obszaru roboczego na zasadzie wyłączności. Podaj adres e-mail i region, w którym chcesz przechowywać dane.  
    ![tworzenie obszaru roboczego i łączenie subskrypcji](./media/log-analytics-get-started/oms-onboard-create-workspace-link01.png)
3. Następnie możesz utworzyć nową subskrypcję platformy Azure lub połączenie z istniejącą subskrypcją platformy Azure. Jeśli chcesz kontynuować, korzystając z bezpłatnej wersji próbnej, kliknij pozycję **Nie teraz**.  
  ![tworzenie obszaru roboczego i łączenie subskrypcji](./media/log-analytics-get-started/oms-onboard-create-workspace-link02.png)

Możesz już zacząć korzystać z portalu Operations Management Suite.

Aby uzyskać więcej informacji o konfigurowaniu obszaru roboczego i łączeniu istniejących kont platformy Azure z obszarami roboczymi utworzonymi za pomocą pakietu Operations Management Suite, zobacz [Manage access to Log Analytics](log-analytics-manage-access.md) (Zarządzanie dostępem do usługi Log Analytics).

## Szybkie tworzenie konta za pomocą platformy Microsoft Azure

1. Przejdź do witryny [Azure Portal](https://portal.azure.com) i zaloguj się. Następnie przejrzyj listę usług i wybierz pozycję **Log Analytics (OMS)**.  
    ![Azure Portal](./media/log-analytics-get-started/oms-onboard-azure-portal.png)
2. Kliknij przycisk **Dodaj**, a następnie wybierz opcje dla następujących elementów:
    - Nazwa **obszaru roboczego OMS**
    - **Subskrypcja** — Jeśli masz wiele subskrypcji, wybierz tę, którą chcesz skojarzyć z nowym obszarem roboczym.
    - **Grupa zasobów**
    - **Lokalizacja**
    - **Warstwa cenowa**  
        ![szybkie tworzenie](./media/log-analytics-get-started/oms-onboard-quick-create.png)
3. Kliknij pozycję **Utwórz**. W witrynie Azure Portal zostaną wyświetlone szczegóły obszaru roboczego.       
    ![szczegóły obszaru roboczego](./media/log-analytics-get-started/oms-onboard-workspace-details.png)         
4. Kliknij link **Portal OMS**, aby otworzyć witrynę sieci Web pakietu Operations Management Suite z nowym obszarem roboczym.

Możesz zacząć korzystać z portalu Operations Management Suite.

Aby uzyskać więcej informacji o konfigurowaniu obszaru roboczego i łączeniu istniejących obszarów roboczych utworzonych za pomocą pakietu Operations Management Suite z subskrypcjami platformy Azure, zobacz [Manage access to Log Analytics](log-analytics-manage-access.md) (Zarządzanie dostępem do usługi Log Analytics).

## Rozpoczynanie pracy w portalu Operations Management Suite
Aby wybrać rozwiązania i nawiązać połączenie z serwerami, które mają być zarządzane, kliknij kafelek **Ustawienia** i wykonaj kroki opisane w tej sekcji.  

![rozpoczynanie pracy](./media/log-analytics-get-started/oms-onboard-get-started.png)  

- **Dodaj rozwiązania** — Wybierz rozwiązania, z których chcesz korzystać, a następnie kliknij przycisk **Dodaj wybrane rozwiązania**.  
    ![rozwiązania](./media/log-analytics-get-started/oms-onboard-solutions.png)
- **Połącz źródło danych** — Wybierz sposób nawiązywania połączenia ze środowiskiem serwera na potrzeby zbierania danych:
    - Zainstaluj agenta, aby bezpośrednio połączyć dowolne serwery lub dowolnych klientów systemu Windows.
    - Użyj programu System Center Operations Manager w celu dołączenia grup zarządzania lub całego wdrożenia programu Operations Manager.
    - Użyj konta usługi Azure Storage, dla którego skonfigurowano rozszerzenie maszyny wirtualnej Diagnostyki Azure dla systemu Windows lub Linux.
        ![źródła danych](./media/log-analytics-get-started/oms-onboard-data-sources.png)    
- **Dodaj dzienniki** — Skonfiguruj co najmniej jedno źródło danych, a następnie wybierz pozycję **Zapisz**. W przypadku dzienników zdarzeń można określić typ komunikatów do monitorowania (błąd, ostrzeżenie lub informacja).    

    ![dzienniki](./media/log-analytics-get-started/oms-onboard-logs.png)    


## Opcjonalne bezpośrednie łączenie serwerów z pakietem Operations Management Suite przez zainstalowanie agenta
1. Kliknij kafelek **Ustawienia**, kliknij kartę **Połączone źródła**, a następnie kliknij pozycję **Pobierz agenta dla systemu Windows** zgodną z architekturą komputera, na którym chcesz przeprowadzić instalację. Agenta można zainstalować tylko w systemie Windows Server 2008 z dodatkiem SP 1 lub nowszym oraz w systemie Windows 7 z dodatkiem SP1 lub nowszym.
2. Zainstaluj agenta na co najmniej jednym serwerze. Agentów można instalować pojedynczo lub za pomocą zautomatyzowanego [niestandardowego skryptu](log-analytics-windows-agents.md). Można też użyć innego dowolnego rozwiązania do dystrybucji oprogramowania.
3. Po wyrażeniu zgody na warunki umowy licencyjnej i wybraniu folderu instalacji wybierz pozycję **Połącz agenta z programem Microsoft Azure Operational Insights**. (Pakiet OMS był wcześniej nazywany programem Operational Insights).  
    ![konfigurowanie agenta](./media/log-analytics-get-started/oms-onboard-agent.png)

4. Na następnej stronie zostanie wyświetlony monit o podanie identyfikatora i klucza obszaru roboczego. Identyfikator i klucz obszaru roboczego są wyświetlane na ekranie, z którego został pobrany plik agenta.  
    ![klucze agenta](./media/log-analytics-get-started/oms-onboard-mma-keys.png)
    ![dołączanie serwerów](./media/log-analytics-get-started/oms-onboard-key.png)
5. Podczas instalacji możesz kliknąć przycisk **Zaawansowane**, aby opcjonalnie skonfigurować serwer proxy i podać informacje uwierzytelniania. Kliknij przycisk **Dalej**, aby powrócić do ekranu informacji o obszarze roboczym.
6. Kliknij przycisk **Dalej**, aby zweryfikować identyfikator i klucz obszaru roboczego. Jeśli zostaną znalezione jakiekolwiek błędy, można kliknąć przycisk **Wstecz** i dokonać poprawek. Po zweryfikowaniu identyfikatora i klucza obszaru roboczego kliknij przycisk **Zainstaluj**, aby ukończyć instalację agenta.
7. Zaloguj się ponownie do portalu Operations Management Suite i kliknij kafelek **Ustawienia** na stronie Przegląd. Kiedy agent nawiąże komunikację z usługą Operations Management Suite, zostanie wyświetlona ikona z zielonym znacznikiem wyboru. Początkowo trwa to około 5–10 minut.

>[AZURE.NOTE] Rozwiązania dotyczące zarządzania pojemnością i oceny konfiguracji nie są obecnie obsługiwane przez serwery podłączone bezpośrednio do pakietu Operations Management Suite.


Agenta można także połączyć z programem System Center Operations Manager 2012 z dodatkiem SP1 lub nowszym. W tym celu wybierz opcję **Połącz agenta z programem System Center Operations Manager**. Po wybraniu tej opcji dane będą wysyłane do usługi bez konieczności instalowania dodatkowego sprzętu czy ładowania grup zarządzania.

Aby uzyskać więcej informacji o łączeniu agentów z pakietem Operations Management Suite, zobacz [Connect Windows computers to Log Analytics](log-analytics-windows-agents.md) (Łączenie komputerów z systemem Windows z usługą Log Analytics).

## Opcjonalne łączenie serwerów za pomocą programu System Center Operations Manager

1. W konsoli programu Operations Manager wybierz pozycję **Administracja**.
2. Rozwiń węzeł **Operational Insights** i wybierz pozycję **Połączenie z programem Operational Insights**.

  >[AZURE.NOTE] W zależności od używanego pakietu zbiorczego aktualizacji oprogramowania SCOM może być wyświetlany węzeł *System Center Advisor*, *Operational Insights* lub *Operations Management Suite*.

3. Kliknij link **Zarejestruj się w usłudze Operational Insights** znajdujący się w prawym górnym rogu, a następnie postępuj zgodnie z instrukcjami.
4. Po ukończeniu pracy Kreatora rejestracji kliknij link **Dodaj komputer/grupę**.
5. W oknie dialogowym **Wyszukiwanie komputerów** można przeprowadzić wyszukiwanie komputerów lub grup monitorowanych przez program Operations Manager. Wybierz komputery lub grupy, które chcesz dołączyć do usługi Log Analytics, kliknij przycisk **Dodaj**, a następnie przycisk **OK**. Możesz sprawdzić, czy usługa OMS odbiera dane, przechodząc na kafelek **Użycie** w portalu Operations Management Suite. Dane powinny się pojawić po około 5–10 minutach.

Aby uzyskać więcej informacji o łączeniu programu Operations Manager z pakietem Operations Management Suite, zobacz [Connect Operations Manager to Log Analytics](log-analytics-om-agents.md) (Łączenie programu Operations Manager z usługą Log Analytics).

## Opcjonalne analizowanie danych z usług Cloud Services na platformie Microsoft Azure

Za pomocą pakietu Operations Management Suite można szybko przeszukiwać dzienniki zdarzeń i dzienniki serwera IIS pod kątem usług Cloud Services i Virtual Machines. W tym celu wystarczy włączyć diagnostykę usług Azure Cloud Services. Po zainstalowaniu programu Microsoft Monitoring Agent można też uzyskiwać dodatkowe informacje na temat usługi Azure Virtual Machines. Aby uzyskać więcej informacji o sposobie konfigurowania środowiska platformy Azure na potrzeby korzystania z pakietu Operations Management Suite, zobacz [Connect Azure storage to Log Analytics](log-analytics-azure-storage.md) (Łączenie usługi Azure Storage z usługą Log Analytics).


## Następne kroki

- [Dodaj rozwiązania Log Analytics z galerii rozwiązań](log-analytics-add-solutions.md), aby dodać funkcje i zebrać dane.
- Zapoznaj się z [wyszukiwaniem w dziennikach](log-analytics-log-searches.md), aby wyświetlić szczegółowe informacje zebrane przez rozwiązania.
- Użyj [pulpitów nawigacyjnych](log-analytics-dashboards.md), aby zapisywać i wyświetlać własne, niestandardowe wyszukiwania.



<!--HONumber=Sep16_HO3-->


