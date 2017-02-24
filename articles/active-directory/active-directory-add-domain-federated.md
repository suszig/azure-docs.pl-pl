---
title: "Dodawanie niestandardowej nazwy domeny i konfigurowanie logowania federacyjnego do usługi Azure Active Directory | Microsoft Docs"
description: "Dodawanie nazw domeny firmy do usługi Azure Active Directory i konfigurowanie logowania federacyjnego między usługą Azure Active Directory a federacyjnym rozwiązaniem lokalnym."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 27126c7e-e6d6-4ef3-a4fb-f5f0706e749d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/04/2016
ms.author: curtand;jeffsta
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 77d34a6ebce12465a15de158b9160a03db58c48e


---
# <a name="add-your-custom-domain-name-to-azure-active-directory"></a>Dodawanie niestandardowej nazwy domeny do usługi Azure Active Directory
Możesz skonfigurować niestandardową nazwę domeny, taką jak „contoso.com”, aby użytkownicy w contoso.com mogli mieć środowisko federacyjnego logowania jednokrotnego z sieci firmowej. Jeśli masz już usługi Active Directory Federation Services (AD FS) lub inny serwer federacyjny uruchomiony w sieci firmowej, możesz skonfigurować usługę Azure AD do korzystania z niestandardowej nazwy domeny za pomocą narzędzia Azure AD Connect. Możesz także użyć programu Azure AD Connect w celu wdrożenia nowego środowiska usług AD FS i skonfigurować je dla federacyjnego logowania jednokrotnego do usługi Azure AD.

Jeśli nie masz i planujesz wdrożenia usług AD FS lub innego serwera federacyjnego, wykonaj te instrukcje: [Dodawanie niestandardowej nazwy domeny do usługi Azure Active Directory](active-directory-add-domain.md).

## <a name="add-a-custom-domain-name-to-your-directory"></a>Dodawanie niestandardowej nazwy domeny do katalogu
1. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com/) przy użyciu konta użytkownika będącego administratorem globalnym katalogu usługi Azure AD.
2. W usłudze **Active Directory** otwórz katalog i wybierz kartę **Domeny**.
3. Na pasku poleceń wybierz pozycję **Dodaj**, a następnie wprowadź niestandardową nazwę domeny, taką jak „contoso.com”. Należy uwzględnić rozszerzenie .com, .net lub inne rozszerzenie najwyższego poziomu.
4. Zaznacz pole wyboru **Planuję skonfigurować logowanie jednokrotne w tej domenie przy użyciu lokalnej usługi Active Directory**.
5. Wybierz pozycję **Dodaj**.

Uruchom narzędzie Azure AD Connect, aby uzyskać wpis DNS, który będzie używany przez usługę Azure AD do weryfikowania domeny. Wpis DNS pojawi się w kroku **Domena usługi Azure AD** w kreatorze. Wygląd tego kroku w Kreatorze można zobaczyć w [tych instrukcjach](connect/active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation). Jeśli nie masz narzędzia Azure AD Connect, możesz [pobrać jej tutaj](http://go.microsoft.com/fwlink/?LinkId=615771).

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>Dodawanie wpisu DNS w rejestratorze nazw domen dla domeny
Następnym krokiem do korzystania z niestandardowej nazwy domeny w usłudze Azure AD jest zaktualizowanie pliku strefy DNS dla domeny. Umożliwi to usłudze Azure AD sprawdzenie, czy organizacja jest właścicielem niestandardowej nazwy domeny.

1. Zaloguj się w witrynie sieci Web u rejestratora nazw domen dla nazwy domeny. Jeśli nie masz dostępu, aby to zrobić, poproś osobę lub zespół w organizacji, którzy mają odpowiedni dostęp, o wykonanie kroku 2 i powiadomienie Cię o jego zakończeniu.
2. Zaktualizuj plik strefy DNS dla domeny, dodając wpis DNS udostępniony przez usługę Azure AD. Ten wpis DNS umożliwia usłudze Azure AD zweryfikowanie prawa własności do domeny. Ten wpis DNS nie zmienia żadnych zachowań, takich jak routing poczty lub hosting sieci Web.

Aby uzyskać pomoc dotyczącą tego kroku, przeczytaj [Instrukcje dotyczące dodawania wpisu DNS u popularnych rejestratorów DNS](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-the-domain-name-with-azure-ad"></a>Weryfikowanie nazwy domeny w usłudze Azure AD
Po dodaniu wpisu DNS można przystąpić do weryfikowania nazwy domeny w usłudze Azure AD.

Aby zweryfikować domenę, wybierz opcję **Dalej** w kroku **Domena usługi Azure AD** kreatora instalacji programu Azure AD Connect. Usługa Azure AD będzie szukała wpisu DNS w pliku strefy DNS dla domeny. Usługa Azure AD zweryfikuje nazwę domeny dopiero po wykonaniu propagacji rekordów DNS. Propagacja często zajmuje tylko kilka sekund, ale czasami może potrwać godzinę lub dłużej. Jeśli weryfikacja nie zadziała za pierwszym razem, spróbuj ponownie później.

Następnie wykonaj pozostałe kroki w kreatorze instalacji programu Azure AD Connect. Spowoduje to synchronizację użytkowników usługi Windows Server AD z usługą Azure AD. Zsynchronizowani użytkownicy w domenie skonfigurowanej na potrzeby federacji będą mogli uzyskać federacyjne logowanie jednokrotne z sieci firmowej do usługi Azure AD.

## <a name="troubleshooting"></a>Rozwiązywanie problemów
Jeśli nie można zweryfikować niestandardowej nazwy domeny, spróbuj następujących rozwiązań. Zaczniemy od najczęstszych i skończymy na najrzadziej występujących.

1. **Zaczekaj godzinę**. Rekordy DNS muszą zostać poddane propagacji, aby usługa Azure AD mogła zweryfikować domenę. Propagacja może zająć godzinę lub dłużej.
2. **Upewnij się, że wprowadzono rekord DNS i że jest on poprawny**. Ten krok należy wykonać w witrynie sieci Web u rejestratora nazw domen dla domeny. Usługa Azure AD nie może zweryfikować nazwy domeny, jeśli wpis DNS nie jest obecny w pliku strefy DNS lub jeśli nie jest identyczny z wpisem DNS udostępnionym użytkownikowi przez usługę Azure AD. Jeśli nie masz dostępu do aktualizowania rekordów DNS domeny w rejestrze nazw domen, udostępnij wpis DNS osobie lub zespołowi w organizacji, którzy mają taki dostęp, i poproś o dodanie wpisu DNS.
3. **Usuń nazwę domeny z innego katalogu w usłudze Azure AD**. Nazwę domeny można weryfikować tylko w jednym katalogu. Jeśli nazwa domeny została zweryfikowana wcześniej w innym katalogu, musi zostać usunięta, zanim będzie ją można zweryfikować w nowym katalogu. Aby dowiedzieć się więcej na temat usuwania nazw domen, przeczytaj artykuł [Zarządzanie niestandardowymi nazwami domen](active-directory-add-manage-domain-names.md).

## <a name="add-more-custom-domain-names"></a>Dodawanie niestandardowych nazw domen
Jeśli Twoja organizacja używa wielu niestandardowych nazw domen, takich jak „contoso.com” czy „contosobank.com”, możesz je dodać (maksymalnie 900 nazw domen). Dla każdej dodawanej nazwy domeny wykonaj kroki opisane w tym artykule.

## <a name="next-steps"></a>Następne kroki
* [Zarządzanie niestandardowymi nazwami domen](active-directory-add-manage-domain-names.md)
* [Zapoznanie z koncepcjami związanymi z zarządzaniem domenami w usłudze Azure AD](active-directory-add-domain-concepts.md)
* [Wyświetlanie znakowania firmowego podczas logowania użytkowników](active-directory-add-company-branding.md)
* [Zarządzanie nazwami domen w usłudze Azure AD przy użyciu programu PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)




<!--HONumber=Dec16_HO1-->


