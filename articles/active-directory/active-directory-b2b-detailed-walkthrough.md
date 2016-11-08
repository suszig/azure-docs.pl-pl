---
title: Szczegółowy przewodnik dotyczący użycia podglądu współpracy B2B usługi Azure Active Directory | Microsoft Docs
description: Współpraca B2B usługi Azure Active Directory wspiera relacje między firmami, umożliwiając partnerom biznesowym selektywne uzyskiwanie dostępu do Twoich aplikacji firmowych
services: active-directory
documentationcenter: ''
author: viv-liu
manager: cliffdi
editor: ''
tags: ''

ms.service: active-directory
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/09/2016
ms.author: viviali

---
# Podgląd współpracy B2B usługi Azure AD: szczegółowy przewodnik
Ten przewodnik przedstawia sposób użycia funkcji współpracy B2B usługi Azure AD. Jako administrator IT firmy Contoso chcemy udostępnić aplikacje pracownikom z trzech firm partnerskich. Żadna z firm partnerskich nie musi mieć usługi Azure AD.

* Alicja z firmy Simple Partner Org
* Robert z firmy Medium Partner Org, wymaga dostępu do zestawu aplikacji
* Karolina z firmy Complex Partner Org, wymaga dostępu do zestawu aplikacji i członkostwa w grupach w firmie Contoso

Po wysłaniu zaproszeń do użytkowników z firm partnerskich można skonfigurować ich w usłudze Azure AD, aby zezwolić na dostęp do aplikacji i członkostwa w grupach poprzez witrynę Azure Portal. Zacznijmy od dodania Alicji.

## Dodawanie Alicji do katalogu Contoso
1. Utwórz plik csv z nagłówkami pokazanymi na zrzucie ekranu, uzupełniając kolumny **Email** (Adres e-mail), **DisplayName** (Nazwa wyświetlana) i **InviteContactUsUrl** (Adres URL z zaproszeniem do kontaktu). **DisplayName** (Nazwa wyświetlana) jest nazwą, która pojawi się w zaproszeniu oraz w katalogu firmy Contoso w usłudze Azure AD. **InviteContactUsUrl** (Adres URL z zaproszeniem do kontaktu) umożliwia Alicji kontakt z firmą Contoso. W poniższym przykładzie kolumna InviteContactUsUrl określa profil LinkedIn firmy Contoso. Bardzo ważne jest dokładne przepisanie etykiet w pierwszym wierszu pliku CSV zgodnie z [dokumentem referencyjnym dotyczącym formatowania pliku CSV](active-directory-b2b-references-csv-file-format.md).  
   ![Przykładowy plik CSV dla Alicji](./media/active-directory-b2b-detailed-walkthrough/AliceCSV.png)
2. W witrynie Azure Portal dodaj użytkownika do katalogu Contoso (Active Directory > Contoso > Użytkownicy > Dodaj użytkownika). W menu rozwijanym „Typ użytkownika” wybierz opcję „Użytkownicy w firmach partnerskich”. Przekaż plik CSV. Upewnij się, że plik CSV jest zamknięty przed przesłaniem.  
   ![Przekazywanie pliku CSV dla Alicji](./media/active-directory-b2b-detailed-walkthrough/AliceUpload.png)
3. Alicja jest teraz reprezentowana jako użytkownik zewnętrzny w katalogu firmy Contoso w usłudze Azure AD.  
   ![Alicja znajduje się w usłudze Azure AD](./media/active-directory-b2b-detailed-walkthrough/AliceInAD.png)
4. Alicja otrzymuje następującą wiadomość e-mail.  
   ![Wiadomość e-mail z zaproszeniem dla Alicji](./media/active-directory-b2b-detailed-walkthrough/AliceEmail.png)
5. Gdy Alicja kliknie link, zostanie poproszona o zaakceptowanie zaproszenia i zalogowanie się przy użyciu swoich poświadczeń służbowych. Jeśli Alicja nie znajduje się w katalogu usługi Azure AD, zostanie poproszona o zarejestrowanie.  
   ![Rejestracja po zaproszeniu Alicji](./media/active-directory-b2b-detailed-walkthrough/AliceSignUp.png)
6. Alicja zostanie przekierowana do panelu dostępu do aplikacji, który będzie pusty, dopóki nie zostanie wygenerowany dostęp do aplikacji.  
   ![Panel dostępu dla Alicji](./media/active-directory-b2b-detailed-walkthrough/AliceAccessPanel.png)

Ta procedura umożliwia wykorzystanie najprostszej formy współpracy B2B. Jako użytkownik w katalogu firmy Contoso w usłudze Azure AD Alicja może otrzymać dostęp do aplikacji i grup za pośrednictwem witryny Azure Portal. Teraz dodajmy Roberta, który wymaga dostępu do aplikacji Moodle i Salesforce.

## Dodawanie Roberta do katalogu firmy Contoso i udzielanie dostępu do aplikacji
1. Użyj środowiska Windows PowerShell z zainstalowanym modułem Azure AD, aby znaleźć identyfikatory aplikacji Moodle i Salesforce. Identyfikatory można pobrać przy użyciu polecenia cmdlet: `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`. Taka operacja wyświetli listę wszystkich dostępnych aplikacji w firmie Contoso i ich identyfikatorów AppPrincialId.  
   ![Pobieranie identyfikatorów dla Roberta](./media/active-directory-b2b-detailed-walkthrough/BobPowerShell.png)
2. Utwórz plik CSV zawierający adres e-mail i nazwę wyświetlaną Roberta, **InviteAppID** (Identyfikator zaproszenia do aplikacji), **InviteAppResources** (Zasoby zaproszenia do aplikacji) oraz adres InviteContactUsUrl (Adres URL z zaproszeniem do kontaktu). Wypełnij kolumnę **InviteAppResources** (Zasoby zaproszenia do aplikacji) identyfikatorami AppPrincipalId aplikacji Moodle i Salesforce znalezionymi w programie PowerShell, oddzielając je spacją. Wypełnij kolumnę **InviteAppId** (Identyfikator zaproszenia do aplikacji) tym samym identyfikatorem AppPrincipalId aplikacji Moodle, aby dodać logo Moodle do adresu e-mail i stron logowania.  
   ![Przykładowy plik CSV dla Roberta](./media/active-directory-b2b-detailed-walkthrough/BobCSV.png)
3. Przekaż plik CSV poprzez witrynę Azure Portal tak samo jak w przypadku Alicji. Robert jest teraz zewnętrznym użytkownikiem w katalogu firmy Contoso w usłudze Azure AD.
4. Robert otrzymuje następującą wiadomość e-mail.  
   ![Wiadomość e-mail z zaproszeniem dla Roberta](./media/active-directory-b2b-detailed-walkthrough/BobEmail.png)
5. Gdy Robert kliknie link, zostanie poproszony o zaakceptowanie zaproszenia. Po zalogowaniu zostanie przeniesiony do panelu dostępu i może rozpocząć korzystanie z aplikacji Moodle i Salesforce.  
   ![Panel dostępu dla Roberta](./media/active-directory-b2b-detailed-walkthrough/BobAccessPanel.png)

Teraz dodamy Karolinę, która wymaga dostępu do aplikacji oraz członkostwa w grupach w katalogu firmy Contoso.

## Dodawanie Karoliny do katalogu firmy Contoso, udzielanie dostępu do aplikacji i przydzielanie członkostwa w grupie
1. Użyj środowiska Windows PowerShell z zainstalowanym modułem Azure AD, aby znaleźć identyfikatory aplikacji i grup w katalogu firmy Contoso.
   
   * Pobierz identyfikator AppPrincipalId przy użyciu polecenia cmdlet `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId` tak samo jak w przypadku Roberta.
   * Pobierz identyfikatory ObjectId dla grup przy użyciu polecenia cmdlet `Get-MsolGroup | fl DisplayName, ObjectId`. Operacja spowoduje wyświetlenie listy wszystkich grup w firmie Contoso oraz ich identyfikatorów ObjectId. Identyfikatory grup można również pobrać jako identyfikatory obiektów na karcie Właściwości grupy w witrynie Azure Portal.  
     ![Pobieranie identyfikatorów i grup dla Karoliny](./media/active-directory-b2b-detailed-walkthrough/CarolPowerShell.png)
2. Utwórz plik CSV i wypełnij kolumny Email (Adres e-mail), DisplayName (Nazwa wyświetlana), InviteAppID (Identyfikator zaproszenia do aplikacji), InviteAppResources (Zasoby zaproszenia do aplikacji), **InviteGroupResources** (Zasoby zaproszenia do grupy) oraz InviteContactUsUrl (Adres URL z zaproszeniem do kontaktu). Kolumnę **InviteGroupResources** (Zasoby zaproszenia do grupy) wypełnia się identyfikatorami ObjectId grup MyGroup1 i Externals, oddzielonymi spacją.  
   ![Przykładowy plik CSV dla Karoliny](./media/active-directory-b2b-detailed-walkthrough/CarolCSV.png)
3. Przekaż plik CSV za pośrednictwem witryny Azure Portal.
4. Karolina jest użytkownikiem katalogu firmy Contoso oraz członkiem grup MyGroup1 i Externals, jak widać w witrynie Azure Portal.  
   ![Karolina znajduje się na listach w grupie w usłudze Azure AD](./media/active-directory-b2b-detailed-walkthrough/CarolGroup.png)
5. Karolina otrzymuje wiadomość e-mail zawierającą link umożliwiający zaakceptowanie zaproszenia. Po zalogowaniu zostanie przeniesiona do panelu dostępu do aplikacji i uzyska dostęp do aplikacji Moodle i Salesforce.  

To już wszystko, jeśli chodzi o dodawanie użytkowników z firm partnerskich do współpracy B2B w usłudze Azure AD. W tym poradniku pokazano, jak dodać użytkowników (Alicję, Roberta i Karolinę) do katalogu Contoso przy użyciu trzech oddzielnych plików CSV. Proces ten można ułatwić poprzez skondensowanie trzech oddzielnych plików CSV do jednego pliku.  
![Przykładowy plik CSV dla Alicji, Roberta i Karoliny](./media/active-directory-b2b-detailed-walkthrough/CombinedCSV.png)

## Pokrewne artykuły:
Zobacz nasze inne artykuły dotyczące współpracy B2B w usłudze Azure AD:

* [Czym jest współpraca B2B w usłudze Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Jak to działa](active-directory-b2b-how-it-works.md)
* [Dokument referencyjny dotyczący formatowania pliku CSV](active-directory-b2b-references-csv-file-format.md)
* [Format tokenu użytkownika zewnętrznego](active-directory-b2b-references-external-user-token-format.md)
* [Zmiany atrybutów obiektu użytkownika zewnętrznego](active-directory-b2b-references-external-user-object-attribute-changes.md)
* [Bieżące ograniczenia wersji zapoznawczej](active-directory-b2b-current-preview-limitations.md)
* [Indeks artykułów dotyczących zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)

<!--HONumber=sep16_HO1-->


