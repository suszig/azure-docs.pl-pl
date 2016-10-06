<properties
   pageTitle="Wprowadzenie do raportów usługi Azure Active Directory | Microsoft Azure"
   description="Lista raportów dostępnych w usłudze Azure Active Directory"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="03/07/2016"
   ms.author="dhanyahk"/>


# Wprowadzenie do raportów usługi Azure Active Directory

## Co to jest

W usłudze Azure Active Directory (Azure AD) uwzględniono raporty dotyczące zabezpieczeń, działań i inspekcji dla katalogu. Oto lista uwzględnionych raportów:

### Raporty dotyczące zabezpieczeń

- Logowania z nieznanych źródeł
- Logowania po wielokrotnych niepowodzeniach
- Logowania z wielu lokalizacji geograficznych
- Logowania z adresów IP związanych z podejrzanymi działaniami
- Nieregularne działania związane z logowaniem
- Logowania z urządzeń, które mogą być zainfekowane
- Nietypowe działania użytkowników związane z logowaniem

### Raporty dotyczące działań

- Podsumowanie użycia aplikacji
- Szczegóły użycia aplikacji
- Pulpit nawigacyjny aplikacji
- Błędy aprowizacji kont
- Urządzenia indywidualnych użytkowników
- Działania indywidualnych użytkowników
- Raport dotyczący działań grup
- Raport dotyczący działań związanych z rejestracją resetowania haseł
- Działania związane z resetowaniem haseł

### Raporty dotyczące inspekcji

- Raport dotyczący inspekcji katalogu

> [AZURE.TIP] Więcej informacji dotyczących dokumentacji raportów usługi Azure AD zawiera temat [Wyświetlanie raportów dotyczących dostępu i użycia](active-directory-view-access-usage-reports.md).



## Jak to działa


### Potok raportowania

Potok raportowania składa się z trzech głównych kroków. Za każdym razem, gdy użytkownik loguje się lub jest uwierzytelniany, wykonywane są następujące operacje:

- Po pierwsze użytkownik jest uwierzytelniany (pomyślnie lub nie), a wynik jest zapisywany w bazach danych usługi Azure Active Directory.
- W regularnych odstępach czasu przetwarzane są wszystkie ostatnie logowania. W tym momencie nasze algorytmy analizy zabezpieczeń i nietypowych działań przeszukują wszystkie ostatnie logowania pod kątem podejrzanych działań.
- Po przetworzeniu raporty są zapisywane, umieszczane w pamięci podręcznej i obsługiwane w klasycznym portalu Azure.

### Czas trwania generowania raportów

Ze względu na dużą liczbę sesji uwierzytelniania i logowania, przetwarzanych na platformie Azure AD, najnowsze sesje logowania są przetwarzane przeciętnie z opóźnieniem jednej godziny. W sporadycznych przypadkach przetwarzanie najnowszych sesji logowania może trwać 8 godzin.

Najnowsze przetworzone sesje logowania można znaleźć, sprawdzając tekst pomocy na początku każdego raportu.

![Tekst pomocy na początku każdego raportu](./media/active-directory-reporting-getting-started/reportingWatermark.PNG)

> [AZURE.TIP] Więcej informacji dotyczących dokumentacji raportów usługi Azure AD zawiera temat [Wyświetlanie raportów dotyczących dostępu i użycia](active-directory-view-access-usage-reports.md).



## Wprowadzenie


### Logowanie się do klasycznego portalu Azure

Najpierw musisz zalogować się do [klasycznego portalu Azure](https://manage.windowsazure.com) jako administrator globalny lub administrator zgodności. Musisz również być administratorem usługi subskrypcji platformy Azure lub współadministratorem albo używać subskrypcji platformy Azure „Dostęp do usługi Azure AD”.

### Przechodzenie do raportów

Aby wyświetlić raporty, musisz przejść do karty Raporty w górnej części katalogu.

Jeśli po raz pierwszy wyświetlasz raporty, musisz zaakceptować okno dialogowe, aby móc wyświetlać raporty. Umożliwia to upewnienie się, że można zaakceptować wyświetlanie przez administratorów w organizacji tych danych, które mogą być uznawane za informacje osobiste w niektórych krajach.

![Okno dialogowe](./media/active-directory-reporting-getting-started/dialogBox.png)

### Eksplorowanie każdego raportu

Musisz przejść do każdego raportu, aby wyświetlić zbierane dane i przetwarzane sesje logowania. Możesz znaleźć [listę wszystkich raportów tutaj](active-directory-reporting-guide.md).

![Wszystkie raporty](./media/active-directory-reporting-getting-started/reportsMain.png)

### Pobieranie raportów w formacie CSV

Każdy raport można pobrać jako plik CSV (wartości rozdzielane przecinkami). Możesz używać tych plików w programie Excel, usłudze PowerBI lub programach analitycznych innych firm w celu przeprowadzenia dalszej analizy danych.

Aby pobrać dowolny raport w formacie CSV, należy przejść do raportu i kliknąć przycisk „Pobierz” u dołu.

![Przycisk Pobierz](./media/active-directory-reporting-getting-started/downloadButton.png)

> [AZURE.TIP] Więcej informacji dotyczących dokumentacji raportów usługi Azure AD zawiera temat [Wyświetlanie raportów dotyczących dostępu i użycia](active-directory-view-access-usage-reports.md).





## Następne kroki

### Dostosowywanie alertów dotyczących nietypowych działań związanych z logowaniem

Przejdź do karty „Konfiguruj” katalogu.

Przewiń do sekcji „Powiadomienia”.

Włącz lub wyłącz sekcję „Wiadomości e-mail z powiadomieniami dotyczącymi nietypowych działań związanych z logowaniem”.

![Sekcja Powiadomienia](./media/active-directory-reporting-getting-started/notificationsSection.png)

### Integracja z interfejsem API raportowania usługi Azure AD

Zobacz [Wprowadzenie do interfejsu API raportowania](active-directory-reporting-api-getting-started.md).

### Włączanie usługi Multi-Factor Authentication dla użytkowników

Wybierz użytkownika w raporcie.

Kliknij przycisk „Włącz usługę MFA” u dołu ekranu.

![Przycisk usługi Multi-Factor Authentication u dołu ekranu](./media/active-directory-reporting-getting-started/mfaButton.png)

> [AZURE.TIP] Więcej informacji dotyczących dokumentacji raportów usługi Azure AD zawiera temat [Wyświetlanie raportów dotyczących dostępu i użycia](active-directory-view-access-usage-reports.md).




## Dowiedz się więcej


### Inspekcja zdarzeń

Dowiedz się, jakie zdarzenia są poddawane inspekcji w katalogu, przechodząc do tematu [Zdarzenia inspekcji raportowania usługi Azure Active Directory](active-directory-reporting-audit-events.md).

### Integracja z interfejsem API

Zobacz [Wprowadzenie do interfejsu API raportowania](active-directory-reporting-api-getting-started.md) i [Dokumentacja interfejsu API raportowania](https://msdn.microsoft.com/library/azure/mt126081.aspx).

### Kontakt

Opinie, zgłoszenia związane z pomocą techniczną lub pytania należy przesyłać na adres [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com).

> [AZURE.TIP] Więcej informacji dotyczących dokumentacji raportów usługi Azure AD zawiera temat [Wyświetlanie raportów dotyczących dostępu i użycia](active-directory-view-access-usage-reports.md).



<!--HONumber=Sep16_HO4-->


