---
title: "Wprowadzenie do raportów usługi Azure Active Directory | Microsoft Docs"
description: "Lista raportów dostępnych w usłudze Azure Active Directory"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: 7ac99919-8df5-4424-9298-fc7c025ba949
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2017
ms.author: dhanyahk;markvi
ms.translationtype: Human Translation
ms.sourcegitcommit: eec9b73cbaccfa50eec6f237e4d1d810c6efa1d9
ms.openlocfilehash: e5b8ac91914203156bd395d7f462385e9f6dbcb4
ms.contentlocale: pl-pl
ms.lasthandoff: 02/24/2017


---
# <a name="getting-started-with-azure-active-directory-reporting"></a>Wprowadzenie do raportów usługi Azure Active Directory
## <a name="what-it-is"></a>Co to jest
W usłudze Azure Active Directory (Azure AD) uwzględniono raporty dotyczące zabezpieczeń, działań i inspekcji dla katalogu. Oto lista uwzględnionych raportów:

### <a name="security-reports"></a>Raporty dotyczące zabezpieczeń
* Logowania z nieznanych źródeł
* Logowania po wielokrotnych niepowodzeniach
* Logowania z wielu lokalizacji geograficznych
* Logowania z adresów IP związanych z podejrzanymi działaniami
* Nieregularne działania związane z logowaniem
* Logowania z urządzeń, które mogą być zainfekowane
* Nietypowe działania użytkowników związane z logowaniem

### <a name="activity-reports"></a>Raporty dotyczące działań
* Podsumowanie użycia aplikacji
* Szczegóły użycia aplikacji
* Pulpit nawigacyjny aplikacji
* Błędy aprowizacji kont
* Urządzenia indywidualnych użytkowników
* Działania indywidualnych użytkowników
* Raport dotyczący działań grup
* Raport dotyczący działań związanych z rejestracją resetowania haseł
* Działania związane z resetowaniem haseł

### <a name="audit-reports"></a>Raporty dotyczące inspekcji
* Raport dotyczący inspekcji katalogu

> [!TIP]
> Więcej informacji dotyczących dokumentacji raportów usługi Azure AD zawiera temat [Wyświetlanie raportów dotyczących dostępu i użycia](active-directory-view-access-usage-reports.md).
> 
> 

## <a name="how-it-works"></a>Jak to działa
### <a name="reporting-pipeline"></a>Potok raportowania
Potok raportowania składa się z trzech głównych kroków. Za każdym razem, gdy użytkownik loguje się lub jest uwierzytelniany, wykonywane są następujące operacje:

* Po pierwsze użytkownik jest uwierzytelniany (pomyślnie lub nie), a wynik jest zapisywany w bazach danych usługi Azure Active Directory.
* W regularnych odstępach czasu przetwarzane są wszystkie ostatnie logowania. W tym momencie nasze algorytmy analizy zabezpieczeń i nietypowych działań przeszukują wszystkie ostatnie logowania pod kątem podejrzanych działań.
* Po przetworzeniu raporty są zapisywane, umieszczane w pamięci podręcznej i obsługiwane w klasycznym portalu Azure.

### <a name="report-generation-times"></a>Czas trwania generowania raportów
Ze względu na dużą liczbę sesji uwierzytelniania i logowania, przetwarzanych na platformie Azure AD, najnowsze sesje logowania są przetwarzane przeciętnie z opóźnieniem jednej godziny. W sporadycznych przypadkach przetwarzanie najnowszych sesji logowania może trwać 8 godzin.

Najnowsze przetworzone sesje logowania można znaleźć, sprawdzając tekst pomocy na początku każdego raportu.

![Tekst pomocy na początku każdego raportu](./media/active-directory-reporting-getting-started/reportingWatermark.PNG)

> [!TIP]
> Więcej informacji dotyczących dokumentacji raportów usługi Azure AD zawiera temat [Wyświetlanie raportów dotyczących dostępu i użycia](active-directory-view-access-usage-reports.md).
> 
> 

## <a name="getting-started"></a>Wprowadzenie
### <a name="sign-into-the-azure-classic-portal"></a>Logowanie się do klasycznego portalu Azure
Najpierw musisz zalogować się do [klasycznego portalu Azure](https://manage.windowsazure.com) jako administrator globalny lub administrator zgodności. Musisz również być administratorem usługi subskrypcji platformy Azure lub współadministratorem albo używać subskrypcji platformy Azure „Dostęp do usługi Azure AD”.

### <a name="navigate-to-reports"></a>Przechodzenie do raportów
Aby wyświetlić raporty, musisz przejść do karty Raporty w górnej części katalogu.

Jeśli po raz pierwszy wyświetlasz raporty, musisz zaakceptować okno dialogowe, aby móc wyświetlać raporty. Umożliwia to upewnienie się, że można zaakceptować wyświetlanie przez administratorów w organizacji tych danych, które mogą być uznawane za informacje osobiste w niektórych krajach.

![Okno dialogowe](./media/active-directory-reporting-getting-started/dialogBox.png)

### <a name="explore-each-report"></a>Eksplorowanie każdego raportu
Musisz przejść do każdego raportu, aby wyświetlić zbierane dane i przetwarzane sesje logowania. Możesz znaleźć [listę wszystkich raportów tutaj](active-directory-reporting-guide.md).

![Wszystkie raporty](./media/active-directory-reporting-getting-started/reportsMain.png)

### <a name="download-the-reports-as-csv"></a>Pobieranie raportów w formacie CSV
Każdy raport można pobrać jako plik CSV (wartości rozdzielane przecinkami). Możesz używać tych plików w programie Excel, usłudze PowerBI lub programach analitycznych innych firm w celu przeprowadzenia dalszej analizy danych.

Aby pobrać dowolny raport w formacie CSV, należy przejść do raportu i kliknąć przycisk „Pobierz” u dołu.

![Przycisk Pobierz](./media/active-directory-reporting-getting-started/downloadButton.png)

> [!TIP]
> Więcej informacji dotyczących dokumentacji raportów usługi Azure AD zawiera temat [Wyświetlanie raportów dotyczących dostępu i użycia](active-directory-view-access-usage-reports.md).
> 
> 

## <a name="next-steps"></a>Następne kroki
### <a name="customize-alerts-for-anomalous-sign-in-activity"></a>Dostosowywanie alertów dotyczących nietypowych działań związanych z logowaniem
Przejdź do karty „Konfiguruj” katalogu.

Przewiń do sekcji „Powiadomienia”.

Włącz lub wyłącz sekcję „Wiadomości e-mail z powiadomieniami dotyczącymi nietypowych działań związanych z logowaniem”.

![Sekcja Powiadomienia](./media/active-directory-reporting-getting-started/notificationsSection.png)

### <a name="integrate-with-the-azure-ad-reporting-api"></a>Integracja z interfejsem API raportowania usługi Azure AD
Zobacz [Wprowadzenie do interfejsu API raportowania](active-directory-reporting-api-getting-started.md).

### <a name="engage-multi-factor-authentication-on-users"></a>Włączanie usługi Multi-Factor Authentication dla użytkowników
Wybierz użytkownika w raporcie.

Kliknij przycisk „Włącz usługę MFA” u dołu ekranu.

![Przycisk usługi Multi-Factor Authentication u dołu ekranu](./media/active-directory-reporting-getting-started/mfaButton.png)

> [!TIP]
> Więcej informacji dotyczących dokumentacji raportów usługi Azure AD zawiera temat [Wyświetlanie raportów dotyczących dostępu i użycia](active-directory-view-access-usage-reports.md).
> 
> 

## <a name="learn-more"></a>Dowiedz się więcej
### <a name="audit-events"></a>Inspekcja zdarzeń
Dowiedz się, jakie zdarzenia są poddawane inspekcji w katalogu, przechodząc do tematu [Zdarzenia inspekcji raportowania usługi Azure Active Directory](active-directory-reporting-audit-events.md).

### <a name="api-integration"></a>Integracja z interfejsem API
Zobacz [Wprowadzenie do interfejsu API raportowania](active-directory-reporting-api-getting-started.md) i [Dokumentacja interfejsu API raportowania](https://msdn.microsoft.com/library/azure/mt126081.aspx).

### <a name="get-in-touch"></a>Kontakt
Jeśli chcesz przesłać opinię, potrzebujesz pomocy lub masz pytania, wyślij wiadomość e-mail na adres [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com).

> [!TIP]
> Więcej informacji dotyczących dokumentacji raportów usługi Azure AD zawiera temat [Wyświetlanie raportów dotyczących dostępu i użycia](active-directory-view-access-usage-reports.md).
> 
> 


