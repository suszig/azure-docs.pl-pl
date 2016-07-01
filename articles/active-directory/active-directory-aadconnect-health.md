<properties
    pageTitle="Monitorowanie lokalnej infrastruktury do obsługi tożsamości w chmurze."
    description="Jest to strona dotycząca programu Azure AD Connect Health, na której opisano, czym on jest i do czego służy."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="stevenpo"
    editor="karavar"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="03/21/2016"
    ms.author="vakarand"/>

# Monitorowanie lokalnej infrastruktury do obsługi tożsamości i usług synchronizacji w chmurze

Program Azure AD Connect Health pomaga monitorować i zdobywać informacje o lokalnej infrastrukturze do obsługi tożsamości oraz usługach synchronizacji.  Umożliwia utrzymywanie niezawodnego połączenia z usługami Office 365 i Microsoft Online Services, udostępniając funkcje monitorowania kluczowych składników tożsamości, takich jak serwery usług AD FS, serwery programu Azure AD Connect (nazywane aparatem synchronizacji), kontrolery domeny usługi Active Directory itp. Zapewnia także łatwy dostęp do kluczowych punktów danych dotyczących tych składników, ułatwiając zdobywanie informacji o użyciu i innych istotnych parametrach.

Informacje są prezentowane w [portalu programu Azure AD Connect Health](https://aka.ms/aadconnecthealth). Korzystając z portalu programu Azure AD Connect Health, możesz wyświetlać alerty, wyniki monitorowania wydajności, analizy użycia itd. Program Azure AD Connect Health pokazuje kondycję kluczowych składników tożsamości w jednym miejscu.

![Co to jest program Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnecthealth2.png)

Przyszłe aktualizacje programu Azure AD Connect Health będą zawierały dodatkowe funkcje monitorowania i wgląd w dodatkowe składniki tożsamości. Dzięki temu uzyskasz jeden pulpit dotyczący tożsamości zapewniający jeszcze bardziej niezawodne, będące w dobrej kondycji i zintegrowane środowisko, którego użytkownicy mogą używać do zwiększania wydajności pracy.

<!-- <center>![What is Azure AD Connect Health](./media/active-directory-aadconnect-health/logo1.png)</center> -->

## Dlaczego warto korzystać z programu Azure AD Connect Health

Zintegrowanie katalogów lokalnych z usługą Azure AD zwiększa produktywność użytkowników, zapewniając wspólną tożsamość na potrzeby dostępu do zasobów, zarówno lokalnych, jak i w chmurze. Jednak tego rodzaju integracja wiąże się z koniecznością zapewnienia dobrej kondycji środowiska, aby użytkownicy mieli niezawodny dostęp z dowolnego urządzenia do zasobów lokalnych i znajdujących się w chmurze. Program Azure AD Connect Health oferuje łatwe, oparte na chmurze podejście do monitorowania i zdobywania informacji o lokalnej infrastrukturze do obsługi tożsamości, która umożliwia dostęp do usługi Office 365 lub innych aplikacji usługi Azure AD. Wymaga to jedynie zainstalowania agenta na każdym z lokalnych serwerów tożsamości.

## [Program Azure AD Connect Health dla usług AD FS](active-directory-aadconnect-health-adfs.md)

Program Azure AD Connect Health dla usług AD FS obsługuje usługi AD FS 2.0 w systemie Windows Server 2008 R2 oraz usługi AD FS w systemach Windows Server 2012 i Windows Server 2012 R2. Dotyczy to także serwerów proxy usług AD FS lub serwerów proxy aplikacji sieci Web, które zapewniają obsługę uwierzytelniania w przypadku dostępu do ekstranetu. Dzięki bardzo prostej i niedrogiej instalacji agenta kondycji program Azure AD Connect Health dla usług AD FS oferuje następujący zestaw kluczowych funkcji:

- Monitorowanie z alertami, które informują, kiedy serwery usług AD FS i serwery proxy usług AD FS nie są w dobrej kondycji
- Powiadomienia e-mail dotyczące alertów krytycznych
- Wyświetlanie trendów w danych dotyczących wydajności, co jest przydatne do planowania wydajności usług AD FS
- Analizy użycia dotyczące różnych wariantów logowania się do usług AD FS (aplikacje, użytkownicy, lokalizacja sieciowa itp.), co pomaga zrozumieć sposób korzystania z usług AD FS
- Raporty dotyczące usług AD FS, na przykład lista 50 użytkowników, którzy najczęściej nieprawidłowo podawali nazwę użytkownika/hasło

Poniższy klip wideo zawiera omówienie programu Azure AD Connect Health dla usług AD FS

>[AZURE.VIDEO azure-ad-connect-health--monitor-you-identity-bridge]

## [Program Azure AD Connect Health do celów synchronizacji](active-directory-aadconnect-health-sync.md)
Program Azure AD Connect Health do celów synchronizacji monitoruje i udostępnia informacje na temat procesów synchronizacji, które mają miejsce między lokalną usługą Active Directory i usługą Azure Active Directory. Program Azure AD Connect Health do celów synchronizacji oferuje następujący zestaw kluczowych funkcji:

- Monitorowanie z alertami, które informują, kiedy serwery programu Azure AD Connect zwane aparatem synchronizacji nie są w dobrej kondycji
- Powiadomienia e-mail dotyczące alertów krytycznych
- Wgląd w dane operacyjne dotyczące synchronizacji, między innymi wykresy opóźnień operacji synchronizacji i trendy w operacjach synchronizacji, takie jak dodawanie, aktualizowanie i usuwanie.
- Przegląd informacji na temat właściwości synchronizacji i ostatniego pomyślnego eksportu do usługi Azure AD

Poniższy klip wideo zawiera omówienie programu Azure AD Connect Health do celów synchronizacji

[Azure Active Directory Connect Health: monitorowanie aparatu synchronizacji](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-Health-Monitoring-the-sync-engine)


## Wprowadzenie do programu Azure AD Connect Health
Rozpoczęcie pracy z programem Azure AD Connect Health jest bardzo proste. Wykonaj poniższe kroki:

1. [Uzyskaj usługę Azure AD w wersji Premium](active-directory-get-started-premium) lub [rozpocznij okres próbny](https://azure.microsoft.com/trial/get-started-active-directory/).

2. [Pobierz i zainstaluj agentów programu Azure AD Connect Health](#download-and-install-azure-ad-connect-health-agent) na serwerach tożsamości.

3. Wyświetl pulpit nawigacyjny programu Azure AD Connect Health na stronie [https://aka.ms/aadconnecthealth](https://aka.ms/aadconnecthealth).

>[AZURE.NOTE]Pamiętaj, że przed wyświetleniem jakichkolwiek danych na pulpicie nawigacyjnym programu Azure AD Connect Health musisz zainstalować agentów programu Azure AD Connect Health na serwerach docelowych.

## Pobieranie i instalowanie agenta programu Azure AD Connect Health

- Zobacz [wymagania](active-directory-aadconnect-health-agent-install.md#Requirements) programu Azure AD Connect Health.

- Aby rozpocząć korzystanie z programu Azure AD Connect Health dla usług AD FS, możesz pobrać najnowszą wersję agenta tutaj: [Pobieranie agenta programu Azure AD Connect Health dla usług AD FS](http://go.microsoft.com/fwlink/?LinkID=518973).
[](active-directory-aadconnect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs)

- Aby rozpocząć korzystanie z programu Azure AD Connect Health do celów synchronizacji, pobierz i zainstaluj [najnowszą wersję programu Azure AD Connect](http://go.microsoft.com/fwlink/?linkid=615771).  Agent kondycji zostanie zainstalowany w ramach instalacji programu Azure AD Connect (w wersji 1.0.9125.0 lub nowszej).  Program Azure AD Connect obsługuje uaktualnienie w miejscu z poprzednich wersji.


## Portal programu Azure AD Connect Health
Portal programu Azure AD Connect Health umożliwia wyświetlanie alertów, wyników monitorowania wydajności i analiz użycia. Otworzenie strony https://aka.ms/aadconnecthealth powoduje wyświetlenie głównego bloku programu Azure AD Connect Health.  Blok możesz traktować jak okno. W głównym bloku znajduje się przycisk Szybki start, usługi dostępne w programie Azure AD Connect Health i dodatkowe opcje konfiguracji. Poniżej zrzutu ekranu znajduje się krótki opis każdego z tych elementów.  Po wdrożeniu agentów usługa identyfikuje usługi monitorowane przez program Azure AD Connect Health.

![Portal programu Azure AD Connect Health](./media/active-directory-aadconnect-health/portal2.png)

- **Szybki start** — wybranie tej pozycji powoduje otwarcie bloku Szybki start. W tym miejscu możesz pobrać agenta programu Azure AD Connect Health, wybierając pozycję Pobierz narzędzia, uzyskać dostęp do dokumentacji i wyrazić swoją opinię.

- **Active Directory Federation Services** — ta pozycja reprezentuje wszystkie usługi AD FS, które są aktualnie monitorowane przez program Azure AD Connect Health. Wybranie jednego z wystąpień powoduje otwarcie bloku z informacjami na temat tego wystąpienia usług.  Informacje te obejmują przegląd, właściwości, alerty, wyniki monitorowania i analizy użycia. Dowiedz się więcej o możliwościach [tutaj](active-directory-aadconnect-health-adfs.md).

- **Azure Active Directory Connect (synchronizacja)** — ta pozycja reprezentuje serwery programu Azure AD Connect, które są aktualnie monitorowane przez program Azure AD Connect Health. Wybranie pozycji powoduje otwarcie bloku z informacjami na temat serwerów programu Azure AD Connect. Dowiedz się więcej o możliwościach [tutaj](active-directory-aadconnect-health-sync.md).

- **Konfiguracja** — ta pozycja umożliwia włączanie i wyłączanie następujących opcji:

    1. Automatyczne aktualizowanie agenta programu Azure AD Connect Health do najnowszej wersji. Oznacza to, że agent programu Azure AD Connect Health zostanie automatycznie zaktualizowany do najnowszej wersji, gdy stanie się ona dostępna. Ta opcja jest domyślnie włączona.

    2. Pozwalaj firmie Microsoft na dostęp do danych o kondycji katalogu usługi Azure AD wyłącznie w celu rozwiązywania problemów. Oznacza to, że jeśli ta opcja jest włączona, firma Microsoft może zobaczyć te same dane co Ty. Może to ułatwić rozwiązywanie problemów i uzyskiwanie pomocy dotyczącej problemów. Ta opcja jest domyślnie wyłączona.


## Powiązane linki

* [Instalowanie agenta programu Azure AD Connect Health](active-directory-aadconnect-health-agent-install.md)
* [Operacje w programie Azure AD Connect Health](active-directory-aadconnect-health-operations.md)
* [Używanie programu Azure AD Connect Health z usługami AD FS](active-directory-aadconnect-health-adfs.md)
* [Używanie programu Azure AD Connect Health w celu synchronizacji](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect Health — często zadawane pytania](active-directory-aadconnect-health-faq.md)
* [Historia wersji programu Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)



<!--HONumber=Jun16_HO2-->


