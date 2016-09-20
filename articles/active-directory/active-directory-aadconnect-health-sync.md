
<properties
    pageTitle="Używanie programu Azure AD Connect Health z usługą synchronizacji | Microsoft Azure"
    description="Jest to strona programu Azure AD Connect Health, która będzie omawiać temat monitorowania synchronizacji usługi Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# Używanie programu Azure AD Connect Health w celu synchronizacji
Poniższa dokumentacja dotyczy monitorowania programu Azure AD Connect (synchronizacja) przy użyciu programu Azure AD Connect Health.  Aby uzyskać informacje na temat monitorowania usług AD FS za pomocą programu Azure AD Connect Health, zobacz [Używanie programu Azure AD Connect Health z usługami AD FS](active-directory-aadconnect-health-adfs.md). Ponadto, aby uzyskać informacje na temat monitorowania Usług domenowych Active Directory za pomocą programu Azure AD Connect Health, zobacz [Używanie programu Azure AD Connect Health z usługami AD DS](active-directory-aadconnect-health-adds.md).

![Program Azure AD Connect Health do celów synchronizacji](./media/active-directory-aadconnect-health-sync/sync.png)

## Alerty dla programu Azure AD Connect Health do celów synchronizacji
Sekcja Alerty programu Azure AD Connect Health do celów synchronizacji zawiera listę aktywnych alertów. Każdy alert zawiera istotne informacje, kroki do rozwiązania problemu i linki do powiązanej dokumentacji. Po wybraniu aktywnego lub rozwiązanego alertu pojawi się nowy blok z dodatkowymi informacjami, kroki, jakie można podjąć, aby rozwiązać alert, oraz linki do dodatkowej dokumentacji. Można również wyświetlić dane historyczne na temat alertów, które zostały rozwiązane w przeszłości.

Po wybraniu alertu pojawią się dodatkowe informacje, kroki, jakie można podjąć, aby rozwiązać alert, oraz linki do dodatkowej dokumentacji.

![Błąd synchronizacji programu Azure AD Connect](./media/active-directory-aadconnect-health-sync/alert.png)

### Ograniczona ocena alertów
Jeśli program Azure AD Connect NIE KORZYSTA z konfiguracji domyślnej (na przykład jeśli filtrowanie atrybutów zostało zmienione z konfiguracji domyślnej na konfigurację niestandardową), wtedy agent programu Azure AD Connect Health nie przekaże zdarzeń błędów powiązanych z programem Azure AD Connect. 

To ogranicza ocenę alertów przez usługę. Zobaczysz baner, który wskazuje na taki stan w witrynie Azure Portal w ramach usługi.

![Program Azure AD Connect Health do celów synchronizacji](./media/active-directory-aadconnect-health-sync/banner.png)

Możesz to zmienić, klikając pozycję „Ustawienia” i pozwalając agentowi programu Azure AD Connect Health, na przekazanie wszystkich dzienników błędów.

![Program Azure AD Connect Health do celów synchronizacji](./media/active-directory-aadconnect-health-sync/banner2.png)

## Wgląd w szczegóły synchronizacji
Najnowsza wersja programu Azure AD Connect Health do celów synchronizacji ma następujące nowe funkcje:

- Opóźnienie operacji synchronizacji
- Trend zmiany obiektu

### Opóźnienie synchronizacji
Ta funkcja prezentuje w sposób graficzny trend opóźnienia operacji synchronizacji (import, eksport itd.) dla łączników.  Zapewnia to szybki i łatwy sposób zrozumienia nie tylko opóźnień operacji (dobrze, jeśli dysponujesz dużym zestawem zachodzących zmian), ale także wykrycia anomalii w opóźnieniu, które mogą wymagać bliższego zbadania.

![Opóźnienie synchronizacji](./media/active-directory-aadconnect-health-sync/synclatency.png)

Tylko opóźnienie operacji „Eksportuj” dla łącznika usługi Azure AD jest wyświetlane domyślnie.  Aby zobaczyć więcej operacji na łączniku lub aby wyświetlić operacje innych łączników, kliknij wykres prawym przyciskiem myszy, a następnie wybierz określoną operację i łącznik.

### Zmiany obiektu synchronizacji
Ta funkcja prezentuje w sposób graficzny trend liczby zmian obliczanych i eksportowanych do usługi Azure AD.  Zebranie tych informacji z dzienników synchronizacji jest obecnie trudne.  Wykres umożliwia nie tylko łatwiejszy sposób monitorowania liczby zmian, jakie zachodzą w Twoim środowisku, ale także przedstawia w sposób czytelny i obrazowy pojawiające się błędy.

![Opóźnienie synchronizacji](./media/active-directory-aadconnect-health-sync/syncobjectchanges.png)

## Powiązane linki

* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Instalowanie agenta programu Azure AD Connect Health](active-directory-aadconnect-health-agent-install.md)
* [Operacje w programie Azure AD Connect Health](active-directory-aadconnect-health-operations.md)
* [Używanie programu Azure AD Connect Health z usługami AD FS](active-directory-aadconnect-health-adfs.md)
* [Używanie programu Azure AD Connect Health z usługami AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health — często zadawane pytania](active-directory-aadconnect-health-faq.md)
* [Historia wersji programu Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)




<!--HONumber=sep16_HO1-->


