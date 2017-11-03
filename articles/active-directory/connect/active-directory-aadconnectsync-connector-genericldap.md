---
title: "Ogólny łącznik LDAP | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób konfigurowania ogólny łącznik LDAP firmy Microsoft."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 984beeb0-4d91-4908-ad81-c19797c4891b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: adb174526bc377f484be5fb0a71b28e8daaaa6fd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="generic-ldap-connector-technical-reference"></a>Ogólny łącznik LDAP informacje techniczne
W tym artykule opisano ogólny łącznik LDAP. Artykuł dotyczy następujących produktów:

* Program Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * Należy użyć poprawki 4.1.3671.0 lub nowszym [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 i FIM2010R2 łącznika jest dostępna do pobrania z [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

Podczas odwoływania się do grupy roboczej IETF RFC, ten dokument jest w formacie (RFC [numer RFC] / [sekcji w dokumencie RFC]), na przykład (RFC 4512/4.3).
Więcej informacji można znaleźć http://tools.ietf.org/html/rfc4500 (należy zastąpić 4500 poprawny numer RFC).

## <a name="overview-of-the-generic-ldap-connector"></a>Omówienie ogólny łącznik LDAP
Ogólny łącznik LDAP umożliwia integrację usługi synchronizacji z serwerem LDAP w wersji 3.

Nie podano niektóre działania i elementy schematu, takich jak niezbędnych do przeprowadzenia import zmian w dokumentach RFC organizacji IETF. Do tych operacji są obsługiwane tylko katalogów LDAP jawnie określony.

Z punktu widzenia wysokiego poziomu następujące funkcje są obsługiwane w bieżącej wersji łącznika:

| Funkcja | Pomoc techniczna |
| --- | --- |
| Połączonego źródła danych |Łącznik jest obsługiwana przez wszystkie serwery v3 LDAP (maksymalnie ze specyfikacją RFC 4510). Był testowany z następujących czynności: <li>Usługi Microsoft Active Directory Lightweight Directory Services (AD LDS)</li><li>Wykaz globalny usługi Microsoft Active Directory (AD GC)</li><li>Serwer katalogowy 389</li><li>Serwer katalogowy Apache</li><li>IBM Tivoli DS</li><li>Isode katalogu</li><li>NetIQ eDirectory</li><li>Novell eDirectory</li><li>Otwórz DJ</li><li>Otwórz DS</li><li>Otwórz LDAP (openldap.org)</li><li>Oracle (wcześniej Sun) Directory Server Enterprise Edition</li><li>RadiantOne serwera katalogu wirtualnego (VDS)</li><li>Jeden serwer katalogowy Sun</li>**Godne katalogi nie są obsługiwane:** <li>Microsoft usług domenowych Active Directory (AD DS) [zamiast tego użyć wbudowanego łącznika usługi Active Directory]</li><li>Oracle Internet katalogu (OID)</li> |
| Scenariusze |<li>Zarządzanie cyklem życia obiektów</li><li>Zarządzanie grupami</li><li>Zarządzanie hasłami</li> |
| Operacje |We wszystkich katalogach LDAP obsługiwane są następujące operacje: <li>Pełny Import</li><li>Eksportowanie</li>Następujące operacje są obsługiwane tylko w określonych katalogach:<li>Import zmian</li><li>Ustawianie hasła, Zmień hasło</li> |
| Schemat |<li>Wykryto schemat ze schematu LDAP (RFC3673 i RFC4512/4.2)</li><li>Obsługuje klasy strukturalne, aux klas i klasy obiektów extensibleObject (RFC4512/4.3)</li> |

### <a name="delta-import-and-password-management-support"></a>Obsługa zarządzania import i hasło delta
Import zmian i zarządzanie hasłami obsługiwane katalogów:

* Usługi Microsoft Active Directory Lightweight Directory Services (AD LDS)
  * Obsługuje wszystkie operacje w import zmian
  * Obsługuje ustawione hasło
* Wykaz globalny usługi Microsoft Active Directory (AD GC)
  * Obsługuje wszystkie operacje w import zmian
  * Obsługuje ustawione hasło
* Serwer katalogowy 389
  * Obsługuje wszystkie operacje w import zmian
  * Obsługuje ustawianie hasła i Zmień hasło
* Serwer katalogowy Apache
  * Nie obsługuje import zmian, ponieważ ten katalog nie ma dziennik trwałych zmian
  * Obsługuje ustawione hasło
* IBM Tivoli DS
  * Obsługuje wszystkie operacje w import zmian
  * Obsługuje ustawianie hasła i Zmień hasło
* Isode katalogu
  * Obsługuje wszystkie operacje w import zmian
  * Obsługuje ustawianie hasła i Zmień hasło
* Novell eDirectory i NetIQ eDirectory
  * Obsługuje operacje Add, Update i zmiana nazwy dla import zmian
  * Nie obsługuje operacji usuwania dla import zmian
  * Obsługuje ustawianie hasła i Zmień hasło
* Otwórz DJ
  * Obsługuje wszystkie operacje w import zmian
  * Obsługuje ustawianie hasła i Zmień hasło
* Otwórz DS
  * Obsługuje wszystkie operacje w import zmian
  * Obsługuje ustawianie hasła i Zmień hasło
* Otwórz LDAP (openldap.org)
  * Obsługuje wszystkie operacje w import zmian
  * Obsługuje ustawione hasło
  * Nie obsługuje zmiany hasła
* Oracle (wcześniej Sun) Directory Server Enterprise Edition
  * Obsługuje wszystkie operacje w import zmian
  * Obsługuje ustawianie hasła i Zmień hasło
* RadiantOne serwera katalogu wirtualnego (VDS)
  * Musi być używana wersja 7.1.1 lub nowszej
  * Obsługuje wszystkie operacje w import zmian
  * Obsługuje ustawianie hasła i Zmień hasło
* Jeden serwer katalogowy Sun
  * Obsługuje wszystkie operacje w import zmian
  * Obsługuje ustawianie hasła i Zmień hasło

### <a name="prerequisites"></a>Wymagania wstępne
Przed użyciem łącznika, upewnij się, że masz następujące czynności na serwerze synchronizacji:

* Framework Microsoft .NET 4.5.2 lub nowszej

### <a name="detecting-the-ldap-server"></a>Wykrywanie serwera LDAP
Łącznik, opiera się na różnych technik wykrywa i identyfikuje serwer LDAP. Łącznik korzysta z głównego elementu DSE, nazwa/wersja dostawcy i jego przeprowadzający schematu można znaleźć unikatowy obiektów i atrybutów w niektórych serwerów LDAP występują znane. Te dane, jeśli znaleziono, jest używany do wstępnego wypełniania opcje konfiguracji w łączniku.

### <a name="connected-data-source-permissions"></a>Połączone uprawnienia źródła danych
Do importowania i eksportowania operacje na obiektach w połączonego katalogu, konto łącznika musi mieć odpowiednie uprawnienia. Łącznik wymaga uprawnienia, aby można było wyeksportować zapisu i odczytu uprawnień, aby można było zaimportować. Uprawnienie konfiguracja jest wykonywana w ramach środowiska zarządzania udostępniającego samego katalogu docelowego.

### <a name="ports-and-protocols"></a>Porty i protokoły
Łącznik korzysta określony w konfiguracji, która domyślnie jest 389 protokołu LDAP i 636 dla LDAPS numer portu.

Dla LDAPS należy użyć protokołu SSL 3.0 i TLS. Protokół SSL 2.0 nie jest obsługiwane i nie może zostać uaktywniony.

### <a name="required-controls-and-features"></a>Formanty wymagane i funkcje
Następujące kontrolki LDAP/funkcje muszą być dostępne na serwerze LDAP dla łącznika do poprawnego działania:  
`1.3.6.1.4.1.4203.1.5.3`Filtry PRAWDA/FAŁSZ

Filtr PRAWDA/FAŁSZ często nie został zgłoszony jako obsługiwany przez katalogów LDAP i może pojawiać się na **globalne strony** w obszarze **obowiązkowe funkcji nie znaleziono**. Pozwala utworzyć **lub** filtrów kwerendy LDAP, na przykład podczas importowania wiele typów obiektów. Jeśli można zaimportować więcej niż jeden typ obiektu, serwer LDAP obsługuje tę funkcję.

Jeśli używasz katalogu Unikatowy identyfikator w przypadku zakotwiczenia następujące również muszą być dostępne (Aby uzyskać więcej informacji, zobacz [Konfiguruj zakotwiczenia](#configure-anchors) sekcji):  
`1.3.6.1.4.1.4203.1.5.1`Wszystkie atrybuty operacyjne

Jeśli katalog ma więcej obiektów niż co mieści się w jednym wywołaniu do katalogu, następnie zaleca się używać stronicowania. Stronicowanie działała, należy jedną z następujących opcji:

**Opcja 1:**  
`1.2.840.113556.1.4.319`pagedResultsControl

**Opcja 2:**  
`2.16.840.1.113730.3.4.9`VLVControl  
`1.2.840.113556.1.4.473`SortControl

Jeśli obie te opcje są włączone w konfiguracji łącznika, pagedResultsControl jest używany.

`1.2.840.113556.1.4.417`ShowDeletedControl

ShowDeletedControl jest używana tylko z Metoda importu delta USNChanged, aby można było wyświetlić usuniętych obiektów.

Łącznik próbuje wykryć opcji istnieje na serwerze. Jeśli nie można wykryć opcje, ostrzeżenie znajduje się na stronie globalne w właściwości łącznika. Nie wszystkie serwery LDAP występuje wszystkie formanty/funkcje obsługują i nawet, jeśli to ostrzeżenie jest obecny, łącznik może działać bez problemów.

### <a name="delta-import"></a>Import zmian
Import zmian jest dostępna tylko w przypadku, gdy wykryto katalogu pomocy technicznej. Obecnie są używane następujące metody:

* LDAP Accesslog. Zobacz [http://www.openldap.org/doc/admin24/overlays.html#Access rejestrowania](http://www.openldap.org/doc/admin24/overlays.html#Access Logging)
* Wykaz zmian LDAP. Zobacz [http://tools.ietf.org/html/draft-good-ldap-changelog-04](http://tools.ietf.org/html/draft-good-ldap-changelog-04)
* Sygnatura czasowa. Dla Novell/NetIQ eDirectory, łącznik używa ostatniego daty/godziny w celu uzyskania utworzony i zaktualizowane obiekty. Novell/NetIQ eDirectory nie zawiera odpowiednika oznacza pobrać usuniętych obiektów. Tę opcję można również czy żadnej innej metody importu zmian jest aktywna na serwerze LDAP. Ta opcja nie jest w stanie usunięty Importowanie obiektów.
* USNChanged. Zobacz: [https://msdn.microsoft.com/library/ms677627.aspx](https://msdn.microsoft.com/library/ms677627.aspx)

### <a name="not-supported"></a>Nieobsługiwane
Następujące funkcje LDAP nie są obsługiwane:

* Odwołań LDAP między serwerami (RFC 4511/4.1.10)

## <a name="create-a-new-connector"></a>Utwórz nowy łącznik
Aby utworzyć łącznik LDAP ogólnego w **usługi synchronizacji** wybierz **agenta zarządzania** i **Utwórz**. Wybierz **ogólnego LDAP (Microsoft)** łącznika.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericldap/createconnector.png)

### <a name="connectivity"></a>Łączność
Na stronie Łączność należy określić informacje hosta, Port i powiązania. W zależności od tego, czy powiązanie jest wybrany, dodatkowe informacje może zostać dostarczona w poniższych sekcjach.

![Łączność](./media/active-directory-aadconnectsync-connector-genericldap/connectivity.png)

* Ustawienie limitu czasu połączenia jest używana tylko dla pierwszego połączenia z serwerem podczas wykrywania schematu.
* Jeśli powiązanie jest anonimowy, następnie ani nazwy użytkownika / hasło lub certyfikat są używane.
* Dla pozostałych powiązaniach wprowadź informacje albo w nazwę użytkownika / hasło lub wybierz certyfikat.
* Jeśli korzystasz z protokołu Kerberos do uwierzytelniania, określ również obszaru/domeny użytkownika.

**Atrybutu aliasy** pole tekstowe jest używany dla zdefiniowanej w schemacie o składni RFC4522 atrybutów. Te atrybuty nie mogą być wykryte podczas wykrywania schematu i łącznika musi pomoc, aby zidentyfikować te atrybuty. Na przykład w polu aliasy atrybut poprawnie zidentyfikować atrybutu certyfikatu użytkownika jako atrybutu danych binarnych należy podać następujące:

`userCertificate;binary`

Poniżej przedstawiono przykładowy sposób ta konfiguracja może wyglądać jak:

![Łączność](./media/active-directory-aadconnectsync-connector-genericldap/connectivityattributes.png)

Wybierz **zawiera atrybuty operacyjne w schemacie** pole wyboru, aby obejmować atrybuty utworzone przez serwer. Obejmują one atrybuty, takie jak kiedy obiekt został utworzony i godzina ostatniej aktualizacji.

Wybierz **zawiera atrybuty extensible w schemacie** jeśli są używane obiekty rozszerzalne (RFC4512/4.3) i włączenie tej opcji umożliwia każdego atrybutu ma być używany dla wszystkich obiektów. Wybranie tej opcji powoduje, że schemat bardzo duży, chyba że połączonego katalogu jest użycie tej funkcji zalecane jest aby zachować tę opcję niezaznaczoną.

### <a name="global-parameters"></a>Parametry globalne
Na stronie parametry globalne można skonfigurować DN dziennik zmian różnicowych i dodatkowe funkcje LDAP. Strona jest wstępnie wypełniony z informacjami pochodzącymi z serwera LDAP.

![Łączność](./media/active-directory-aadconnectsync-connector-genericldap/globalparameters.png)

Pierwsza sekcja zawiera informacje o serwerze, takie jak nazwa serwera. Łącznik sprawdza także, czy formanty obowiązkowe znajdują się w katalogu głównego elementu DSE. Jeśli te formanty nie są wyświetlane, ostrzeżenia. Niektóre katalogów LDAP nie listę wszystkich funkcji w katalogu głównego elementu DSE i jest możliwe, że łącznik działa bez problemów, nawet jeśli jest obecne ostrzeżenie.

**Obsługiwane formanty** pola wyboru kontrolowania zachowania dla niektórych operacji:

* Drzewo Usuń zaznaczone, hierarchii jest usuwane z jednego wywołania LDAP. Usuń drzewa niezaznaczoną łącznik jest delete cykliczne w razie potrzeby.
* Z wyników stronicowania zaznaczone łącznik działa stronicowanych importu z rozmiar określony na wykonywania czynności.
* VLVControl i SortControl stanowi alternatywę dla pagedResultsControl można odczytać danych z katalogiem LDAP.
* Łącznik importuje wszystkich obiektów w jednej operacji, która może się nie powieść, jeśli jest duża katalog, jeśli wszystkie trzy opcje (pagedResultsControl, VLVControl i SortControl) są wyłączone.
* ShowDeletedControl jest używana tylko w przypadku, gdy Metoda importu zmian jest USNChanged.

Dziennik zmian nazwy domeny jest używany przez dziennik zmian różnicowych, na przykład kontekst nazewnictwa **cn = wykaz zmian**. Należy określić tę wartość, aby można było wykonać import zmian.

Poniżej przedstawiono listę dziennik zmian domyślnego DNs:

| Katalog | Dziennik zmian różnicowych |
| --- | --- |
| Microsoft AD LDS i AD GC |Wykrywane automatycznie. USNChanged. |
| Serwer katalogowy Apache |Nie jest dostępna. |
| Katalog 389 |Dziennik zmian. Domyślna wartość do użycia: **cn = wykaz zmian** |
| IBM Tivoli DS |Dziennik zmian. Domyślna wartość do użycia: **cn = wykaz zmian** |
| Isode katalogu |Dziennik zmian. Domyślna wartość do użycia: **cn = wykaz zmian** |
| Novell/NetIQ eDirectory |Nie jest dostępna. Sygnatura czasowa. Łącznik używa ostatniej aktualizacji daty/godziny, aby uzyskać dodane lub zaktualizowane rekordy. |
| Otwórz DJ/DS. |Dziennik zmian.  Domyślna wartość do użycia: **cn = wykaz zmian** |
| Otwórz LDAP |Dziennik dostępu. Domyślna wartość do użycia: **cn = accesslog** |
| Oracle DSEE |Dziennik zmian. Domyślna wartość do użycia: **cn = wykaz zmian** |
| RadiantOne VDS |Katalog wirtualny. Zależy od katalogu podłączone do usługi dysków wirtualnych. |
| Jeden serwer katalogowy Sun |Dziennik zmian. Domyślna wartość do użycia: **cn = wykaz zmian** |

Nazwa atrybutu, powinien używać łącznika, aby ustawić hasło w przypadku zmiany hasła jest atrybut password i operacje na zestawie hasła.
Ta wartość jest domyślnie ustawiona na **userPassword** , ale można zmienić w razie potrzeby w określonym systemie LDAP.

Na liście dodatkowe partycje jest można dodać dodatkowe przestrzenie nazw, które nie są automatycznie wykrywane. Na przykład to ustawienie może być używane, jeśli kilka serwerów tworzą logicznej klastra, który należy zaimportować wszystkie w tym samym czasie. Tak samo, jak Active Directory może mieć wiele domen w jednym lesie, ale jeden schemat udziału wszystkich domen, można takie same symulowane wprowadzając dodatkowe przestrzenie nazw w tym polu. Każdej przestrzeni nazw można importować z różnych serwerów i dalsze skonfigurowano na stronie Konfiguruj partycje i hierarchie. Użyj klawiszy Ctrl + Enter, aby uzyskać nowy wiersz.

### <a name="configure-provisioning-hierarchy"></a>Konfigurowanie hierarchii zastrzegania
Ta strona służy do mapowania składnik nazwy domeny, na przykład jednostki Organizacyjnej, typ obiektu powinna być przygotowana, na przykład jednostka organizacyjna.

![Inicjowanie obsługi administracyjnej hierarchii](./media/active-directory-aadconnectsync-connector-genericldap/provisioninghierarchy.png)

Konfigurując inicjowania obsługi administracyjnej hierarchii można skonfigurować łącznik, aby automatycznie utworzyć struktury w razie potrzeby. Na przykład, jeśli jest kontrolerem domeny przestrzeń nazw = contoso, dc = com i nowych cn obiektu Jan, ou = = Seattle, c = US, kontrolera domeny = contoso, dc = com jest udostępniony, a następnie łącznik można utworzyć obiektu typu kraju dla Stanów Zjednoczonych i organizationalUnit dla Seattle, jeśli te nie istnieją już w katalogu.

### <a name="configure-partitions-and-hierarchies"></a>Konfiguruj partycje i hierarchie
Na stronie partycje i hierarchie zaznacz wszystkie przestrzenie nazw z obiektami, które chcesz importować i eksportować.

![Partycje](./media/active-directory-aadconnectsync-connector-genericldap/partitions.png)

Dla każdej przestrzeni nazw jest również możliwe do skonfigurowania ustawień łączności przesłonić wartości podane na ekranie łączności. Jeśli te wartości są ich pustą wartość domyślną, informacje na ekranie łączności są używane.

Użytkownik może również wybrać kontenery i jednostki organizacyjne importowanego i eksportowania do łącznika.

Podczas wyszukiwania jest to realizowane przez wszystkie kontenery w partycji. W przypadkach, gdy istnieją dużą liczbę kontenerów to zachowanie powoduje spadek wydajności.

>[!NOTE]
Począwszy od aktualizacji 2017 marca do ogólnego LDAP wyszukiwania łącznika można ograniczony w zakresie do wybranych kontenerów. Można to zrobić, zaznaczając pole wyboru "Wyszukiwanie tylko w kontenerach wybranego", jak pokazano na poniższej ilustracji.

![Wyszukiwanie tylko wybrane kontenerów](./media/active-directory-aadconnectsync-connector-genericldap/partitions-only-selected-containers.png)

### <a name="configure-anchors"></a>Konfiguruj zakotwiczenia
Ta strona ma zawsze wartość wstępnie skonfigurowane i nie można zmienić. W razie znalezienia dostawcy serwera zakotwiczenia może być wypełnione z atrybutem niezmienne, na przykład identyfikator GUID obiektu. Jeśli nie wykryto lub nie ma atrybutu niezmienne, następnie łącznik używa nazwy domeny (nazwa wyróżniająca) jako swojej kotwicy.

![punkty kontrolne](./media/active-directory-aadconnectsync-connector-genericldap/anchors.png)


Oto lista serwerów LDAP i zakotwiczenia używany:

| Katalog | Atrybut zakotwiczenia |
| --- | --- |
| Microsoft AD LDS i AD GC |Atrybut objectGUID |
| Serwer katalogowy 389 |Nazwa wyróżniająca |
| Apache katalogu |Nazwa wyróżniająca |
| IBM Tivoli DS |Nazwa wyróżniająca |
| Isode katalogu |Nazwa wyróżniająca |
| Novell/NetIQ eDirectory |IDENTYFIKATOR GUID |
| Otwórz DJ/DS. |Nazwa wyróżniająca |
| Otwórz LDAP |Nazwa wyróżniająca |
| Oracle ODSEE |Nazwa wyróżniająca |
| RadiantOne VDS |Nazwa wyróżniająca |
| Jeden serwer katalogowy Sun |Nazwa wyróżniająca |

## <a name="other-notes"></a>Inne uwagi
Ta sekcja zawiera informacje o aspektach, które są specyficzne dla tego łącznika lub z innych powodów musisz znać.

### <a name="delta-import"></a>Import zmian
Znak wodny delta w otwartych LDAP jest UTC daty/godziny. Z tego powodu należy zsynchronizować zegary między usługę synchronizacji programu FIM i otwórz LDAP. Jeśli nie może zostać pominięte niektóre wpisy w dzienniku zmian różnicowych.

Dla Novell eDirectory import zmian nie wykrywa usunięcia żadnych obiektów. Z tego powodu należy uruchomić pełny import okresowo, aby znaleźć wszystkie obiekty usunięte.

W przypadku katalogów z dziennika zmian różnicowych, oparty na daty/godziny zaleca Uruchom pełny import w czasie okresowe. Ten proces umożliwia aparatu synchronizacji można znaleźć i nierówności pomiędzy serwera LDAP i co to jest aktualnie przestrzeni łącznika.

## <a name="troubleshooting"></a>Rozwiązywanie problemów
* Aby uzyskać informacje o tym, jak włączyć rejestrowanie rozwiązywać problemy z łącznika, zobacz [jak włączyć ETW Tracing łączników](http://go.microsoft.com/fwlink/?LinkId=335731).
