---
title: "Zarządzanie dostępem do usługi Log Analytics | Microsoft Docs"
description: "Możesz zarządzać dostępem do usługi Log Analytics przy użyciu różnych zadań administracyjnych dotyczących użytkowników, kont, obszarów roboczych pakietu OMS i kont platformy Azure."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: jwhit
editor: 
ms.assetid: d0e5162d-584b-428c-8e8b-4dcaa746e783
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/02/2016
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: fbd35f9fdd5165a2dd4bc7dd47bd70c890539e38


---
# <a name="manage-access-to-log-analytics"></a>Zarządzanie dostępem do usługi Log Analytics
Dostępem do usługi Log Analytics można zarządzać, wykonując różne zadania administracyjne dotyczące użytkowników, kont, obszarów roboczych pakietu OMS i kont platformy Azure. Aby utworzyć obszar roboczy pakietu Operations Management Suite (OMS), wybierz nazwę obszaru roboczego, skojarz go z kontem, a następnie wybierz lokalizację geograficzną. Obszar roboczy to kontener, który zawiera informacje o koncie i podstawowe informacje o konfiguracji konta. Ty i inni członkowie organizacji możecie używać wielu obszarów roboczych pakietu OMS, aby zarządzać różnymi zestawami danych zebranymi z całej infrastruktury IT lub jej części.

W artykule [Wprowadzenie do usługi Log Analytics](log-analytics-get-started.md) pokazano, jak szybko rozpocząć pracę. W pozostałej części tego artykułu opisano najczęściej używane zadania:

* Określanie wymaganej liczby obszarów roboczych
* Zarządzanie kontami i użytkownikami
* Dodawanie grupy do istniejącego obszaru roboczego
* Łączenie istniejącego obszaru roboczego z subskrypcją platformy Azure
* Uaktualnianie obszaru roboczego do płatnego planu taryfowego
* Zmienianie typu planu taryfowego
* Dodawanie organizacji usługi Azure Active Directory do istniejącego obszaru roboczego
* Usuwanie obszaru roboczego pakietu OMS

## <a name="determine-the-number-of-workspaces-you-need"></a>Określanie wymaganej liczby obszarów roboczych
Obszar roboczy jest zasobem platformy Azure w postaci kontenera, w którym dane są zbierane, agregowane, analizowane i przedstawiane w portalu pakietu OMS.

Możesz utworzyć wiele obszarów roboczych usługi Log Analytics pakietu OMS, a użytkownicy mogą mieć dostęp do jednego lub wielu obszarów roboczych. Zminimalizowanie liczby obszarów roboczych pozwala na wykonywanie zapytań dla większości danych i korelowanie tych danych. W tej sekcji opisano sytuacje, w których pomocne może być utworzenie więcej niż jednego obszaru roboczego.

Obecnie obszar roboczy usługi Log Analytics oferuje następujące możliwości:

* Lokalizacja geograficzna do przechowywania danych
* Poziom szczegółowości rozliczeń
* Izolacja danych

Na podstawie powyższych właściwości można wskazać następujące sytuacje, w których warto utworzyć wiele obszarów roboczych:

* Globalna firma wymaga, aby jej dane były przechowywane w określonych regionach w celu zachowania poufności danych i ich zgodności z przepisami.
* Korzystasz z platformy Azure i chcesz uniknąć naliczania opłat za transfer danych wychodzących przez umieszczenie obszaru roboczego usługi Log Analytics w tym samym regionie co zarządzane przez niego zasoby platformy Azure.
* Chcesz przydzielić opłaty do różnych działów lub grup biznesowych na podstawie ich użycia. Gdy utworzysz obszar roboczy dla każdego działu lub każdej grupy biznesowej, na rachunku dotyczącym platformy Azure i w zestawieniu użycia będą widoczne osobne opłaty dla poszczególnych obszarów roboczych.
* Jesteś dostawcą usługi zarządzanej i chcesz, aby dane analizy dziennika dla klientów, którymi zarządzasz, były odizolowane od danych innych klientów.
* Zarządzasz wieloma klientami i chcesz, aby dla poszczególnych klientów, działów i grup biznesowych były widoczne ich własne dane, ale nie dane innych klientów, działów czy grup biznesowych.

Kiedy zbierasz dane przy użyciu agenta, możesz skonfigurować każdego agenta w celu raportowania do wymaganego obszaru roboczego.

Jeśli używasz programu System Center Operations Manager, jedna grupa zarządzania programu Operations Manager może być połączona tylko z jednym obszarem roboczym. Możesz zainstalować program Microsoft Monitoring Agent na komputerach zarządzanych przez program Operations Manager oraz skonfigurować agenta w celu raportowania do programu Operations Manager i innego obszaru roboczego usługi Log Analytics.

### <a name="workspace-information"></a>Informacje o obszarze roboczym
W portalu pakietu OMS możesz wyświetlić informacje o obszarze roboczym i określić, czy chcesz otrzymywać informacje od firmy Microsoft.

#### <a name="view-workspace-information"></a>Wyświetlanie informacji o obszarze roboczym
1. W pakiecie OMS kliknij kafelek **Ustawienia**.
2. Kliknij kartę **Konta**.
3. Kliknij kartę **Informacje o obszarze roboczym**.  
   ![Informacje o obszarze roboczym](./media/log-analytics-manage-access/workspace-information.png)

## <a name="manage-accounts-and-users"></a>Zarządzanie kontami i użytkownikami
Z obszarem roboczym może być skojarzonych wiele kont użytkowników, a konto użytkownika (konto Microsoft lub konto organizacyjne) może uzyskiwać dostęp do wielu obszarów roboczych pakietu OMS.

Domyślnie konto Microsoft lub konto organizacyjne użyte do utworzenia obszaru roboczego jest kontem administratora obszaru roboczego. Administrator może zaprosić dodatkowe konta Microsoft lub wybrać użytkowników z usługi Azure Active Directory.

Dostęp do obszaru roboczego pakietu OMS jest kontrolowany w dwóch miejscach:

* Na platformie Azure można użyć kontroli dostępu opartej na rolach, aby zapewnić dostęp do subskrypcji platformy Azure i skojarzonych zasobów platformy Azure. Te uprawnienia są również używane w przypadku dostępu do programu PowerShell i interfejsu API REST.
* W portalu pakietu OMS — dostęp tylko portalu pakietu OMS, a nie do skojarzonej subskrypcji platformy Azure.

Jeśli udzielisz użytkownikom dostępu tylko do portalu pakietu OMS, ale nie do połączonej z nim subskrypcji platformy Azure, użytkownicy nie będą widzieć żadnych danych na kafelkach rozwiązań Backup i Site Recovery.
Aby umożliwić wszystkim użytkownikom wyświetlanie danych w tych rozwiązaniach, upewnij się, że mają oni uprawnienia dostępu co najmniej na poziomie **czytelnika** do magazynu kopii zapasowych i magazynu usługi Site Recovery połączonego z obszarem roboczym pakietu OMS.   

### <a name="managing-access-to-log-analytics-using-the-azure-portal"></a>Zarządzanie dostępem do usługi Log Analytics przy użyciu witryny Azure Portal
Jeśli udzielisz użytkownikom dostępu do obszaru roboczego usługi Log Analytics przy użyciu uprawnień platformy Azure (np. w witrynie Azure Portal), będą oni mogli uzyskiwać dostęp do portalu usługi Log Analytics. W witrynie Azure Portal użytkownicy mogą przejść do portalu pakietu OMS przez kliknięcie zadania **Portal pakietu OMS**, gdy jest wyświetlany zasób obszaru roboczego usługi Log Analytics.

Należy pamiętać o kilku kwestiach związanych z witryną Azure Portal:

* Nie jest to *kontrola dostępu oparta na rolach*. Jeśli masz uprawnienia dostępu *czytelnika* do obszaru roboczego usługi Log Analytics w witrynie Azure Portal, możesz wprowadzać zmiany, korzystając z portalu pakietu OMS. Portal pakietu OMS korzysta z ról administratora, współautora i użytkownika tylko do odczytu. Jeśli konto, za pomocą którego Cię zalogowano, znajduje się w usłudze Azure Active Directory połączonej z obszarem roboczym, jesteś administratorem w portalu pakietu OMS. W przeciwnym razie jesteś współautorem.
* Po zalogowaniu się do portalu pakietu OMS na stronie http://mms.microsoft.com domyślnie jest wyświetlana lista **Wybierz obszar roboczy**. Zawiera ona tylko obszary robocze dodane przy użyciu portalu pakietu OMS. Aby wyświetlić obszary robocze, do których uzyskujesz dostęp za pomocą subskrypcji platformy Azure, musisz podać dzierżawę w adresie URL. Na przykład:
  
  `mms.microsoft.com/?tenant=contoso.com` Identyfikator dzierżawy jest często ostatnią częścią adresu e-mail użytego podczas logowania.
* Jeśli zalogowano Cię przy użyciu konta w dzierżawie usługi Azure Active Directory, jesteś *administratorem* w portalu pakietu OMS. Zwykle tak jest, o ile nie logujesz się jako dostawca CSP.  Jeśli konto nie znajduje się w dzierżawie usługi Azure Active Directory, jesteś *użytkownikiem* w portalu pakietu OMS.
* Aby przejść bezpośrednio do portalu, do którego uzyskujesz dostęp przy użyciu uprawnień platformy Azure, musisz podać zasób w adresie URL. Taki adres URL można uzyskać przy użyciu programu PowerShell.
  
  Na przykład `(Get-AzureRmOperationalInsightsWorkspace).PortalUrl`.
  
  Adres URL wygląda następująco: `https://eus.mms.microsoft.com/?tenant=contoso.com&resource=%2fsubscriptions%2faaa5159e-dcf6-890a-a702-2d2fee51c102%2fresourcegroups%2fdb-resgroup%2fproviders%2fmicrosoft.operationalinsights%2fworkspaces%2fmydemo12`

### <a name="managing-users-in-the-oms-portal"></a>Zarządzanie użytkownikami w portalu pakietu OMS
Do zarządzania użytkownikami i grupami służy karta **Zarządzanie użytkownikami** na karcie **Konta** na stronie Ustawienia.   

![zarządzanie użytkownikami](./media/log-analytics-manage-access/setup-workspace-manage-users.png)

#### <a name="add-a-user-to-an-existing-workspace"></a>Dodawanie użytkownika do istniejącego obszaru roboczego
Poniżej przedstawiono procedurę dodawania użytkownika lub grupy do obszaru roboczego pakietu OMS.

1. W pakiecie OMS kliknij kafelek **Ustawienia**.
2. Kliknij kartę **Konta**, a następnie kliknij kartę **Zarządzanie użytkownikami**.
3. W sekcji **Zarządzanie użytkownikami** wybierz typ konta, które chcesz dodać: **Konto organizacyjne**, **Konto Microsoft** lub **Pomoc techniczna firmy Microsoft**.
   
   * W przypadku wybrania konta Microsoft wpisz adres e-mail użytkownika skojarzonego z kontem Microsoft.
   * W przypadku wybrania konta organizacyjnego możesz podać część nazwy albo aliasu e-mail użytkownika lub grupy, co spowoduje wyświetlenie listy zgodnych użytkowników i grup w polu listy rozwijanej. Wybierz użytkownika lub grupę.
   * Użyj konta Pomoc techniczna firmy Microsoft, aby udzielić inżynierowi pomocy technicznej lub innemu pracownikowi firmy Microsoft tymczasowego dostępu do obszaru roboczego w celu ułatwienia rozwiązywania problemów.
     
     > [!NOTE]
     > W celu uzyskania optymalnej wydajności zmniejsz liczbę grup usługi Active Directory skojarzonych z kontem pakietu OMS do trzech — jednej dla administratorów, jednej dla współautorów i jednej dla użytkowników tylko do odczytu. Użycie większej liczby grup może mieć zły wpływ na wydajność usługi Log Analytics.
     > 
     > 
4. Wybierz typ użytkownika lub grupy do dodania: **Administrator**, **Współautor** lub **Użytkownik tylko do odczytu**.  
5. Kliknij pozycję **Dodaj**.
   
   W przypadku dodawania konta Microsoft na podany adres e-mail zostanie wysłane zaproszenie do dołączenia do obszaru roboczego. Gdy użytkownik wykona instrukcje zawarte w zaproszeniu do dołączenia do pakietu OMS, będzie mógł uzyskać dostęp do tego obszaru roboczego pakietu OMS.
   W przypadku dodawania konta organizacyjnego użytkownik będzie miał natychmiast dostęp do usługi Log Analytics.  
   ![wiadomość e-mail z zaproszeniem](./media/log-analytics-manage-access/setup-workspace-invitation-email.png)

#### <a name="edit-an-existing-user-type"></a>Edytowanie istniejącego typu użytkownika
Możesz zmienić rolę konta użytkownika skojarzonego z Twoim kontem pakietu OMS. Dostępne są następujące role:

* *Administrator*: może zarządzać użytkownikami, wyświetlać wszystkie alerty i wykonywać na nich operacje oraz dodawać i usuwać serwery
* *Współautor*: może wyświetlać wszystkie alerty i wykonywać na nich operacje oraz dodawać i usuwać serwery
* *Użytkownik tylko do odczytu*: użytkownicy oznaczeni jako tylko do odczytu mają następujące ograniczenia:
  
  1. Nie mogą dodawać ani usuwać rozwiązań. Galeria rozwiązań jest ukryta.
  2. Nie mogą dodawać, modyfikować ani usuwać kafelków na stronie **Mój pulpit nawigacyjny**.
  3. Nie mogą wyświetlać stron **Ustawienia**. Są one ukryte.
  4. W widoku wyszukiwania, konfiguracji usługi Power BI, zapisanych wyszukiwaniach i alertach zadania są ukryte.

#### <a name="to-edit-an-account"></a>Aby edytować konto
1. W pakiecie OMS kliknij kafelek **Ustawienia**.
2. Kliknij kartę **Konta**, a następnie kliknij kartę **Zarządzanie użytkownikami**.
3. Wybierz rolę użytkownika, którą chcesz zmienić.
4. W oknie dialogowym potwierdzenia kliknij przycisk **Tak**.

### <a name="remove-a-user-from-an-oms-workspace"></a>Usuwanie użytkownika z obszaru roboczego pakietu OMS
Poniżej przedstawiono procedurę usuwania użytkownika z obszaru roboczego pakietu OMS. Usunięcie użytkownika nie powoduje zamknięcia obszaru roboczego. Powoduje to natomiast usunięcie skojarzenia między użytkownikiem i obszarem roboczym. Jeśli użytkownik jest skojarzony z wieloma obszarami roboczymi, będzie on nadal miał możliwość zalogowania się do pakietu OMS i wyświetlania innych obszarów roboczych.

1. W pakiecie OMS kliknij kafelek **Ustawienia**.
2. Kliknij kartę **Konta**, a następnie kliknij kartę **Zarządzanie użytkownikami**.
3. Kliknij pozycję **Usuń** obok nazwy użytkownika, którego chcesz usunąć.
4. W oknie dialogowym potwierdzenia kliknij przycisk **Tak**.

### <a name="add-a-group-to-an-existing-workspace"></a>Dodawanie grupy do istniejącego obszaru roboczego
1. Wykonaj kroki od 1 do 4 przedstawione w powyższej sekcji „Dodawanie użytkownika do istniejącego obszaru roboczego”.
2. W obszarze **Wybierz użytkownika/grupę** wybierz pozycję **Grupa**.
   ![dodawanie grupy do istniejącego obszaru roboczego](./media/log-analytics-manage-access/add-group.png)
3. Wprowadź nazwę wyświetlaną lub adres e-mail grupy, którą chcesz dodać.
4. Wybierz grupę z listy wyników, a następnie kliknij przycisk **Dodaj**.

## <a name="link-an-existing-workspace-to-an-azure-subscription"></a>Łączenie istniejącego obszaru roboczego z subskrypcją platformy Azure
Wszystkie obszary robocze utworzone po 26 września 2016 roku muszą być w czasie tworzenia połączone z subskrypcją platformy Azure. Obszary robocze utworzone wcześniej muszą zostać połączone z obszarem roboczym po następnym zalogowaniu. 

> [!IMPORTANT]
> Aby można było połączyć obszar roboczy, konto platformy Azure musi już mieć dostęp do obszaru roboczego, który chcesz połączyć.  Oznacza to, że konto używane do uzyskiwania dostępu do witryny Azure Portal musi być **tym samym kontem**, za pomocą którego jest uzyskiwany dostęp do obszaru roboczego pakietu OMS. Jeśli nie jest, zobacz [Dodawanie użytkownika do istniejącego obszaru roboczego](#add-a-user-to-an-existing-workspace).
> 
> 

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-oms-portal"></a>Aby połączyć obszar roboczy z subskrypcją platformy Azure w portalu pakietu OMS
Aby móc połączyć obszar roboczy z subskrypcją platformy Azure w portalu pakietu OMS, zalogowany użytkownik musi już mieć płatne konto platformy Azure.

1. W pakiecie OMS kliknij kafelek **Ustawienia**.
2. Kliknij kartę **Konta**, a następnie kliknij kartę **Subskrypcja i plan taryfowy platformy Azure**.
3. Kliknij plan taryfowy, którego chcesz używać.
4. Kliknij pozycję **Zapisz**.  
   ![subskrypcja i plany taryfowe](./media/log-analytics-manage-access/subscription-tab.png)

Nowy plan taryfowy jest wyświetlany na wstążce portalu pakietu OMS w górnej części strony sieci Web.

![Wstążka pakietu OMS](./media/log-analytics-manage-access/data-plan-changed.png)

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-azure-portal"></a>Aby połączyć obszar roboczy z subskrypcją platformy Azure w witrynie Azure Portal
1. Zaloguj się do [Azure Portal](http://portal.azure.com).
2. Wyszukaj pozycję **Log Analytics (OMS)** i wybierz ją.
3. Zostanie wyświetlona lista istniejących obszarów roboczych. Kliknij pozycję **Dodaj**.  
   ![lista obszarów roboczych](./media/log-analytics-manage-access/manage-access-link-azure01.png)
4. W obszarze **Obszar roboczy pakietu OMS** kliknij pozycję **Lub połącz istniejący**.  
   ![łączenie istniejącego obszaru roboczego](./media/log-analytics-manage-access/manage-access-link-azure02.png)
5. Kliknij pozycję **Skonfiguruj wymagane ustawienia**.  
   ![konfigurowanie wymaganych ustawień](./media/log-analytics-manage-access/manage-access-link-azure03.png)
6. Zostanie wyświetlona lista obszarów roboczych, które nie są jeszcze połączone z Twoim kontem platformy Azure. Wybierz obszar roboczy.  
   ![wybieranie obszarów roboczych](./media/log-analytics-manage-access/manage-access-link-azure04.png)
7. W razie potrzeby możesz zmienić wartości następujących elementów:
   * Subskrypcja
   * Grupa zasobów
   * Lokalizacja
   * Warstwa cenowa  
     ![zmienianie wartości](./media/log-analytics-manage-access/manage-access-link-azure05.png)
8. Kliknij pozycję **Utwórz**. Obszar roboczy jest teraz połączony z kontem platformy Azure.

> [!NOTE]
> Jeśli obszar roboczy, który chcesz połączyć, nie jest wyświetlany, oznacza to, że subskrypcja platformy Azure nie ma dostępu do obszaru roboczego pakietu OMS utworzonego przy użyciu witryny sieci Web pakietu OMS.  Musisz udzielić dostępu do tego konta z poziomu obszaru roboczego pakietu OMS. Aby to zrobić, zobacz [Dodawanie użytkownika do istniejącego obszaru roboczego](#add-a-user-to-an-existing-workspace).
> 
> 

## <a name="upgrade-a-workspace-to-a-paid-plan"></a>Uaktualnianie obszaru roboczego do płatnego planu
Dostępne są trzy typy planów obszarów roboczych dla pakietu OMS: **Bezpłatny**, **Autonomiczny** i **OMS**.  Jeśli używany jest plan *Bezpłatny*, do usługi Log Analytics można wysłać dziennie maksymalnie 500 MB danych.  Aby po przekroczeniu tego limitu móc nadal zbierać dane, musisz zmienić obszar roboczy na plan płatny. Typ planu można zmienić w dowolnym momencie.  Aby uzyskać więcej informacji o cenach pakietu OMS, zobacz [szczegółowe informacje o cenach](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-pricing).

### <a name="using-entitlements-from-an-oms-subscription"></a>Używanie uprawnień z subskrypcji pakietu OMS
Aby używać uprawnień wynikających z zakupu pakietu OMS E1, OMS E2 OMS lub dodatku pakietu OMS dla programu System Center, wybierz plan *OMS* dla usługi Log Analytics pakietu OMS.

Po zakupie subskrypcji pakietu OMS uprawnienia zostaną dodane do umowy Enterprise Agreement. Z tych uprawnień mogą korzystać dowolne subskrypcje platformy Azure utworzone w ramach tej umowy. Dzięki temu można mieć na przykład wiele obszarów roboczych pakietu OMS korzystających z uprawnień subskrypcji pakietu OMS.

Aby upewnić się, że uprawnienia subskrypcji pakietu OMS są uwzględniane w danych użycia obszaru roboczego pakietu OMS, wykonaj następujące czynności:

1. Połącz obszar roboczy pakietu OMS z subskrypcją platformy Azure w ramach umowy Enterprise Agreement, która obejmuje subskrypcję pakietu OMS
2. Wybierz plan *OMS* dla obszaru roboczego

> [!NOTE]
> Jeśli obszar roboczy został utworzony przed 26 września 2016 roku, a plan taryfowy usługi Log Analytics to *Premium*, to ten obszar roboczy będzie używać uprawnień z dodatku pakietu OMS dla programu System Center. Uprawnień można także użyć, zmieniając warstwę cenową pakietu *OMS*. 
> 
> 

Uprawnienia subskrypcji pakietu OMS nie są widoczne w witrynie Azure Portal lub w portalu pakietu OMS. Uprawnienia i użycie są natomiast widoczne w witrynie Enterprise Portal.  

Jeśli chcesz zmienić subskrypcję platformy Azure, z którą jest połączony obszar roboczy pakietu OMS, możesz użyć polecenia cmdlet [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) programu Azure PowerShell.

### <a name="using-azure-commitment-from-an-enterprise-agreement"></a>Korzystanie z zobowiązania platformy Azure w ramach umowy Enterprise Agreement
Jeśli nie masz subskrypcji pakietu OMS, zapłacisz osobno za każdy składnik tego pakietu, a dane użycia będą widoczne na rachunku dotyczącym platformy Azure.

Jeśli istnieje zobowiązanie pieniężne platformy Azure dotyczące rejestracji przedsiębiorstwa, z którą są połączone subskrypcje platformy Azure, każde użycie usługi Log Analytics spowoduje automatyczne obciążenie pozostałego zobowiązania pieniężnego.

Jeśli chcesz zmienić subskrypcję platformy Azure, z którą jest połączony obszar roboczy pakietu OMS, możesz użyć polecenia cmdlet [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) programu Azure PowerShell.  

### <a name="change-a-workspace-to-a-paid-data-plan"></a>Zmienianie obszaru roboczego na płatny plan taryfowy
1. Zaloguj się do [Azure Portal](http://portal.azure.com).
2. Wyszukaj pozycję **Log Analytics** i wybierz ją.
3. Zostanie wyświetlona lista istniejących obszarów roboczych. Wybierz obszar roboczy.  
   ![lista obszarów roboczych](./media/log-analytics-manage-access/manage-access-change-plan01.png)
4. W obszarze **Ustawienia** kliknij pozycję **Warstwa cenowa**.  
   ![warstwa cenowa](./media/log-analytics-manage-access/manage-access-change-plan02.png)
5. W obszarze **Warstwa cenowa** wybierz plan taryfowy, a następnie kliknij przycisk **Wybierz**.  
   ![wybieranie planu](./media/log-analytics-manage-access/manage-access-change-plan03.png)
6. Gdy odświeżysz widok w witrynie Azure Portal, zostanie wyświetlona zaktualizowana **warstwa cenowa** wybranego planu.  
   ![aktualizowanie warstwy cenowej](./media/log-analytics-manage-access/manage-access-change-plan04.png)

## <a name="add-an-azure-active-directory-organization-to-an-existing-workspace"></a>Dodawanie organizacji usługi Azure Active Directory do istniejącego obszaru roboczego
Możesz skojarzyć obszar roboczy usługi Log Analytics z domeną usługi Azure Active Directory i dodać użytkowników z katalogu do obszaru roboczego pakietu OMS.

Jeśli utworzysz obszar roboczy z poziomu witryny Azure Portal lub połączysz obszar roboczy z subskrypcją platformy Azure, usługa Azure Active Directory zostanie połączona jako konto organizacyjne.

Jeśli utworzysz obszar roboczy z poziomu portalu pakietu OMS, zostanie wyświetlony monit o utworzenie połączenia z subskrypcją platformy Azure, a konto organizacyjne subskrypcji platformy Azure zostanie połączone z obszarem roboczym usługi Log Analytics.

### <a name="to-add-an-azure-active-directory-organization-to-an-existing-workspace"></a>Aby dodać organizację usługi Azure Active Directory do istniejącego obszaru roboczego
Dodanie organizacji usługi Azure Active Directory umożliwia dodawanie użytkowników i grup z tego katalogu do obszaru roboczego.

1. Na stronie Ustawienia w pakiecie OMS kliknij pozycję **Konta**, a następnie kliknij kartę **Zarządzanie użytkownikami**.  
2. Przejrzyj informacje dotyczące konta organizacyjnego, a następnie kliknij przycisk **Dodaj organizację**.  
    ![dodawanie organizacji](./media/log-analytics-manage-access/manage-access-add-adorg01.png)
3. Wprowadź informacje o tożsamości administratora domeny usługi Azure Active Directory. Następnie zostanie wyświetlone potwierdzenie z informacją o tym, że obszar roboczy jest połączony z domeną usługi Azure Active Directory.
    ![potwierdzenie dotyczące połączonego obszaru roboczego](./media/log-analytics-manage-access/manage-access-add-adorg02.png)

Jeśli konieczne jest dodanie użytkowników z innego katalogu, możesz kliknąć przycisk *Zmień organizację*, aby skojarzyć obszar roboczy z innym katalogiem.

## <a name="delete-your-log-analytics-workspace"></a>Usuwanie obszaru roboczego usługi Log Analytics
Gdy usuniesz obszar roboczy usługi Log Analytics, wszystkie powiązane z nim dane zostaną usunięte z pakietu OMS w ciągu 30 dni.

Jeśli jesteś administratorem i z tym obszarem roboczym jest skojarzonych wielu użytkowników, skojarzenie między tymi użytkownikami i obszarem roboczym zostanie przerwane. Jeśli użytkownicy są skojarzeni z innymi obszarami roboczymi, mogą nadal korzystać z innych obszarów roboczych w pakiecie OMS. Jeśli jednak nie są oni skojarzeni z innymi obszarami roboczymi, muszą utworzyć obszar roboczy, aby móc korzystać z pakietu OMS.

### <a name="to-delete-an-oms-workspace"></a>Aby usunąć obszar roboczy pakietu OMS
1. W pakiecie OMS kliknij kafelek **Ustawienia**.
2. Kliknij kartę **Konta**, a następnie kliknij kartę **Informacje o obszarze roboczym**.
3. Kliknij pozycję **Zamknij obszar roboczy**.
4. Wybierz jedną z przyczyn zamknięcia obszaru roboczego lub podaj inną przyczynę w polu tekstowym.
5. Kliknij pozycję **Zamknij obszar roboczy**.

## <a name="next-steps"></a>Następne kroki
* Aby dowiedzieć się, jak dodać agentów i gromadzić dane, zobacz [Connect Windows computers to Log Analytics](log-analytics-windows-agents.md) (Tworzenie połączenia między komputerami z systemem Windows i usługą Log Analytics).
* [Dodaj rozwiązania Log Analytics z galerii rozwiązań](log-analytics-add-solutions.md), aby dodać funkcje i zebrać dane.
* [Skonfiguruj ustawienia serwera proxy i zapory w usłudze Log Analytics](log-analytics-proxy-firewall.md), jeśli organizacja korzysta z serwera proxy lub zapory, aby agenci mogli komunikować się z usługą Log Analytics.




<!--HONumber=Nov16_HO2-->


