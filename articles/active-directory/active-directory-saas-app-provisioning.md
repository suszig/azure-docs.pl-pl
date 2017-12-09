---
title: "Automatyczne inicjowanie obsługi użytkowników aplikacji SaaS w usłudze Azure AD | Dokumentacja firmy Microsoft"
description: "Wprowadzenie do wykorzystania usługi Azure AD można automatycznie udostępnić, usuwanie i aktualizowane na bieżąco kont użytkowników dla wielu aplikacji SaaS innych firm."
services: active-directory
documentationcenter: 
author: asmalser-msft
manager: femila
editor: 
ms.assetid: 58c5fa2d-bb33-4fba-8742-4441adf2cb62
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/15/2017
ms.author: asmalser
ms.openlocfilehash: 0fdb1605487cca7ba3dc7f13fe8cbe3d1be78d44
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Automatyzowanie użytkownika alokowania i anulowania alokowania do aplikacji SaaS w usłudze Azure Active Directory
## <a name="what-is-automated-user-provisioning-for-saas-apps"></a>Co to jest automatyczne Inicjowanie obsługi użytkowników dla aplikacji SaaS?
Azure Active Directory (Azure AD) pozwala na automatyzację tworzenia, obsługi i usuwania tożsamości użytkowników w chmurze ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)) aplikacji, takich jak Dropbox, Salesforce, ServiceNow i inne.

**Poniżej przedstawiono kilka przykładów co ta funkcja umożliwia:**

* Automatycznie Utwórz nowych kont w systemach prawym dla nowych użytkowników w celu dołączenia zespołu lub organizacji.
* Automatycznie Dezaktywuj kont w systemach prawo w przypadku osób pozostawia zespół lub organizacja.
* Upewnij się, że tożsamości w aplikacji i systemów są aktualizowane na podstawie zmian w katalogu lub system zasobów ludzkich.
* Udostępnianie obiektów nienależących do użytkownika, takich jak grupy, do aplikacji, które je obsługują.

**Inicjowanie obsługi użytkowników automatycznych zawiera również następujące funkcje:**

* Możliwość odpowiada tożsamościom istniejących między systemami źródłowym i docelowym.
* Opcje dostosowywania Pomocy usługi Azure AD zależności od bieżącej konfiguracji aplikacji i systemów, które organizacja korzysta obecnie.
* Opcjonalne wiadomości e-mail dla alertów do inicjowania obsługi błędów.
* Dzienniki raportowania i działania do monitorowania i rozwiązywania problemów.

## <a name="why-use-automated-provisioning"></a>Dlaczego warto używać automatyczne Inicjowanie obsługi?
Niektóre typowe motywacji dla tej funkcji obejmują:

* Aby uniknąć kosztów, wydajność i błędu ludzkiego związanego z ręcznego inicjowania obsługi procesów.
* Aby uniknąć kosztów związanych z udostępniania i utrzymywania opracowany niestandardowe rozwiązania inicjowania obsługi administracyjnej i skryptów
* W celu zabezpieczenia organizacji natychmiastowe usunięcie tożsamości użytkowników z klucza aplikacji SaaS w przypadku opuszczenia organizacji.
* Aby łatwo importować zbiorczego liczbę użytkowników do określonej aplikacji SaaS lub systemu.
* Aby korzystać z wygodą dostępności rozwiązania inicjowania obsługi administracyjnej uruchomić się te same zasady dostępu do aplikacji, zdefiniowane dla usługi Azure AD rejestracji jednokrotnej.


## <a name="how-does-automatic-provisioning-work"></a>Jak działa automatyczne udostępnianie?
    
**Usługi Azure AD udostępniania** udostępnia użytkownikom aplikacji SaaS i innymi systemami, łącząc się z punktów końcowych interfejsu API zarządzania użytkownika dostarczone przez dostawcę każdej aplikacji. Zezwalaj na te punkty końcowe interfejsu API zarządzania użytkownika usługi Azure AD, można programowo tworzyć, aktualizować i usuwać użytkowników. Dla wybranych aplikacji, inicjowania obsługi usługi mogą także tworzyć aktualizować i usuwać dodatkowe obiekty dotyczące tożsamości, takie jak grup i ról. 

![Inicjowanie obsługi administracyjnej](./media/active-directory-saas-app-provisioning/provisioning0.PNG)
*rysunku 1: usługi Azure AD, świadczenie usługi*

![Udostępnianie wychodzące](./media/active-directory-saas-app-provisioning/provisioning1.PNG)
*na rysunku 2: "Wychodzący" użytkownika inicjowania obsługi przepływu pracy z usługą Azure AD popularnych aplikacji SaaS*

![Przychodzące inicjowania obsługi administracyjnej](./media/active-directory-saas-app-provisioning/provisioning2.PNG)
*rysunek 3: użytkownik "Przychodzącego" Inicjowanie obsługi przepływu pracy z popularnych aplikacji zarządzania kapitału człowieka (HCM) do usługi Azure Active Directory i usługi Active Directory systemu Windows Server*


## <a name="what-applications-and-systems-can-i-use-with-azure-ad-automatic-user-provisioning"></a>Jakie aplikacje i systemy można użyć z inicjowaniem obsługi administracyjnej użytkowników usługi Azure AD?

Funkcje platformy Azure AD wstępnie zintegrowanych pomocy technicznej w różnych popularnych aplikacji SaaS i systemami kadr, a także ogólne pomocy technicznej dla aplikacji, które implementują określonych części [standard SCIM 2.0](https://docs.microsoft.com/azure/active-directory/active-directory-scim-provisioning).

Wszystkie aplikacje "Proponowanym" w galerii aplikacji usługi Azure AD obsługuje użytkownika automatycznego inicjowania obsługi administracyjnej. [W tym miejscu można wyświetlić listę polecanych aplikacji.](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1&subcategories=featured)

Aby aplikacja do obsługi automatycznego przypisywania użytkowników najpierw przekazuje użytkownika wymagane punkty końcowe zarządzania, które umożliwiają automatyzację tworzenia, obsługi i usuwania użytkowników zewnętrznych programy. W związku z tym nie wszystkie aplikacje SaaS są zgodne z tą funkcją. W przypadku aplikacji, które obsługują zarządzanie użytkownikami interfejsy API zespołu inżynieryjnego usługi Azure AD będą w stanie Tworzenie łącznika inicjowania obsługi administracyjnej do tych aplikacji, a tej pracy jest priorytety zgodnie z potrzebami aktualnych i potencjalnych klientów. 

Skontaktuj się z usługą Azure AD engineering team do żądania obsługi inicjowania obsługi administracyjnej dodatkowych aplikacji, przesłać wiadomość za pośrednictwem [forum opinii w usłudze Azure Active Directory](https://feedback.azure.com/forums/374982-azure-active-directory-application-requests/category/172035-user-provisioning). 
    
    
## <a name="how-do-i-set-up-automatic-provisioning-to-an-application"></a>Jak skonfigurować automatyczne Inicjowanie obsługi administracyjnej dla aplikacji?

Konfiguracja programu Azure AD usługi inicjowania obsługi administracyjnej dla wybranej aplikacji jest uruchamiany w  **[portalu Azure](https://potal.azure.com)**. W **usługi Azure Active Directory > aplikacje przedsiębiorstwa** wybierz opcję **Dodaj**, następnie **wszystkie**, a następnie dodaj jednej z następujących pozycji w zależności od danego scenariusza:

* Wszystkie aplikacje w **polecanych aplikacji** sekcji obsługi automatycznego inicjowania obsługi administracyjnej

* Użyj opcji "bez galerii aplikacji" opracowany niestandardowej integracji SCIM

![Galeria](./media/active-directory-saas-app-provisioning/gallery.png)

Na ekranie aplikacji zarządzania inicjowania obsługi administracyjnej jest skonfigurowany w **inicjowania obsługi administracyjnej** kartę.

![Ustawienia](./media/active-directory-saas-app-provisioning/provisioning_settings0.PNG)


* **Poświadczenia administratora** należy podać do usługi Azure AD, inicjowania obsługi usługi, które umożliwiają podłączenie go do zarządzania użytkownikami udostępniany przez aplikację interfejsu API.

* **Mapowania atrybutów** można skonfigurować które określają, które pola w systemie źródłowym (przykład: usługi Azure AD) będzie ich zawartość zsynchronizowano do pola, które w systemie docelowym (przykład: ServiceNow). Jeśli aplikacja docelowa obsługuje tę funkcję, w tej sekcji umożliwi opcjonalnie skonfigurować inicjowania obsługi grup oprócz kont użytkowników. "Zgodnych właściwości" umożliwiają wybierz pola, które są używane do dopasowania kont między systemami. "[Wyrażenia](active-directory-saas-writing-expressions-for-attribute-mappings.md)" umożliwiają modyfikowanie i przekształcanie wartości pobrać z systemu źródłowego zapisywane do systemu docelowego. Aby uzyskać więcej informacji, zobacz [Dostosowywanie mapowań atrybutów](active-directory-saas-customizing-attribute-mappings.md).

![Ustawienia](./media/active-directory-saas-app-provisioning/provisioning_settings1.PNG)

* **Filtrami zakresów** Poinformuj użytkowników, którzy inicjowania obsługi usługi i grupy w systemie źródłowym powinny być udostępniane i/lub cofnąć jej zainicjowania jej do systemu docelowego. Istnieją dwa aspekty zakresu filtry, które są oceniane razem określające, który znajduje się w zakresie do inicjowania obsługi:

* **Filtr w wartości atrybutu** -menu "Zakres obiektu źródłowego" w mapowaniu atrybutu umożliwia filtrowanie na określone wartości danego atrybutu. Na przykład można określić, że tylko użytkownicy z atrybutem "Dział", "Sprzedaży" musi należeć do zakresu do inicjowania obsługi.

* **Filtr przydziałów** — w obsługi menu 'Scope' > sekcji Ustawienia portalu umożliwia określenie, czy tylko "przypisanych" użytkowników i grup powinny być w zakresie udostępniania lub jeśli wszyscy użytkownicy w katalogu usługi Azure AD powinna być Zainicjowano obsługę administracyjną. Aby uzyskać informacje na "przypisywanie" użytkowników i grup, zobacz [przypisać użytkownika lub grupy do aplikacji przedsiębiorstwa w usłudze Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md).
    
* **Ustawienia** sterowania działaniem usługi inicjowania obsługi administracyjnej dla aplikacji, w tym, czy jest obecnie uruchomiona lub nie.

* **Dzienniki inspekcji** Podaj rekordów każda operacja wykonywana przez usługę Azure AD, inicjowania obsługi usługi. Aby uzyskać więcej informacji, zobacz [inicjowania obsługi administracyjnej Przewodnik po raportowaniu](active-directory-saas-provisioning-reporting.md).

![Ustawienia](./media/active-directory-saas-app-provisioning/audit_logs.PNG)

## <a name="what-happens-during-provisioning"></a>Co się dzieje podczas inicjowania obsługi?

1. Po włączeniu udostępniania dla aplikacji po raz pierwszy, są wykonywane następujące czynności:
   * Usługi Azure AD będzie podejmować próby odpowiada żadnych istniejących użytkowników w aplikacji SaaS, ich odpowiednich tożsamości w katalogu. Po dopasowaniu użytkownika, są one *nie* automatycznie włączone dla rejestracji jednokrotnej. Aby użytkownik miał dostęp do aplikacji ich musi mieć jawnie przypisanej do aplikacji w usłudze Azure AD, bezpośrednio lub za pośrednictwem członkostwa w grupie.
   * Jeśli został już określony użytkowników, którzy powinny zostać przypisane do aplikacji i nie może odnaleźć istniejących kont dla tych użytkowników usługi Azure AD, Azure AD udostępnić nowych kont dla nich w aplikacji.
2. Po ukończeniu synchronizacji początkowej, jak opisano powyżej, usługi Azure AD sprawdza co 20 minut dla następujących zmian:
   * Jeśli nowych użytkowników zostały przypisane do aplikacji (bezpośrednio lub za pośrednictwem członkostwa w grupie), następnie są one udostępniane z nowego konta w aplikacji SaaS.
   * Jeśli usunięto dostępu użytkownika, a następnie swojego konta w aplikacji SaaS jest oznaczone jako wyłączone (użytkownicy nigdy nie pełni usunięciu, który chroni przed utratą danych w przypadku błędnej konfiguracji).
   * Jeśli użytkownik niedawno został przypisany do aplikacji i już swoje konto w aplikacji SaaS, to konto jest oznaczone jako włączone, a niektóre właściwości użytkownika mogą być aktualizowane, jeśli są nieaktualne w porównaniu do katalogu.
   * Jeśli informacje o użytkowniku (takie jak numer telefonu, oddział) została zmieniona w katalogu, tych informacji będzie można również zaktualizować w aplikacji SaaS.


## <a name="frequently-asked-questions"></a>Często zadawane pytania
**Jak często usługi Azure AD Zapisz zmiany katalogu do aplikacji SaaS?**

Po zakończeniu początkowej pełnej synchronizacji programu Azure AD, zwykle świadczenie usługi sprawdza obecność zmian co 20 minut. 

Jeśli aplikacja SaaS zwraca kilka błędów, (na przykład w przypadku poświadczeń administratora nieprawidłowy), w usłudze Azure AD spowoduje stopniowo spowolnienie jego częstotliwość maksymalnie raz dziennie, dopóki nie zostaną one usunięte. W takim przypadku zadania inicjowania obsługi usługi Azure AD przechodzi w stan "kwarantanny" i wskazuje to w [inicjowania obsługi administracyjnej raport z podsumowaniem](active-directory-saas-provisioning-reporting.md).

**Jak długo trwa udostępnianie moich użytkowników?**

Po zakończeniu pełnej synchronizacji początkowej zmiany przyrostowe zwykle nastąpić w ciągu 20 40 minut. Jeśli chcesz udostępnić większość katalogu, następnie zależy od liczby użytkowników i grup, do których masz. Wydajność zależy od wydajności Zarządzanie użytkownikami interfejsy API, używaną do odczytywania danych z systemu źródłowego i zapisać dane do systemu docelowego świadczenia usług. 

Wydajność jest również wolniej, jeśli istnieje wiele błędów (zarejestrowane w [dzienniki inspekcji](active-directory-saas-provisioning-reporting.md)) i inicjowania obsługi usługi stała się w stanie "kwarantanny".

**Co to jest pełną synchronizację początkową i dlaczego trwa dłużej niż kolejne synchronizacje?**

Podczas pierwszego uruchomienia programu Azure AD usługi inicjowania obsługi administracyjnej dla danej aplikacji, jego pobiera "migawkę" użytkowników (i opcjonalnie grup) w katalogu źródłowym. Ta migawka umożliwia usłudze inicjowania obsługi administracyjnej i zmniejsza liczbę rund do źródłowa i docelowa interfejsów API i umożliwia synchronizacje kolejnych "delta" wydajniej zachowuje się. 

Pełna synchronizacja początkowa użytkowników często można wykonać w minutach, bardzo małych katalogów, ale może potrwać kilka godzin w przypadku większych katalogów. Katalogi przedsiębiorstwa z setkami tysięcy użytkowników może zająć wiele godzin na potrzeby początkowej synchronizacji zakończyć. Jednak po początkowej synchronizacji synchronizacje kolejnych "delta" nastąpić znacznie szybciej.

> [!NOTE]
> Dla aplikacji, które obsługę grup i członkostwa w grupach włączenie tego znacznie zwiększa czas, jaki zajmuje pełną synchronizację, aby zakończyć. Jeśli nie chcesz udostępniać nazwy grupy i członkostwa w grupach aplikacji, można wyłączyć tę opcję w [mapowania atrybutów](active-directory-saas-customizing-attribute-mappings.md) konfiguracji inicjowania obsługi administracyjnej.

**Jak można śledzić postęp bieżącego zadania inicjowania obsługi administracyjnej**

Zobacz [inicjowania obsługi administracyjnej Przewodnik po raportowaniu](active-directory-saas-provisioning-reporting.md).

**Jak wiedzą, jeśli użytkownicy nie mogą pobrać udostępniane poprawnie?**

Wszystkie błędy są rejestrowane w usłudze Azure AD dzienniki inspekcji. Aby uzyskać więcej informacji, zobacz [inicjowania obsługi administracyjnej Przewodnik po raportowaniu](active-directory-saas-provisioning-reporting.md).

**Jak przesłać opinię do zespołu inżynieryjnego**

Skontaktuj się z nami za pośrednictwem [forum opinii w usłudze Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/).


## <a name="related-articles"></a>Pokrewne artykuły:
* [Indeks artykułów dotyczących zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
* [Dostosowywanie mapowań atrybutów do inicjowania obsługi użytkowników](active-directory-saas-customizing-attribute-mappings.md)
* [Tworzenie wyrażeń na potrzeby mapowań atrybutów](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Filtry zakresu dla Inicjowanie obsługi użytkowników](active-directory-saas-scoping-filters.md)
* [Włączanie automatycznej aprowizacji użytkowników i grup z usługi Azure Active Directory do aplikacji przy użyciu SCIM](active-directory-scim-provisioning.md)
* [Powiadomienia aprowizacji kont](active-directory-saas-account-provisioning-notifications.md)
* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS](active-directory-saas-tutorial-list.md)

