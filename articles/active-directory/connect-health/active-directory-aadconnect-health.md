---
title: "Monitorowanie lokalnej infrastruktury do obsługi tożsamości w chmurze."
description: "Jest to strona dotycząca programu Azure AD Connect Health, na której opisano, czym on jest i do czego służy."
services: active-directory
documentationcenter: 
author: karavar
manager: samueld
editor: curtand
ms.assetid: 82798ea6-5cd3-4f30-93ae-d56536f8d8e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/18/2016
ms.author: vakarand
translationtype: Human Translation
ms.sourcegitcommit: c9ebe21acbe06aa25bcadb3b500c34cf89154388
ms.openlocfilehash: 9abdfa7349bee74b89a23663969904110c730395


---
# <a name="monitor-your-on-premises-identity-infrastructure-and-synchronization-services-in-the-cloud"></a>Monitorowanie lokalnej infrastruktury do obsługi tożsamości i usług synchronizacji w chmurze
Program Azure AD Connect Health pomaga monitorować i zdobywać informacje o lokalnej infrastrukturze do obsługi tożsamości oraz usługach synchronizacji.  Umożliwia utrzymywanie niezawodnego połączenia z usługami Office 365 i Microsoft Online Services, udostępniając funkcje monitorowania kluczowych składników tożsamości, takich jak serwery usług AD FS, serwery programu Azure AD Connect (nazywane aparatem synchronizacji), kontrolery domeny usługi Active Directory itp. Zapewnia także łatwy dostęp do kluczowych punktów danych dotyczących tych składników, ułatwiając zdobywanie informacji o użyciu i innych istotnych parametrach, co pozwala na świadome podejmowanie decyzji.

Informacje są prezentowane w [portalu programu Azure AD Connect Health](https://aka.ms/aadconnecthealth). Korzystając z portalu programu Azure AD Connect Health, możesz wyświetlać alerty, wyniki monitorowania wydajności, analizy użycia itd. Program Azure AD Connect Health pokazuje kondycję kluczowych składników tożsamości w jednym miejscu.

![Co to jest program Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnecthealth2.png)

Przyszłe aktualizacje programu Azure AD Connect Health będą zawierały dodatkowe funkcje monitorowania i wgląd w dodatkowe składniki tożsamości. Dzięki temu uzyskasz jeden pulpit dotyczący tożsamości zapewniający jeszcze bardziej niezawodne, będące w dobrej kondycji i zintegrowane środowisko, którego użytkownicy mogą używać do zwiększania wydajności pracy.

## <a name="why-use-azure-ad-connect-health"></a>Dlaczego warto korzystać z programu Azure AD Connect Health
Zintegrowanie katalogów lokalnych z usługą Azure AD zwiększa produktywność użytkowników, zapewniając wspólną tożsamość na potrzeby dostępu do zasobów, zarówno lokalnych, jak i w chmurze. Jednak tego rodzaju integracja wiąże się z koniecznością zapewnienia dobrej kondycji środowiska, aby użytkownicy mieli niezawodny dostęp z dowolnego urządzenia do zasobów lokalnych i znajdujących się w chmurze. Program Azure AD Connect Health oferuje łatwe, oparte na chmurze podejście do monitorowania i zdobywania informacji o lokalnej infrastrukturze do obsługi tożsamości, która umożliwia dostęp do usługi Office 365 lub innych aplikacji usługi Azure AD. Wymaga to jedynie zainstalowania agenta na każdym z lokalnych serwerów tożsamości.

## <a name="azure-ad-connect-health-for-ad-fsactive-directory-aadconnect-health-adfsmd"></a>[Program Azure AD Connect Health dla usług AD FS](active-directory-aadconnect-health-adfs.md)
Program Azure AD Connect Health dla usług AD FS obsługuje usługi AD FS 2.0 w systemie Windows Server 2008 R2 oraz usługi AD FS w systemach Windows Server 2012 i Windows Server 2012 R2. Obsługuje także monitorowanie serwerów proxy usług AD FS lub serwerów proxy aplikacji sieci Web, które zapewniają obsługę uwierzytelniania w przypadku dostępu do ekstranetu. Dzięki prostej i niedrogiej instalacji agenta kondycji program Azure AD Connect Health dla usług AD FS oferuje następujący zestaw kluczowych funkcji:

* Monitorowanie z alertami, które informują, kiedy serwery usług AD FS i serwery proxy usług AD FS nie są w dobrej kondycji
* Powiadomienia e-mail dotyczące alertów krytycznych
* Wyświetlanie trendów w danych dotyczących wydajności, co jest przydatne do planowania wydajności usług AD FS
* Analizy użycia dotyczące różnych wariantów logowania się do usług AD FS (aplikacje, użytkownicy, lokalizacja sieciowa itp.), co pomaga zrozumieć sposób korzystania z usług AD FS.
* Raporty dotyczące usług AD FS, na przykład lista 50 użytkowników, którzy najczęściej nieprawidłowo podawali nazwę użytkownika/hasło, wraz z ostatnim adresem IP

Poniższy klip wideo zawiera omówienie programu Azure AD Connect Health dla usług AD FS

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD-Connect-Health--Monitor-you-identity-bridge/player]
>
>

## <a name="azure-ad-connect-health-for-syncactive-directory-aadconnect-health-syncmd"></a>[Program Azure AD Connect Health do celów synchronizacji](active-directory-aadconnect-health-sync.md)
Program Azure AD Connect Health do celów synchronizacji monitoruje i udostępnia informacje na temat procesów synchronizacji, które mają miejsce między lokalną usługą Active Directory i usługą Azure Active Directory. Program Azure AD Connect Health do celów synchronizacji oferuje następujący zestaw kluczowych funkcji:

* Monitorowanie z alertami, które informują, kiedy serwery programu Azure AD Connect zwane aparatem synchronizacji nie są w dobrej kondycji
* Powiadomienia e-mail dotyczące alertów krytycznych
* Wgląd w dane operacyjne dotyczące synchronizacji, między innymi wykresy opóźnień operacji synchronizacji i trendy w innych operacjach, takich jak dodawanie, aktualizowanie i usuwanie.
* Przegląd informacji na temat właściwości synchronizacji i ostatniego pomyślnego eksportu do usługi Azure AD
* Raporty o błędach synchronizacji na poziomie obiektu \(nie wymagają usługi Azure AD w wersji Premium\)

Poniższy klip wideo zawiera omówienie programu Azure AD Connect Health do celów synchronizacji

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-Health-Monitoring-the-sync-engine/player]
>
>

## <a name="azure-ad-connect-health-for-ad-ds-previewactive-directory-aadconnect-health-addsmd"></a>[Program Azure AD Connect Health dla usług AD DS (wersja zapoznawcza)](active-directory-aadconnect-health-adds.md)
Program Azure AD Connect Health dla usług AD DS oferuje funkcje monitorowania kontrolerów domeny zainstalowanych w systemie Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 i Windows Server 2016. Łatwa i ekonomiczna instalacja agenta kondycji umożliwia monitorowanie lokalnego środowiska usług AD DS prosto z chmury. Program Azure AD Connect Health dla usług AD DS oferuje następujący zestaw kluczowych funkcji:

* Monitorowanie alertów w celu wykrywania złej kondycji kontrolerów domeny wraz z powiadomieniami e-mail w przypadku alertów krytycznych.
* Pulpit nawigacyjny Kontrolery domeny, który zapewnia szybki wgląd w kondycję i stan operacyjny kontrolerów domeny.
* Pulpit nawigacyjny Stan replikacji zawierający najnowsze informacje o replikacji wraz z linkami do poradników rozwiązywania problemów w przypadku wykrycia błędów.
* Szybki dostęp z dowolnego miejsca do wykresów danych wydajności utworzonych na podstawie popularnych liczników wydajności, które są niezbędne do celów monitorowania i rozwiązywania problemów.

Poniższy klip wideo zawiera omówienie programu Azure AD Connect Health dla usług AD DS

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD-Connect-Health-monitors-on-premises-AD-Domain-Services/player]
>
>

## <a name="get-started-with-azure-ad-connect-health"></a>Wprowadzenie do programu Azure AD Connect Health
Rozpoczęcie pracy z programem Azure AD Connect Health jest bardzo proste. Wykonaj poniższe kroki:

1. [Uzyskaj usługę Azure AD w wersji Premium](../active-directory-get-started-premium.md) lub [rozpocznij okres próbny](https://azure.microsoft.com/trial/get-started-active-directory/)
2. [Pobierz i zainstaluj agentów programu Azure AD Connect Health](#download-and-install-azure-ad-connect-health-agent) na serwerach tożsamości.
3. Wyświetl pulpit nawigacyjny programu Azure AD Connect Health na stronie [https://aka.ms/aadconnecthealth](https://aka.ms/aadconnecthealth).

> [!NOTE]
> Pamiętaj, że przed wyświetleniem jakichkolwiek danych na pulpicie nawigacyjnym programu Azure AD Connect Health musisz zainstalować agentów programu Azure AD Connect Health na serwerach docelowych.
>
>

## <a name="download-and-install-azure-ad-connect-health-agent"></a>Pobieranie i instalowanie agenta programu Azure AD Connect Health
* Sprawdzanie, czy wymagania programu Azure AD Connect Health zostały spełnione
* Aby rozpocząć korzystanie z programu Azure AD Connect Health dla usług AD FS, możesz pobrać najnowszą wersję agenta tutaj: [Pobieranie agenta programu Azure AD Connect Health dla usług AD FS](http://go.microsoft.com/fwlink/?LinkID=518973).
  [](active-directory-aadconnect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs)
* Aby rozpocząć korzystanie z programu Azure AD Connect Health do celów synchronizacji, pobierz i zainstaluj [najnowszą wersję programu Azure AD Connect](http://go.microsoft.com/fwlink/?linkid=615771).  Agent kondycji zostanie zainstalowany w ramach instalacji programu Azure AD Connect (w wersji 1.0.9125.0 lub nowszej).  Program Azure AD Connect obsługuje uaktualnienie w miejscu z poprzednich wersji.
* Aby rozpocząć korzystanie z programu Azure AD Connect Health dla usług AD DS, możesz pobrać najnowszą wersję agenta tutaj: [Pobieranie agenta programu Azure AD Connect Health dla usług AD DS](http://go.microsoft.com/fwlink/?LinkID=820540).
  [](active-directory-aadconnect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs)

## <a name="azure-ad-connect-health-portal"></a>Portal programu Azure AD Connect Health
Portal programu Azure AD Connect Health umożliwia wyświetlanie alertów, wyników monitorowania wydajności i analiz użycia. Otworzenie strony https://aka.ms/aadconnecthealth powoduje wyświetlenie głównego bloku programu Azure AD Connect Health.  Blok możesz traktować jak okno. W głównym bloku znajduje się przycisk Szybki start, usługi dostępne w programie Azure AD Connect Health i dodatkowe opcje konfiguracji. Poniżej zrzutu ekranu znajduje się krótki opis każdego z tych elementów.  Po wdrożeniu agentów usługa kondycji automatycznie identyfikuje usługi monitorowane przez program Azure AD Connect Health.

![Portal programu Azure AD Connect Health](./media/active-directory-aadconnect-health/portal4.png)

* **Szybki start** — wybranie tej pozycji powoduje otwarcie bloku Szybki start. W tym miejscu możesz pobrać agenta programu Azure AD Connect Health, wybierając pozycję Pobierz narzędzia, uzyskać dostęp do dokumentacji i wyrazić swoją opinię.
* **Active Directory Federation Services** — ta pozycja reprezentuje wszystkie usługi AD FS, które są aktualnie monitorowane przez program Azure AD Connect Health. Wybranie jednego z wystąpień powoduje otwarcie bloku z informacjami na temat tego wystąpienia usług.  Informacje te obejmują przegląd, właściwości, alerty, wyniki monitorowania i analizy użycia. Dowiedz się więcej o możliwościach [tutaj](active-directory-aadconnect-health-adfs.md).
* **Azure Active Directory Connect (synchronizacja)** — ta pozycja reprezentuje serwery programu Azure AD Connect, które są aktualnie monitorowane przez program Azure AD Connect Health. Wybranie pozycji powoduje otwarcie bloku z informacjami na temat serwerów programu Azure AD Connect. Dowiedz się więcej o możliwościach [tutaj](active-directory-aadconnect-health-sync.md).
* **Usługi domenowe Active Directory** — reprezentuje wszystkie lasy usług AD DS, które są aktualnie monitorowane przez program Azure AD Connect Health. Wybranie jednego z lasów powoduje otwarcie bloku z informacjami na jego temat.  Te informacje obejmują przegląd najważniejszych danych, pulpit nawigacyjny Kontrolery domeny, pulpit nawigacyjny Stan replikacji, alerty i monitorowanie. Dowiedz się więcej o możliwościach [tutaj](active-directory-aadconnect-health-adds.md).
* **Konfiguracja** — ta pozycja umożliwia włączanie i wyłączanie następujących opcji:

  1. Automatyczne aktualizowanie agenta programu Azure AD Connect Health do najnowszej wersji. Oznacza to, że agent programu Azure AD Connect Health zostanie automatycznie zaktualizowany do najnowszej wersji, gdy stanie się ona dostępna. Ta opcja jest domyślnie włączona.
  2. Pozwalaj firmie Microsoft na dostęp do danych o kondycji katalogu usługi Azure AD wyłącznie w celu rozwiązywania problemów. Oznacza to, że jeśli ta opcja jest włączona, firma Microsoft może zobaczyć te same dane co Ty. Może to ułatwić rozwiązywanie problemów i uzyskiwanie pomocy dotyczącej problemów. Ta opcja jest domyślnie wyłączona.

## <a name="related-links"></a>Powiązane linki
* [Instalowanie agenta programu Azure AD Connect Health](active-directory-aadconnect-health-agent-install.md)
* [Operacje w programie Azure AD Connect Health](active-directory-aadconnect-health-operations.md)
* [Używanie programu Azure AD Connect Health z usługami AD FS](active-directory-aadconnect-health-adfs.md)
* [Używanie programu Azure AD Connect Health w celu synchronizacji](active-directory-aadconnect-health-sync.md)
* [Używanie programu Azure AD Connect Health z usługami AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health — często zadawane pytania](active-directory-aadconnect-health-faq.md)
* [Historia wersji programu Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)



<!--HONumber=Jan17_HO3-->


