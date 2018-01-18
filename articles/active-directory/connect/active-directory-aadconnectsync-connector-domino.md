---
title: "Łącznika programu Lotus Domino | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób konfigurowania firmy Microsoft programu Lotus Domino łącznika."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: e07fd469-d862-470f-a3c6-3ed2a8d745bf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/119/2017
ms.author: barclayn
ms.openlocfilehash: 6c412be1c54e0378166791c61469c951bca3a583
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2018
---
# <a name="lotus-domino-connector-technical-reference"></a>Dokumentacja techniczna programu Lotus Domino łącznika
W tym artykule opisano łącznika programu Lotus Domino. Artykuł dotyczy następujących produktów:

* Program Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * Należy użyć poprawki 4.1.3671.0 lub nowszym [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 i FIM2010R2 łącznika jest dostępna do pobrania z [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-lotus-domino-connector"></a>Omówienie łącznika programu Lotus Domino
Łącznik programu Lotus Domino umożliwia integrację usługi synchronizacji z serwerem programu Lotus Domino firmy IBM.

Z punktu widzenia wysokiego poziomu następujące funkcje są obsługiwane w bieżącej wersji łącznika:

| Cecha | Pomoc techniczna |
| --- | --- |
| Połączonego źródła danych |Serwer: <li>Programu Lotus Domino 8.5.x</li><li>Programu Lotus Domino 9.x</li>Klient:<li>Programu Lotus Domino 8.5.x</li><li>Program Lotus Notes 9.x</li> |
| Scenariusze |<li>Zarządzanie cyklem życia obiektów</li><li>Zarządzanie grupami</li><li>Zarządzanie hasłami</li> |
| Operacje |<li>Pełne i różnicowe importu</li><li>Eksportowanie</li><li>Ustaw i Zmień hasło na hasło HTTP</li> |
| Schemat |<li>Osoby (mobilnego użytkownika, skontaktuj się z pomocą (osób z żadnego certyfikatu))</li><li>Grupa</li><li>Zasób (zasobu, miejsca, spotkań Online)</li><li>Poczty w bazie danych</li><li>Dynamiczne odnajdowanie atrybuty obsługiwanych obiektów</li><li>Obsługuje do 250 niestandardowych wydających świadectwa organizacji i jednostek organizacyjnych (OU)</li> |

Łącznik programu Lotus Domino używa klienta programu Lotus Notes do komunikowania się z serwerem programu Lotus Domino. Wyniku tę zależność obsługiwany klient programu Lotus Notes musi być zainstalowany na serwerze synchronizacji. Komunikacja między klientem a serwerem jest implementowane za pośrednictwem interfejsu programu Lotus Notes .NET Interop (Interop.domino.dll). Ten interfejs umożliwia komunikację między platformy Microsoft.NET i klienta programu Lotus Notes i obsługuje dostęp do programu Lotus Domino dokumentów i widoków. Import zmian jego jest również możliwe użytą interfejsu natywnych języka C++ (w zależności od wybranej różnicowej Metoda importu).

### <a name="prerequisites"></a>Wymagania wstępne
Przed użyciem łącznika, upewnij się, że na serwerze synchronizacji, masz następujące wymagania wstępne:

* Framework Microsoft .NET 4.5.2 lub nowszej
* Klient programu Lotus Notes musi być zainstalowany na serwerze synchronizacji
* Łącznik programu Lotus Domino wymaga domyślnej programu Lotus Domino LDAP schematu bazy danych (schema.nsf) znajdować się na serwerze Domino katalogu. Jeśli nie jest obecny, można zainstalować go uruchomiony lub uruchamiając ponownie usługę LDAP na serwerze Domino.

### <a name="connected-data-source-permissions"></a>Połączone uprawnienia źródła danych
Umożliwia wykonywanie zadań z obsługiwanych w łącznika programu Lotus Domino, użytkownik musi należeć do następujących grup:

* Pełny dostęp do administratorów
* Administratorzy
* Administratorzy baz danych

W poniższej tabeli przedstawiono uprawnienia, które są wymagane dla każdej operacji:

| Operacja | Prawa dostępu |
| --- | --- |
| Import |<li>Odczytywać dokumenty publiczne</li><li> Pełny dostęp administratora (podczas jesteś członkiem grupy administratorów pełny dostęp automatycznie masz dostęp czynny na liście ACL.)</li> |
| Eksportowanie i ustawienia hasła |Dostęp czynny: <li>Tworzenie dokumentów</li><li>Usuwanie dokumentów</li><li>Odczytywać dokumenty publiczne</li><li>Zapis dokumenty publiczne</li><li>Replikacja lub kopiowania dokumentów</li>Dla operacji eksportowania potrzebne są także następujące role: <li>Metoda CreateResource</li><li>GroupCreator</li><li>GroupModifier</li><li>UserCreator</li><li>UserModifier</li> |

### <a name="direct-operations-and-adminp"></a>Operacje bezpośredniego i AdminP
Operacje albo przejść bezpośrednio do katalogu Domino lub proces AdminP. W poniższych tabelach przedstawiono listę obsługiwanych obiektów, operacje i, jeśli to konieczne, metoda pokrewne implementacji:

**Książka adresowa podstawowego**

| Obiekt | Przycisk Utwórz | Aktualizacja | Usuwanie |
| --- | --- | --- | --- |
| Osoby |AdminP |Bezpośrednie |AdminP |
| Grupa |AdminP |Bezpośrednie |AdminP |
| MailInDB |Bezpośrednie |Bezpośrednie |Bezpośrednie |
| Zasób |AdminP |Bezpośrednie |AdminP |

**Książka adresowa dodatkowej**

| Obiekt | Przycisk Utwórz | Aktualizacja | Usuwanie |
| --- | --- | --- | --- |
| Osoby |ND |Bezpośrednie |Bezpośrednie |
| Grupa |Bezpośrednie |Bezpośrednie |Bezpośrednie |
| MailInDB |Bezpośrednie |Bezpośrednie |Bezpośrednie |
| Zasób |ND |ND |ND |

Po utworzeniu zasobu, tworzony jest dokument uwagi. Analogicznie gdy zasób zostanie usunięty, dwukierunkowe jest usuwane.

### <a name="ports-and-protocols"></a>Porty i protokoły
IBM Lotus Notes klientów i serwerów Domino komunikują się za pomocą uwagi dotyczące zdalnego procedury wywołania (NRPC) gdzie NRPC należy używać protokołu TCP/IP. Domyślny numer portu jest 1352, ale może zostać zmieniona przez administratora Domino.

### <a name="not-supported"></a>Nieobsługiwane
Następujące operacje nie są obsługiwane w bieżącej wersji łącznika programu Lotus Domino:

* Przenoszenie skrzynek pocztowych między serwerami.

## <a name="create-a-new-connector"></a>Utwórz nowy łącznik
### <a name="client-software-installation-and-configuration"></a>Instalacja oprogramowania klienta i konfiguracji
Program Lotus Notes musi być zainstalowany na serwerze **przed** łącznika jest zainstalowana.

Po zainstalowaniu, pamiętaj, aby **zainstalować pojedynczego użytkownika**. Wartość domyślna **zainstalować wielu użytkowników** nie działa.  
![Notes1](./media/active-directory-aadconnectsync-connector-domino/notes1.png)

Na stronie funkcji, zainstaluj tylko wymagane funkcje programu Lotus Notes i **logowania jednokrotnego klienta**. Wymagany dla łącznika móc zalogować się do serwera Domino jest logowania jednokrotnego.  
![Notes2](./media/active-directory-aadconnectsync-connector-domino/notes2.png)

**Uwaga:** konto usługi uruchamiania programu Lotus Notes po użytkownika, który znajduje się na tym samym serwerze jako konto używane jako łącznika. Upewnij się również zamknąć klienta programu Lotus Notes na serwerze. Nie może ona być uruchomiona w tym samym czasie, który próbuje połączyć się z serwerem Domino łącznika.

### <a name="create-connector"></a>Tworzenie łącznika
Aby utworzyć łącznik programu Lotus Domino w **usługi synchronizacji** wybierz **agenta zarządzania** i **Utwórz**. Wybierz **programu Lotus Domino (Microsoft)** łącznika.  
![CreateConnector](./media/active-directory-aadconnectsync-connector-domino/createconnector.png)

Jeśli swoją wersję usługi synchronizacji oferuje możliwość konfigurowania **architektura**, upewnij się, że łącznik ma ustawioną wartość domyślną do uruchamiania w **procesu**.

### <a name="connectivity"></a>Łączność
Na stronie Łączność należy określić nazwę serwera programu Lotus Domino, a następnie wprowadź poświadczenia logowania.  
![Łączność](./media/active-directory-aadconnectsync-connector-domino/connectivity.png)

Właściwość serwera Domino obsługuje dwa formaty dla nazwy serwera:

* ServerName
* ServerName/DirectoryName

**ServerName/DirectoryName** format jest preferowanym formatem dla tego atrybutu, ponieważ zapewnia szybszej odpowiedzi łącznika kontaktuje się z serwerem Domino.

Podany plik nazwa użytkownika jest przechowywany w bazie danych konfiguracji usługi synchronizacji.

Aby uzyskać **Import zmian** dostępne są następujące opcje:

* **Brak**. Łącznik nie importuje żadnych zmian.
* **Dodawanie aktualizacji**. Import zmian łącznika jest dodać operacje i aktualizacji. Do usunięcia **pełny Import** operacji jest wymagany. Ta operacja jest za pomocą międzyoperacyjności .net.
* **Dodaj/aktualizowania/usuwania**. Import zmian ma łącznika dodawania, aktualizacji i usuwania operacji. Ta operacja jest przy użyciu macierzystych interfejsów C++.

W **opcje schematu** są następujące opcje:

* **Domyślny schemat**. Łącznik wykrywa schematu z serwera Domino. Ta opcja jest opcją domyślną.
* **DSML-Schema**. Używane, jeśli serwer Domino nie ujawnia schematu. Następnie można utworzyć plik DSML ze schematem i zaimportuj go w zamian. Aby uzyskać więcej informacji o DSML, zobacz [OASIS](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=dsml).

Po kliknięciu przycisku Dalej parametry konfiguracji identyfikator użytkownika i hasło są weryfikowane.

### <a name="global-parameters"></a>Parametry globalne
Na stronie parametry globalne skonfigurować strefę czasową i import i export — opcja operacji.  
![Parametry globalne](./media/active-directory-aadconnectsync-connector-domino/globalparameters.png)

**Strefy czasowej serwera Domino** parametr określa lokalizację serwera Domino.

Ta opcja konfiguracji jest wymagany do obsługi **import zmian** operacji ponieważ dzięki usługi synchronizacji Określanie zmian między ostatnich dwóch.

>[!Note]
Począwszy od aktualizacji 2017 marca ekranu parametry globalne obejmuje możliwość usunięcia bazy danych poczty użytkownika podczas usuwania użytkownika.

![Usuwanie skrzynki pocztowej użytkownika](./media/active-directory-aadconnectsync-connector-domino/AdminP.png)

#### <a name="import-settings-method"></a>Importowanie ustawień — metoda
**Wykonaj pełny Import przez** zawiera następujące opcje:

* Wyszukiwanie
* Widok (zalecane)

**Wyszukiwanie** jest przy użyciu indeksowania w Domino, ale jest typowe indeksy nie są aktualizowane w czasie rzeczywistym, a nie z danymi zwróconymi z serwera zawsze jest poprawna. Dla systemu z dużej liczby zmian ta opcja zwykle nie działa prawidłowo i zapewnia usuwa false w niektórych sytuacjach. Jednak **wyszukiwania** jest szybsza niż **widoku**.

**Widok** jest to zalecana opcja, ponieważ zapewnia faktyczny stan danych. Jest nieco wolniej niż wyszukiwania.

#### <a name="creation-of-virtual-contact-objects"></a>Tworzenie wirtualnych obiektów kontaktów
**Umożliwiają tworzenie \_obiektu kontaktu** zawiera następujące opcje:

* Brak
* Wartości-Reference
* Wartości odwołania i -Reference

W Domino atrybutów odwołania może zawierać wielu różnych formatach, odwołując się do innych obiektów. Aby można było do reprezentowania różnych zmian, implementuje łącznika \_skontaktuj się z obiektów, znanej także jako **wirtualnego kontaktów** (VC). Te obiekty są tworzone, można przyłączyć się do istniejących obiektów MV lub przekazywany jako nowych obiektów. W ten sposób odwołania atrybut będzie możliwe.

Po włączeniu tego ustawienia i jeśli zawartość jest atrybut odwołania nie jest formacie nazwa Wyróżniająca \_tworzony jest obiekt kontaktu. Na przykład atrybutem elementu członkowskiego grupy może zawierać adresów SMTP. Istnieje również możliwość nazwa_skrócona i inne atrybuty w atrybutów odwołania. W tym scenariuszu wybierz **wartości inne niż odwołanie**. Ta konfiguracja jest najbardziej typowe ustawienia dla implementacji Domino.

Po skonfigurowaniu programu Lotus Domino mają książki adresowe oddzielne o różnych nazwach wyróżniającej reprezentujący ten sam obiekt jest można również utworzyć \_skontaktuj się z pomocą obiektów, wszystkie wartości odwołania, które znajdują się w książce adresowej. W tym scenariuszu wybierz **odwołania i wartości inne niż odwołanie** opcji.

Jeśli masz wiele wartości w atrybucie **imię i nazwisko** w Domino, następnie chcesz także włącza tworzenie wirtualnych kontaktów, więc odwołań, mogą zostać rozwiązane. Na przykład ten atrybut może mieć wielu wartości po unieważnienie lub rozwodu. Zaznacz pole wyboru **włączyć... Imię i nazwisko ma wiele wartości** dla tego scenariusza.

Po dołączeniu do na prawidłowe atrybuty \_skontaktuj się z pomocą obiektów może być przyłączony do obiektu MV.

Te obiekty mają VC =\_kontakt dodany do ich nazwy domeny.

#### <a name="import-settings-conflict-object"></a>Importowanie ustawień, konflikt obiektu
**Wyklucz obiekty konflikt**

W dużych implementacji Domino jest to możliwe, że wiele obiektów ma tej samej nazwy domeny z powodu problemów z replikacją. W takich przypadkach łącznik zobaczy dwa obiekty o różnych UniversalIDs, ale tej samej nazwy domeny. Ten konflikt spowoduje, że obiekt przejściowej tworzony w przestrzeni łącznika. Łącznik można zignorować obiektów, które zostały wybrane w Domino jako ofiary replikacji. Zaleca się zachowanie zaznaczenie tego pola wyboru.

#### <a name="export-settings"></a>Eksportuj ustawienia
Jeśli opcja **AdminP używać do aktualizowania odwołań** nie jest zaznaczona, eksport atrybutów odwołania, takich jak elementu członkowskiego, bezpośrednie wywołania i nie używa procesu AdminP. Tej opcji należy używać tylko wtedy, gdy AdminP nie został skonfigurowany do obsługi integralności referencyjnej.

#### <a name="routing-information"></a>Informacje dotyczące routingu
W Domino istnieje możliwość, że atrybut odwołania ma informacje routingu osadzony jako sufiks nazwy domeny. Na przykład może zawierać atrybut member w grupie **CN =example/organization@ABC**. Sufiks @ABC znajdują się informacje routingu. Informacje o routingu jest używany przez Domino do wysyłania wiadomości e-mail do poprawny system Domino, który może być systemu w innej organizacji. W polu informacje routingu można określić routing sufiksów używanych w organizacji w zakresie łącznika. Jeśli jeden z tych wartości zostanie znaleziony sufiks atrybut odwołania, routingu informacje zostaną usunięte z odwołania. Jeśli sufiks routingu na wartość odwołania nie można dopasować do jednej z tych wartości, \_tworzony jest obiekt kontaktu. Te \_kontaktu obiekty są tworzone z **RO = @<RoutingSuffix>**  wstawione do nazwy domeny. W tym \_skontaktuj się z obiektów, dodawane są także następujące atrybuty umożliwia dołączenie do rzeczywistego obiektu w razie potrzeby: \_routingName, \_przedstawiciel, \_displayName i UniversalID.

#### <a name="additional-address-books"></a>Dodatkowe książki adresowe
Jeśli nie masz **pomocy katalogu** zainstalowany, która zawiera nazwę książki adresowe dodatkowej, a następnie możesz ręcznie wprowadzić te książki adresowej.

#### <a name="multivalued-transformation"></a>Transformacja wielowartościowego
Wiele w Lotus Domino atrybuty wielowartościowe. Odpowiednie atrybuty metaverse są zwykle jednej wartości. Konfigurowanie importu i opcja operacji eksportowania, należy włączyć łącznik ułatwiające tłumaczenie wymaganych atrybutów.

**Eksportuj**  
Opcja operacji eksportowania obsługuje dwa tryby:

* Dołączanie elementu
* Zastąp element

**Zastąp element** — po wybraniu tej opcji, łącznik zawsze usunąć bieżącej wartości atrybutu Domino i zastąp je z podanych wartości. Podane wartości może być jednowartościowych lub wielokrotne wartości.

Przykład: Atrybut Asystenta obiektu osoba ma następujące wartości:

* CN = Winston/OU=Contoso/O=Americas,NAB=names.nsf Gregowi
* CN = Smith/OU=Contoso/O=Americas,NAB=names.nsf Jan

Jeśli nowego Asystenta o nazwie **Alexander Dominik** jest przypisany do tego obiektu osoby wynik jest:

* CN = Alexander/OU=Contoso/O=Americas,NAB=names.nsf Dominik

**Dołączanie elementu** — po wybraniu tej opcji, łącznik zachowuje istniejące wartości w atrybucie Domino i Wstaw nowe wartości na początku listy danych.

Przykład: Atrybut Asystenta obiektu osoba ma następujące wartości:

* CN = Winston/OU=Contoso/O=Americas,NAB=names.nsf Gregowi
* CN = Smith/OU=Contoso/O=Americas,NAB=names.nsf Jan

Jeśli nowego Asystenta o nazwie **Alexander Dominik** jest przypisany do tego obiektu osoby wynik jest:

* CN = Alexander/OU=Contoso/O=Americas,NAB=names.nsf Dominik
* CN = Winston/OU=Contoso/O=Americas,NAB=names.nsf Gregowi
* CN = Smith/OU=Contoso/O=Americas,NAB=names.nsf Jan

**Importowanie**  
Opcja operacji importu obsługuje dwa tryby:

* Domyślne
* Wielowartościowego do pojedynczej wartości

**Domyślna** — po wybraniu opcji domyślnej, wszystkie wartości wszystkich atrybutów są importowane.

**Wielowartościowego do pojedynczej wartości** — po wybraniu tej opcji wielowartościowy atrybut jest konwertowany na atrybutu jednowartościowego. Jeśli istnieje więcej niż jedną wartość, jest używana wartość pierwszych (Ta wartość jest zazwyczaj również r).

Przykład: Atrybut Asystenta obiektu osoba ma następujące wartości:

* CN = Alexander/OU=Contoso/O=Americas,NAB=names.nsf Dominik
* CN = Winston/OU=Contoso/O=Americas,NAB=names.nsf Gregowi
* CN = Smith/OU=Contoso/O=Americas,NAB=names.nsf Jan

Ostatnia aktualizacja z tym atrybutem jest **Alexander Dominik**. Ponieważ Multivalued do pojedynczej wartości ustawiono opcję operacji importu, łącznik importuje jedynie **Alexander Dominik** do przestrzeni łącznika.

Logiki, aby przekonwertować atrybuty wielowartościowe atrybuty jednowartościowe nie ma zastosowania do atrybutu elementu członkowskiego grupy i atrybut imię i nazwisko osoby.

Możliwe również skonfigurować importowanie i eksportowanie reguł przekształcania dla atrybutów wielowartościowych dla atrybutu, jako wyjątek do globalnej reguły. Aby skonfigurować tę opcję, wprowadź [objecttype]. [attributename] w **zaimportować listy wykluczeń atrybutu** i **wyeksportować listę wykluczeń atrybutu** pól tekstowych. Na przykład wprowadź Person.Assistant globalne flaga jest ustawiona na zaimportować wszystkie wartości, dla Asystenta jest importowany tylko pierwsza wartość.

#### <a name="certifiers"></a>Wydających świadectwa
Wszystkie jednostki organizacyjnej/organizacji są wyświetlane przez łącznik. Aby można było wyeksportować obiekty osoby w książce adresowej podstawowego, certifier z jego hasło jest wymagane.

Jeśli wszystkie wydających świadectwa ma to samo hasło **hasła dla wszystkich Certifers** mogą być używane. Następnie możesz wprowadzić w tym miejscu i tylko określić plik certifier.

Jeśli importowany tylko, nie trzeba określić wszelkie wydających świadectwa.

### <a name="configure-provisioning-hierarchy"></a>Konfigurowanie hierarchii zastrzegania
Podczas konfigurowania łącznika programu Lotus Domino, Pomiń tę stronę okna dialogowego. Łącznik programu Lotus Domino nie obsługuje udostępniania hierarchii.  
![Inicjowanie obsługi administracyjnej hierarchii](./media/active-directory-aadconnectsync-connector-domino/provisioninghierarchy.png)

### <a name="configure-partitions-and-hierarchies"></a>Konfiguruj partycje i hierarchie
Po skonfigurowaniu partycje i hierarchie, musisz wybrać książki adresowej podstawowego o nazwie NAB=names.nsf. Oprócz książki podstawowego adresu książki adresowe dodatkowej można wybrać, jeśli istnieją.  
![Partycje](./media/active-directory-aadconnectsync-connector-domino/partitions.png)

### <a name="select-attributes"></a>Wybierz atrybuty
Po skonfigurowaniu sieci atrybuty, należy wybrać wszystkie atrybuty, które mają przedrostek  **\_MMS\_**. Te atrybuty są wymagane podczas obsługi administracyjnej nowych obiektów do programu Lotus Domino

![Atrybuty](./media/active-directory-aadconnectsync-connector-domino/attributes.png)

## <a name="object-lifecycle-management"></a>Zarządzanie cyklem życia obiektów
Ta sekcja zawiera przegląd różnych obiektów w Domino.

### <a name="person-objects"></a>Obiekty osoby
Obiekt osoby reprezentuje użytkowników w organizacji i jednostek organizacyjnych. Oprócz domyślne atrybuty Domino administrator może dodać niestandardowe atrybuty do obiektu osoby. Co najmniej obiektu osoby musi zawierać wszystkie wymagane atrybuty. Aby uzyskać pełną listę atrybutów obowiązkowych, zobacz [programu Lotus Notes właściwości](#lotus-notes-properties). Aby zarejestrować obiekt osoby, muszą być spełnione następujące wymagania wstępne:

* Książka adresowa (names.nsf) musi została określona i powinna być w książce adresowej podstawowego.
* Musi mieć certifier O/OU identyfikator i hasło, aby zarejestrować dany użytkownik w organizacji / jednostka organizacyjna.
* Musisz ustawić określony zbiór właściwości programu Lotus Notes dla obiekt osoby. Te właściwości są używane do inicjowania obsługi administracyjnej obiektu osoby. Aby uzyskać więcej informacji, zobacz sekcję o nazwie [programu Lotus Notes właściwości](#lotus-notes-properties) dalszej części tego dokumentu.
* Hasła początkowego HTTP dla danej osoby jest atrybut i zestaw podczas inicjowania obsługi.
* Obiekt osoby musi być jedną z następujących trzech obsługiwane typy:
  1. Użytkownik normalny z pliku poczty i plik identyfikator użytkownika
  2. Mobilnych użytkownika (zwykle użytkownik, który zawiera wszystkie pliki bazy danych mobilnych)
  3. Kontakty (użytkownika z żadnego pliku id)

Osoby (z wyjątkiem kontaktów) można grupować w nam użytkownicy i międzynarodowej dalsze określone przez wartość \_MMS\_IDRegType właściwości. Przy użyciu klienta uwagi dotyczące dostępu do serwerów programu Lotus Domino te osoby mają identyfikatora uwagi i zarządzania dokumentami osoby. Jeśli używają poczty uwagi, następnie mają także plik poczty. Użytkownik musi być zarejestrowana staje się aktywny. Aby uzyskać więcej informacji, zobacz:

* [Konfigurowanie użytkowników o wersji](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_SETTING_UP_NOTES_USERS.html)
* [Rejestracja użytkownika](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_REGISTERING_USERS.html)
* [Zarządzanie użytkownikami](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_USERS_5151.html)
* [Zmiana nazwy użytkowników](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_USER_AUTOMATICALLY.html)

Te operacje są wykonywane w Lotus Domino i następnie zaimportować do usługi synchronizacji.

### <a name="resources-and-rooms"></a>Zasoby i pomieszczenia
Zasób jest innego typu z bazą danych programu Lotus Domino. Zasoby mogą być sal konferencyjnych z różnymi typami urządzeń, takich jak projektorów. Istnieją podtypów zasobów obsługiwane przez łącznik programu Lotus Domino, które są zdefiniowane w atrybucie typu zasobu:

| Typ zasobu | Atrybut typu zasobu |
| --- | --- |
| Miejsca |1 |
| Zasób (inny) |2 |
| Spotkań online |3 |

Dla typu obiektu zasobów do pracy, wymagane są następujące elementy:

* Baza danych zasobów rezerwacji powinna już istnieć w połączonym serwerze Domino
* Witryny jest już zdefiniowany dla zasobu

Baza danych rezerwacji zasobów zawiera trzy typy dokumentów:

* Profil lokacji
* Zasób
* Rezerwacja

Aby uzyskać więcej informacji na temat konfigurowania rezerwacji zasobów bazy danych, zobacz [konfigurowania bazy danych zasobów zastrzeżenia](https://www-01.ibm.com/support/knowledgecenter/SSKTMJ_8.0.1/com.ibm.help.domino.admin.doc/DOC/H_SETTING_UP_THE_RESOURCE_RESERVATIONS_DATABASE.html).

**Tworzenia, aktualizacji i usuwania zasobów**  
Operacje tworzenia, aktualizowania lub usuwania są wykonywane przez łącznik programu Lotus Domino w bazie danych rezerwacji zasobów. Zasoby są tworzone jako dokumenty w Names.nsf (to znaczy, że książka adresowa głównej). Aby uzyskać więcej informacji na temat edytowanie i usuwanie zasobów, zobacz [edytowanie i usuwanie dokumentów zasobu](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_EDITING_AND_DELETING_RESOURCE_DOCUMENTS.html).

**Importowanie i eksportowanie operacji dla zasobów**  
Zasoby można zaimportować na i wyeksportowany z usługi synchronizacji, takie jak dowolny inny typ obiektu. Wybierz typ obiektu jako zasób podczas konfiguracji. Dla operacji eksportowania pomyślnie powinien mieć szczegółowe informacje dotyczące typu zasobu, konferencji bazy danych i nazwa lokacji.

### <a name="mail-in-databases"></a>Poczty w bazach danych
Baza danych w poczty jest bazy danych, która służy do odbierania wiadomości. Jest skrzynki pocztowej programu Lotus Domino, która nie jest skojarzony z dowolnego określonego konta użytkownika programu Lotus Domino (to znaczy, że nie ma własny plik ID i hasło). Poczty w bazie danych ma unikatowy identyfikator użytkownika ("krótkiej nazwy") z nią związane oraz adres e-mail.

Jeśli istnieje potrzeba oddzielne skrzynki pocztowej swój własny adres e-mail, które mogą być współużytkowane przez różnych użytkowników (na przykład group@contoso.com), zostanie utworzona baza danych w wiadomości. Dostęp do tej skrzynki pocztowej jest kontrolowany za pośrednictwem jego listy kontroli dostępu (ACL), która zawiera nazwy użytkowników uwagi, które mogą otworzyć skrzynki pocztowej.

Aby uzyskać listę wymaganych atrybutów, zobacz sekcję o nazwie [atrybutów obowiązkowych](#mandatory-attributes) dalszej części tego artykułu.

W przypadku bazy danych służy do odbierania wiadomości e-mail, dokumentów poczty w bazie danych jest tworzony w Lotus Domino. Ten dokument musi istnieć w katalogu Domino na każdym serwerze, który przechowuje kopię bazy danych. Aby uzyskać szczegółowy opis o tworzeniu dokumentów w poczty bazy danych, zobacz [tworzenia dokumentu poczty w bazie danych](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_A_MAILIN_DATABASE_DOCUMENT_FOR_A_NEW_DATABASE_OVERVIEW.html).

Przed utworzeniem poczty w bazie danych, powinna już istnieć bazy danych (powinna zostać utworzona przez administratora programu Lotus) na serwerze Domino.

### <a name="group-management"></a>Zarządzanie grupami
Szczegółowe omówienie zarządzania grupy programu Lotus Domino można uzyskać z następujących zasobów:

* [Przy użyciu grup](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_USING_GROUPS_OVER.html)
* [Tworzenie grupy](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS_MIDTOPIC_55038956829238418.html)
* [Tworzenie i modyfikowanie grup](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS.html)
* [Zarządzanie grupami](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_GROUPS_1804.html)
* [Zmiana nazwy grupy](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_GROUP_STEPS.html)

### <a name="password-management"></a>Zarządzanie hasłami
W przypadku zarejestrowanego użytkownika programu Lotus Domino istnieją dwa typy haseł:

1. Hasło użytkownika (przechowywane w pliku User.id)
2. Internet / HTTP hasła

Łącznik programu Lotus Domino obsługuje tylko operacje hasłem HTTP.

Aby wykonać zarządzania hasłami, należy włączyć zarządzanie hasłami do łącznika w Projektancie agenta zarządzania. Aby włączyć zarządzanie hasłami, zaznacz **włączyć zarządzanie hasłami** na **skonfigurować rozszerzenia** strony okna dialogowego.  
![Konfigurowanie rozszerzeń](./media/active-directory-aadconnectsync-connector-domino/configureextensions.png)

Obsługa łącznika programu Lotus Domino następujące operacje na Internet hasło:

* Ustaw hasło: Ustaw hasło ustawia użytkownika w Domino nowe hasło HTTP w Internecie. Domyślnie konto jest także odblokowane. Flaga unlock jest narażony na interfejsie WMI aparatu synchronizacji.
* Zmień hasło: W tym scenariuszu użytkownik może być wymagana zmiana hasła lub jest monit o zmianę hasła po określonym czasie. Dla tej operacji do wykonania należy umieścić, zarówno (stary i nowe hasło) są obowiązkowe. Po zmianie, nowe hasło jest aktualizowana w Lotus Domino.

Aby uzyskać więcej informacji, zobacz:

* [Przy użyciu funkcji blokady Internet](http://www.ibm.com/developerworks/lotus/library/domino8-lockout/)
* [Zarządzanie hasłami Internet](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_NOTES_AND_INTERNET_PASSWORD_SYNCHRONIZATION_7570_OVER.html)

## <a name="reference-information"></a>Informacje referencyjne
Ta sekcja zawiera informacje takie jak opisy atrybutów i wymagania dotyczące atrybutów dla łącznika programu Lotus Domino.

### <a name="lotus-notes-properties"></a>Właściwości programu Lotus Notes
Podczas obsługi administracyjnej osoby obiektów do katalogu programu Lotus Domino obiektów musi mieć określony zestaw właściwości z określonymi wartościami wypełnione. Te wartości są wymagane tylko dla operacji tworzenia.

Poniższa tabela zawiera listę tych właściwości i opisano je.

| Właściwość | Opis |
| --- | --- |
| \_MMS_AltFullName |Alternatywne pełną nazwę użytkownika. |
| \_MMS_AltFullNameLanguage |Język do zastosowania w przypadku określenia alternatywnej pełną nazwę użytkownika. |
| \_MMS_CertDaysToExpire |Liczba dni od bieżącej daty przed certyfikat wygaśnie. Jeśli nie zostanie określony, domyślna data jest dwóch lat od bieżącej daty. |
| \_MMS_Certifier |Właściwość, która zawiera nazwę organizacji hierarchii certifier. Na przykład: OU = OrganizationUnit, O = Org, C = kraju. |
| \_MMS_IDPath |Jeśli właściwość jest pusta, plik identyfikator użytkownika nie zostanie utworzony lokalnie na serwerze synchronizacji. Jeśli właściwość zawiera nazwę pliku, plik identyfikator użytkownika jest tworzony w folderze madata. Właściwość może również zawierać pełną ścieżkę. |
| \_MMS_IDRegType |Osoby mogą być klasyfikowane jako kontakty, nam użytkowników i międzynarodowe użytkowników. W poniższej tabeli przedstawiono możliwe wartości: <li>0 — kontakt</li><li>1 - użytkownik stany USA</li><li>2 - międzynarodowe użytkownika</li> |
| \_MMS_IDStoreType |Wymagana właściwość amerykańskie i międzynarodowe użytkowników. Ta właściwość zawiera wartość całkowitą, która określa, czy identyfikator użytkownika jest przechowywana jako załącznik w książce adresowej notatki lub w pliku mail osoby. Jeśli plik identyfikator użytkownika jest załącznik w książce adresowej, opcjonalnie można go utworzyć jako plik z \_MMS_IDPath. <li>Empty — identyfikator magazynu plików w magazynie identyfikator, plik nie identyfikacji (używanych do kontaktów).</li><li> 1 - załącznik w książce adresowej uwagi. \_Musi być ustawiona właściwość MMS_Password plików identyfikacji użytkownika, które są załączników</li><li>2 — identyfikator sklepu w pliku Mail osoby. \_MMS_UseAdminP musi mieć ustawioną na false let pliku poczty, można utworzyć podczas rejestracji osoby. \_Musi być ustawiona właściwość MMS_Password plików identyfikacji użytkownika.</li> |
| \_MMS_MailQuotaSizeLimit |Wyrażony w megabajtach, które są dozwolone dla plików bazy danych poczty e-mail. |
| \_MMS_MailQuotaWarningThreshold |Wyrażony w megabajtach, które są dozwolone dla plików bazy danych poczty e-mail, zanim pojawi się ostrzeżenie. |
| \_MMS_MailTemplateName |Plik szablonu e-mail służy do tworzenia pliku wiadomości e-mail użytkownika. Jeśli szablon zostanie określona, plik poczty jest tworzony, używając określonego szablonu. Jeśli szablon nie zostanie określona, plik szablonu domyślnego służy do tworzenia pliku. |
| \_MMS_OU |Opcjonalna właściwość jest nazwa jednostki Organizacyjnej, w obszarze certifier. Ta właściwość powinna być pusta kontaktów. |
| \_MMS_Password |Wymagana właściwość dla użytkowników. Ta właściwość zawiera hasło dla pliku identyfikator obiektu. |
| \_MMS_UseAdminP |Właściwość powinna być równa true, jeśli plik poczty należy utworzyć przez proces AdminP na serwerze Domino (asynchronicznej do procesu eksportu). Jeśli właściwość jest ustawiona na wartość false, plik poczty jest tworzone przez Domino użytkownika (synchroniczne w procesie eksportu). |

Dla użytkownika z pliku skojarzony identyfikator \_właściwości MMS_Password musi zawierać wartość. Aby uzyskać dostęp do poczty e-mail za pomocą klienta programu Lotus Notes MailServer i MailFile właściwości użytkownika musi zawierać wartość.

Dostęp do poczty e-mail za pośrednictwem przeglądarki sieci Web, następujące właściwości muszą zawierać wartości:

* MailFile - wymaganą właściwość, która zawiera ścieżkę na serwerze programu Lotus Domino przechowywania plików wiadomości.
* MailServer - wymaganą właściwość, która zawiera nazwę serwera programu Lotus Domino. Ta wartość jest nazwa do użycia podczas tworzenia pliku poczty programu Lotus Domino serwera.
* HTTPPassword — opcjonalnie właściwość, która zawiera hasło dostępu do sieci Web dla obiekt.

Dostęp do serwera Domino bez możliwości poczty, właściwość HTTPPassword musi zawierać wartość. Właściwości MailFile i MailServer może być pusta.

Z \_MMS_ IDStoreType = 2 (identyfikator magazynu w pliku poczty), właściwość MailSystem NotesRegistrationclass ustawiono REG_MAILSYSTEM_INOTES (3).

### <a name="mandatory-attributes"></a>Atrybutów obowiązkowych
Łącznik programu Lotus Domino głównie obsługuje następujące typy obiektów (typów dokumentów):

* Grupa
* Poczty w bazie danych
* Osoby
* Skontaktuj się z pomocą (osoba mająca nie certifier)
* Zasób

Ta sekcja zawiera listę atrybutów, które są wymagane dla każdego obsługiwanego obiektu można wyeksportować do serwera Domino.

| Typ obiektu | Atrybutów obowiązkowych |
| --- | --- |
| Grupa |<li>ListName</li> |
| Główny w bazie danych |<li>Imię i nazwisko</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li> |
| Osoby |<li>LastName</li><li>MailFile</li><li>Nazwa_skrócona</li><li>\_MMS_Password</li><li>\_MMS_IDStoreType</li><li>\_MMS_Certifier</li><li>\_MMS_IDRegType</li><li>\_MMS_UseAdminP</li> |
| Skontaktuj się z pomocą (osoba mająca nie certifier) |<li>\_MMS_IDRegType</li> |
| Zasób |<li>Imię i nazwisko</li><li>ResourceType</li><li>ConfDB</li><li>ResourceCapacity</li><li>Witryna</li><li>Nazwa wyświetlana</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li> |

## <a name="common-issues-and-questions"></a>Najczęściej występujących problemów i pytania
### <a name="schema-detection-does-not-work"></a>Wykrywanie schematu nie działa.
Aby można było wykryć schematu, jest to konieczne, że plik schema.nsf znajduje się na serwerze Domino. Ten plik jest wyświetlany tylko, jeśli LDAP jest zainstalowany na serwerze. Jeśli schemat nie jest wykrywalny, sprawdź następujące informacje:

* Schema.nsf plik znajduje się w folderze głównym serwera Domino
* Użytkownik ma uprawnienia do znajduje się w pliku schema.nsf.
* Wymusić ponowne uruchomienie serwera LDAP. Otwórz **programu Lotus Domino konsoli** i użyj **Poinformuj ReloadSchema LDAP** polecenie, aby ponownie załadować schemat.

### <a name="not-all-secondary-address-books-are-visible"></a>Nie wszystkie książki adresowe dodatkowej są widoczne
Łącznik Domino zależy od funkcji **pomocy katalogu** aby można było znaleźć książki adresowe dodatkowej. Jeśli brakuje książki adresowe dodatkowej, sprawdź, czy [pomocy katalogu](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_DIRECTORY_ASSISTANCE.html) zostało włączone i skonfigurowane na serwerze Domino.

### <a name="custom-attributes-in-domino"></a>Atrybuty niestandardowe w Domino
Istnieje kilka metod w Domino rozszerzania schematu, więc jest widoczny jako atrybut niestandardowy eksploatacyjnych przez łącznik.

**Podejście 1: Rozszerzenie schematu programu Lotus Domino**

1. Utwórz kopię szablonu katalogu Domino {PUBNAMES. NTF} wykonując [te kroki](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (należy nie dostosować domyślny katalog IBM Lotus Domino szablonu):
2. Otwórz szablon katalogu kopii Domino {CONTOSO. Szablon NTF}, który został utworzony w Projektancie Domino i wykonaj następujące kroki:
   * Kliknij pozycję udostępnione elementy, a następnie rozwiń węzeł podformularze
   * Kliknij dwukrotnie podformularz InheritableSchema ${ObjectName} ({ObjectName} w przypadku nazwę domyślnej klasy obiektu strukturalnego, na przykład: osoby).
   * Nazwa atrybutu, który chcesz dodać do schematu {MyPersonAtrribute} i odpowiednie do tego atrybutu. Utwórz pole przez wybierz **Utwórz** Menu, a następnie wybierz **pola** z menu.
   * W polu dodane ustawienia swoich właściwości, wybierając jego typ, Style, rozmiar, czcionki i inne powiązane parametry w oknie właściwości pola.
   * Zachowanie atrybutu wartością domyślną takie same jak nazwa podana dla tego atrybutu (na przykład, jeśli nazwa atrybutu jest MyPersonAttribute, zachować wartość domyślną o takiej samej nazwie).
   * Zapisz zaktualizowane wartości podformularz InheritableSchema ${ObjectName}.
3. Zastąp szablon katalogu Domino {PUBNAMES. NTF} nowy szablon dostosowane {CONTOSO. NTF} wykonując [te kroki](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
4. Zamknij Domino administratora i Otwórz konsolę Domino, aby ponownie uruchomić usługę LDAP i ponownie załadować schemat LDAP:
   * W konsoli Domino Wstaw polecenia w obszarze **polecenia Domino** tekst złożony, aby ponownie uruchomić usługę LDAP - [Uruchom ponownie zadanie LDAP](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html).
   * Aby ponownie załadować LDAP schematu użyj polecenia Poinformuj LDAP - Poinformuj ReloadSchema LDAP
5. Otwórz administratora Domino i wybierz kartę osoby i grupy, aby zobaczyć, dodane atrybuty jest widoczny w domino Dodaj osobę.
6. Otwórz Schema.nsf z **pliki** karcie i zobacz, dodane atrybuty jest uwzględniane w klasie obiektów dominoPerson LDAP.

**Podejście 2: Tworzenie auxClass z atrybutu niestandardowego i skojarzyć z klasy obiektów**

1. Utwórz kopię szablonu katalogu Domino {PUBNAMES. NTF} wykonując [te kroki](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (Dostosowywanie nigdy nie domyślny katalog IBM Lotus Domino szablonu):
2. Otwórz szablon katalogu kopii Domino {CONTOSO. Szablon NTF}, który został utworzony w Projektancie Domino.
3. W okienku po lewej stronie wybierz współużytkowanego kodu, a następnie podformularze.
4. Kliknij przycisk nowy podformularz
5. Wykonaj następujące polecenie, aby określić właściwości dla nowego podformularza:
   * Nowy podformularz otwarty, wybierz projekt - podformularz właściwości
   * Obok właściwości Name wprowadź nazwę dla klasy pomocnicze obiektów — na przykład TestSubform.
   * Zachowaj właściwości Options "Dołącz podformularza Insert... okna dialogowego" wybrane
   * Usuń zaznaczenie opcji właściwości "renderowania przekazywania HTML w notatkach."
   * Inne właściwości pozostaną bez zmian, a następnie zamknij okno podformularza właściwości.
   * Zapisz i zamknij nowy podformularz.
6. Wykonaj następujące czynności, aby dodać pole w celu zdefiniowania pomocnicze obiektu klasy:
   * Otwórz podformularz, który został utworzony.
   * Wybierz opcję Utwórz — pole.
   * Obok nazwy na karcie podstawy okna dialogowego pola, określić dowolną nazwę, na przykład: {MyPersonTestAttribute}.
   * W polu dodane ustawienia swoich właściwości, wybierając typ, Style, rozmiar, czcionki i powiązanych właściwości.
   * Zachowanie atrybutu wartością domyślną takie same jak nazwa podana dla tego atrybutu (na przykład, jeśli nazwa atrybutu jest MyPersonTestAttribute, zachować wartość domyślną o takiej samej nazwie).
   * Zapisz podformularza z zaktualizowane wartości i wykonaj następujące czynności:
     * W okienku po lewej stronie wybierz współużytkowanego kodu, a następnie podformularze
     * Wybierz nowy podformularz i wybierz projekt - właściwości projektu.
     * Kliknij kartę trzeci z lewej strony, a następnie wybierz **propagację zakaz zmianę projektu**.
7. Otwórz podformularzu ExtensibleSchema ${ObjectName} ({ObjectName} w przypadku nazwę domyślnej klasy obiektu strukturalnego, na przykład — osoby).
8. Wstaw zasób i wybierz podformularz (który został utworzony, na przykład — TestSubform) i Zapisz podformularz ExtensibleSchema ${ObjectName}.
9. Zastąp szablon katalogu Domino {PUBNAMES. NTF} nowy szablon dostosowane {CONTOSO. NTF} wykonując [te kroki](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
10. Zamknij Domino administratora i Otwórz konsolę Domino, aby ponownie uruchomić usługę LDAP i ponownie załadować schemat LDAP:
    * W konsoli Domino Wstaw polecenia w obszarze **polecenia Domino** tekst złożony, aby ponownie uruchomić usługę LDAP - [Uruchom ponownie zadanie LDAP](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html).
    * Aby ponownie załadować LDAP schematu użyj polecenia Poinformuj LDAP **Poinformuj ReloadSchema LDAP**.
11. Otwórz administratora Domino i wybierz kartę osoby i grupy, aby zobaczyć, dodane atrybuty jest widoczny w domino Dodaj osobę (w obszarze innym tabulacji).
12. Otwórz Schema.nsf z **pliki** karcie i zobacz, dodane atrybuty jest odzwierciedlone w obszarze TestSubform LDAP pomocnicze klasy obiektów.

**Podejście 3: Dodaj atrybut niestandardowy do klasy ExtensibleObject**

1. Otwórz plik {Schema.nsf} umieszczony w katalogu głównym
2. Wybierz klasy obiektu LDAP z menu po lewej stronie w obszarze **wszystkie dokumenty schematu** i kliknij przycisk **Dodaj obiekt klasy** przycisk:
3. Podaj nazwę LDAP w formie {zzzExtensibleSchema} (gdzie zzz jest nazwą domyślną klasę obiektu strukturalnego, na przykład osoby). Na przykład aby rozszerzyć schemat dla osoby klasę obiektu, należy podać nazwę LDAP {PersonExtensibleSchema}.
4. Podaj nazwę klasy obiekt nadrzędny, dla którego chcesz rozszerzyć schemat. Na przykład aby rozszerzyć schemat dla klasy obiektu osoby, należy podać nazwę klasy obiekt nadrzędny {dominoPerson}:
5. Podaj prawidłowy identyfikator OID odpowiadający klasy obiektów.
6. Wybierz atrybuty rozszerzone/niestandardowe w obszarze obowiązkowe i opcjonalne typy atrybutów pola zgodnie z harmonogramem wymaganie:
7. Po dodaniu wymaganych atrybutów do ExtensibleObjectClass, kliknij przycisk **Zapisz i Zamknij**.
8. ExtensibleObjectClass jest tworzona dla odpowiednich domyślną klasę obiektu z rozszerzonymi atrybutami.

## <a name="troubleshooting"></a>Rozwiązywanie problemów
* Aby uzyskać informacje o tym, jak włączyć rejestrowanie rozwiązywać problemy z łącznika, zobacz [jak włączyć ETW Tracing łączników](http://go.microsoft.com/fwlink/?LinkId=335731).
