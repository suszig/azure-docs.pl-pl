---
title: "Wprowadzenie do usługi Azure Active Directory — wersja Premium"
description: "Temat opisujący tworzenie konta w usłudze Azure Active Directory — wersja Premium za pośrednictwem witryny sieci Web licencjonowania zbiorowego."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 391764e5-c4eb-4ae6-b8a9-3d09f0de04a6
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/31/2016
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 73cbbce9ae3d727632b8cac2a29c2b1b92332ae7


---
# <a name="getting-started-with-azure-active-directory-premium"></a>Wprowadzenie do usługi Azure Active Directory — wersja Premium
Konto w usłudze Active Directory — wersja Premium można utworzyć na kilka sposobów: 

**Platforma Azure lub usługa Office 365** — jako subskrybent platformy Azure lub usługi Office 365 możesz kupić usługę Active Directory — wersja Premium online. Aby uzyskać szczegółowy opis kroków, zobacz [How to Purchase Azure Active Directory Premium - Existing Customers](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/How-to-Purchase-Azure-Active-Directory-Premium-Existing-Customer) (Jak kupić usługę Azure Active Directory — wersja Premium: istniejący klienci) lub [How to Purchase Azure Active Directory Premium - New Customers](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/How-to-Purchase-Azure-Active-Directory-Premium-New-Customers) (Jak kupić usługę Azure Active Directory — wersja Premium: nowi klienci).  

**Enterprise Mobility + Security** — Enterprise Mobility + Security (dawniej Enterprise Mobility Suite) to ekonomiczne rozwiązanie dla organizacji korzystających z następujących usług w ramach jednego planu licencjonowania: Active Directory — wersja Premium, Azure Rights Management i Microsoft Intune. Aby uzyskać więcej informacji, zobacz witrynę sieci Web pakietu [Enterprise Mobility + Security](https://www.microsoft.com/en-us/server-cloud/enterprise-mobility/overview.aspx). Aby uzyskać bezpłatną 30-dniową wersję próbną, kliknij [tutaj](https://portal.office.com/Signup/Signup.aspx?OfferId=2E63A04D-BE0B-4A0F-A8CF-407C1C299221&dl=EMS&ali=1#0).

**Licencjonowanie zbiorowe firmy Microsoft** — usługa Azure Active Directory — wersja Premium jest dostępna za pośrednictwem [umowy Microsoft Enterprise Agreement](https://www.microsoft.com/en-us/licensing/licensing-programs/enterprise.aspx) (250 lub więcej licencji) lub [programu licencjonowania Open](https://www.microsoft.com/en-us/licensing/licensing-programs/open-license.aspx) (5-250 licencji).

W tym temacie opisano rozpoczynanie pracy z usługą Azure Active Directory — wersja Premium zakupioną w ramach programu licencjonowania zbiorowego. Jeśli nie znasz jeszcze różnych wersji usługi Azure Active Directory, zobacz [Wersje usługi Azure Active Directory](active-directory-editions.md).  

> [!NOTE]
> Klienci w Chinach mogą używać wersji Premium i Podstawowa usługi Azure Active Directory za pomocą wystąpienia usługi Azure Active Directory dostępnego na całym świecie. Wersje Premium i Podstawowa usługi Azure Active Directory nie są obecnie obsługiwane w usłudze Microsoft Azure świadczonej przez 21Vianet w Chinach. Aby uzyskać więcej informacji, skontaktuj się z nami na [forum usługi Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/).
> 
> 

## <a name="step-1-sign-up-for-active-directory-premium"></a>Krok 1: Tworzenie konta w usłudze Active Directory — wersja Premium
Aby utworzyć konto, zobacz [How to purchase through Volume Licensing](http://www.microsoft.com/en-us/licensing/how-to-buy/how-to-buy.aspx) (Jak kupić w ramach licencjonowania zbiorowego).

## <a name="step-2-activate-your-license-plan"></a>Krok 2: Aktywowanie planu licencjonowania
Czy to Twój pierwszy zakup planu licencjonowania w ramach programu licencjonowania zbiorowego dla przedsiębiorstw firmy Microsoft?
W takim przypadku otrzymasz wiadomość e-mail z potwierdzeniem po wykonaniu zakupu.
Ta wiadomość e-mail będzie potrzebna do aktywowania Twojego pierwszego planu licencjonowania.

W przypadku kolejnych zakupów dla tego katalogu licencje zostaną automatycznie aktywowane w tym samym katalogu.

**Aby aktywować plan licencjonowania, wykonaj jedną z następujących czynności:**

1. Aby rozpocząć aktywację, kliknij pozycję **Zaloguj** lub **Utwórz konto**.
   
    ![Logowanie][1]

    - Jeśli masz istniejącą dzierżawę, kliknij pozycję **Zaloguj**, aby zalogować się przy użyciu istniejącego konta administratora. Musisz zalogować się przy użyciu poświadczeń administratora globalnego z katalogu, w którym zostały aktywowane licencje.

    - Jeśli chcesz utworzyć nową dzierżawę usługi Azure Active Directory do użycia w ramach planu licencjonowania, kliknij pozycję **Utwórz konto**, aby otworzyć okno dialogowe **Tworzenie profilu konta**.

        ![Tworzenie profilu konta][2]

Gdy wszystko będzie gotowe, zostanie wyświetlone następujące okno dialogowe z potwierdzeniem aktywowania planu licencjonowania dla dzierżawy.

![Potwierdzenie][3]

## <a name="step-3-activate-your-azure-active-directory-access"></a>Krok 3: Aktywowanie dostępu do usługi Azure Active Directory
Jeśli Twoje doświadczenie obejmuje korzystanie z platformy Microsoft Azure, możesz przejść do [Kroku 4](#step-4-assign-license-to-user-accounts). 

Gdy licencje zostaną aprowizowane do Twojego katalogu, zostanie do Ciebie wysłana **powitalna wiadomość e-mail**. Wiadomość e-mail stanowi potwierdzenie, że możesz rozpocząć zarządzanie licencjami i funkcjami usługi Azure Active Directory — wersja Premium lub pakietu Enterprise Mobility Suite. 

Jeśli spróbujesz aktywować dostęp do usługi Azure Active Directory przed odebraniem powitalnej wiadomości e-mail, otrzymasz następujący komunikat o błędzie. 

![Brak dostępu][9]

Spróbuj ponownie za kilka minut, po odebraniu wiadomości e-mail.

Nowi administratorzy w Twojej subskrypcji mogą także aktywować swój dostęp do klasycznego portalu Azure za pomocą tego linku.

**Aby aktywować dostęp do usługi Azure Active Directory, wykonaj następujące kroki:**

1. W Twojej **powitalnej wiadomości e-mail** kliknij pozycję **Zaloguj**. 
   
    ![Powitalna wiadomość e-mail][4]
2. Gdy logowanie zostanie wykonane pomyślnie, musisz przeprowadzić logowanie za pomocą drugiego składnika w formularzu weryfikacji mobilnej:
   
    ![Weryfikacja mobilna][5]

Aktywacja może potrwać kilka minut. Gdy dostęp zostanie aktywowany, brązowy pasek zniknie i będzie można kliknąć pozycję **Portal**.

![Zaczekaj na skonfigurowanie][6]

W takim przypadku dostęp do platformy Azure będzie ograniczony do usługi Azure Active Directory.

![Możliwości platformy Azure][7]

Być może masz już dostęp do platformy Azure, ponieważ wcześniej była przez Ciebie używana. Możesz także uaktualnić Twój dostęp do usługi Azure Active Directory do poziomu pełnego dostępu do platformy Azure przez aktywowanie dodatkowych subskrypcji platformy Azure. W takich przypadkach klasyczny portal Azure ma więcej możliwości.

![Możliwości platformy Azure][8]

## <a name="step-4-assign-license-to-user-accounts"></a>Krok 4: Przypisywanie licencji do kont użytkowników
Przed rozpoczęciem używania zakupionego planu musisz ręcznie przypisać licencje do kont użytkowników w organizacji, tak aby mogli używać różnorodnych funkcji udostępnionych w wersji Premium. Wykonaj następujące kroki, aby przypisać licencje do użytkowników, tak aby mogli używać różnorodnych funkcji usługi Azure Active Directory — wersja Premium.

**Aby przypisać licencje do użytkowników, wykonaj następujące kroki:**

1. Zaloguj się do klasycznego portalu Azure jako administrator katalogu, który chcesz dostosować.
2. Kliknij pozycję **Usługa Active Directory**, a następnie wybierz katalog, w którym chcesz przypisać licencje.
3. Wybierz kartę **Licencje**, wybierz pozycję **Usługa Active Directory — wersja Premium** lub **Pakiet Enterprise Mobility Suite**, a następnie kliknij polecenie **Przypisz**.
   
    ![Plany licencjonowania][10]
4. W oknie dialogowym wybierz użytkowników, do których chcesz przypisać licencje, a następnie kliknij ikonę znacznika wyboru, aby zapisać zmiany.
   
    ![Przypisywanie licencji][11]

### <a name="license-restrictions"></a>Ograniczenia licencji
Niektóre plany licencjonowania są podzbiorami lub nadzbiorami innych planów licencjonowania. Zazwyczaj do użytkownika nie można przypisać planu licencjonowania, która już został do niego przypisany. Jeśli masz zamiar przypisać plan licencjonowania, który jest nadzbiorem, musisz najpierw usunąć plan licencjonowania będący podzbiorem.

### <a name="license-requirements"></a>Wymagania licencyjne
Po przypisaniu licencji do użytkownika możesz określić podstawową lokalizację użytkowania we właściwościach jego konta. Jeśli nie określono lokalizacji użytkowania, lokalizacja dzierżawy zostanie automatycznie przypisana do użytkownika.

![Lokalizacja użytkownika][12]

Dostępność usług i funkcji dla usługi w chmurze firmy Microsoft jest zależna od kraju lub regionu. Usługa, taka jak protokół Voice over Internet Protocol (VoIP), może być dostępna w jednym kraju lub regionie i niedostępna w innym. Funkcje w ramach usługi mogą być ograniczone z przyczyn prawnych w niektórych krajach lub regionach. Aby sprawdzić, czy usługa lub funkcja jest dostępna z ograniczeniami, wyszukaj Twój kraj lub region w witrynie ograniczeń licencji usługi.

## <a name="whats-next"></a>Co dalej
* [Dodawanie znakowania firmowego do stron logowania i panelu dostępu](active-directory-add-company-branding.md)
* [Wyświetlanie raportów dostępu i użycia](active-directory-view-access-usage-reports.md)

<!--Image references-->
[1]: ./media/active-directory-get-started-premium/MOLSEmail.png
[2]: ./media/active-directory-get-started-premium/MOLSAccountProfile.png
[3]: ./media/active-directory-get-started-premium/MOLSThankYou.png
[4]: ./media/active-directory-get-started-premium/AADEmail.png
[5]: ./media/active-directory-get-started-premium/SignUppage.png
[6]: ./media/active-directory-get-started-premium/Subscriptionspage.png
[7]: ./media/active-directory-get-started-premium/Premiuminportal.png
[8]: ./media/active-directory-get-started-premium/Premiuminportal_large.png
[9]: ./media/active-directory-get-started-premium/Signuppage_oops.png
[10]: ./media/active-directory-get-started-premium/contosolicenseplan.png
[11]: ./media/active-directory-get-started-premium/Assignlicensespicker.png
[12]: ./media/active-directory-get-started-premium/Usagelocation.png



<!--HONumber=Dec16_HO1-->


