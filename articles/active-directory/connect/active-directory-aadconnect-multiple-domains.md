---
title: Azure AD Connect wielu domen
description: "W tym dokumencie opisano instalowanie i konfigurowanie wielu domen najwyższego poziomu z usługi Office 365 i Azure AD."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: 5595fb2f-2131-4304-8a31-c52559128ea4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 8e3f496c2868cc3430e0efd47805aec2205168aa
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="multiple-domain-support-for-federating-with-azure-ad"></a>Obsługa wielu domen do federowania w usłudze Azure AD
Poniższa dokumentacja zawiera wskazówki dotyczące sposobu używania wielu najwyższego poziomu domen i poddomen podczas federowania w usłudze Office 365 lub domen usługi Azure AD.

## <a name="multiple-top-level-domain-support"></a>Obsługa wielu domen najwyższego poziomu
Federowanie wielu, domeny najwyższego poziomu z usługą Azure AD wymaga dodatkowego skonfigurowania, który nie jest wymagany, gdy federowania z jednej domeny najwyższego poziomu.

Gdy domeny jest Sfederowane przy użyciu usługi Azure AD, kilka właściwości są ustawiane w domenie na platformie Azure.  Jeden z nich ważne jest IssuerUri.  Jest to identyfikator URI, który jest używany przez usługę Azure AD, aby zidentyfikować domenę, w której token jest skojarzony z.  Identyfikator URI nie musi rozpoznać cokolwiek, ale musi być prawidłowym identyfikatorem URI.  Domyślnie usługi Azure AD ustawia tę wartość Identyfikator usługi federacyjnej w lokalnej usługi AD FS konfiguracji.

> [!NOTE]
> Identyfikator usługi federacyjnej jest identyfikatorem URI, który unikatowo identyfikuje usługi federacyjnej.  Usługi federacyjnej jest wystąpieniem programu AD FS, która działa jako usługę tokenu zabezpieczającego. 
> 
> 

Możesz wyświetlić IssuerUri za pomocą polecenia programu PowerShell `Get-MsolDomainFederationSettings -DomainName <your domain>`.

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

Problem występuje, gdy chcemy dodać więcej niż jednej domeny najwyższego poziomu.  Na przykład, załóżmy, że masz konfiguracji federacji między usługą Azure AD oraz w lokalnym środowisku.  W tym dokumencie używam bmcontoso.com.  Po dodaniu drugiego najwyższego poziomu domeny, bmfabrikam.com.

![Domeny](./media/active-directory-multiple-domains/domains.png)

Gdy firma Microsoft spróbuje przekonwertować naszych domeny bmfabrikam.com federacyjną, możemy komunikat o błędzie.  Przyczyną jest, usługa Azure AD ma ograniczenie, które nie zezwala na właściwość IssuerUri mają taką samą wartość dla więcej niż jedną domenę.  

![Błąd federacyjnego](./media/active-directory-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a>Parametr SupportMultipleDomain
Do rozwiązania, należy dodać inny IssuerUri, można to zrobić za pomocą `-SupportMultipleDomain` parametru.  Ten parametr jest używany z następujących poleceń cmdlet:

* `New-MsolFederatedDomain`
* `Convert-MsolDomaintoFederated`
* `Update-MsolFederatedDomain`

Ten parametr umożliwia usługi Azure AD, skonfiguruj IssuerUri, dzięki czemu jest on oparty na nazwę domeny.  Są to unikatowy między katalogów w usłudze Azure AD.  Za pomocą parametru umożliwia polecenia programu PowerShell zakończyć się pomyślnie.

![Błąd federacyjnego](./media/active-directory-multiple-domains/convert.png)

Patrzeć ustawienia naszej nowej domeny bmfabrikam.com zobacz następujące tematy:

![Błąd federacyjnego](./media/active-directory-multiple-domains/settings.png)

Należy pamiętać, że `-SupportMultipleDomain` nie powoduje zmiany innych punktów końcowych, które są nadal skonfigurowane, aby wskazywał naszej usługi federacyjnej na adfs.bmcontoso.com.

Innym czynnikiem który `-SupportMultipleDomain` jest jest ona sprawdza, czy system usług AD FS zawiera poprawną wartość wystawca w tokenów wystawionych dla usługi Azure AD. Robi to przez pobranie część nazwy UPN użytkowników domeny i to ustawienie jako domeny w IssuerUri, tj. sufiks https://{upn} / adfs/services/zaufania. 

W związku z tym podczas uwierzytelniania do usługi Azure AD lub usługi Office 365, element IssuerUri token użytkownika jest używana do lokalizowania domeny w usłudze Azure AD.  Jeśli nie można odnaleźć dopasowania, że uwierzytelnianie zakończy się niepowodzeniem. 

Na przykład, jeśli nazwy UPN użytkownika jest bsimon@bmcontoso.com, http://bmcontoso.com/adfs/services/trust zostanie ustawiony element IssuerUri problemy tokenu usługi AD FS. To będzie odpowiadała konfiguracji usługi Azure AD i uwierzytelniania powiedzie się.

Poniżej przedstawiono reguły niestandardowe oświadczeń, który implementuje tę logikę:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


> [!IMPORTANT]
> Aby użyć przełącznika - SupportMultipleDomain, gdy próbuje dodać nowe lub przekonwertować już dodane do domeny, musisz mieć Instalatora z zaufaniem federacyjnym do pierwotnie ich obsługi.  
> 
> 

## <a name="how-to-update-the-trust-between-ad-fs-and-azure-ad"></a>Jak zaktualizować relacji zaufania między usługami AD FS a usługą Azure AD
Nie uaktualniono konfiguracji federacyjnych relacji zaufania między usługami AD FS a wystąpieniem usługi Azure AD, należy ponownie utworzyć zaufanie.  To dlatego kiedy została pierwotnie bez `-SupportMultipleDomain` parametru IssuerUri została skonfigurowana z wartością domyślną.  Na poniższym zrzucie ekranu można zauważyć, że IssuerUri ma ustawioną wartość https://adfs.bmcontoso.com/adfs/services/trust.

Teraz tak, jeśli firma Microsoft pomyślnie dodano nową domenę w portalu usługi Azure AD, a następnie spróbuj przekonwertować ją przy użyciu `Convert-MsolDomaintoFederated -DomainName <your domain>`, uzyskujemy następujący błąd.

![Błąd federacyjnego](./media/active-directory-multiple-domains/trust1.png)

Jeśli próbujesz dodać `-SupportMultipleDomain` przełącznika możemy zostanie wyświetlony następujący błąd:

![Błąd federacyjnego](./media/active-directory-multiple-domains/trust2.png)

Po prostu próby uruchomienia `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` w domenie oryginalnego również spowoduje błąd.

![Błąd federacyjnego](./media/active-directory-multiple-domains/trust3.png)

Skorzystaj z poniższych czynności, aby dodać dodatkowe domeny najwyższego poziomu.  Jeśli dodano już domeny i nie używa `-SupportMultipleDomain` start parametru prostych kroków, usuwania i aktualizowania domenę oryginalnego.  Jeśli domeny najwyższego poziomu nie dodano jeszcze można uruchomić z poziomu procedury dodawania domeny przy użyciu programu PowerShell Azure AD Connect.

Wykonaj następujące kroki, aby usunąć zaufanie Online firmy Microsoft i zaktualizować domenę oryginalnego.

1. Na serwerze federacyjnym usług AD FS Otwórz **Zarządzanie usługami AD FS.** 
2. Po lewej stronie rozwiń **relacje zaufania** i **zaufania jednostek uzależnionych**
3. Po prawej stronie, należy usunąć **Microsoft Office 365 tożsamość platformy** wpisu.
   ![Usuń Online firmy Microsoft](./media/active-directory-multiple-domains/trust4.png)
4. Na komputerze, na którym ma [Azure Active Directory modułu dla środowiska Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) zainstalowanym uruchom następujące polecenie: `$cred=Get-Credential`.  
5. Wprowadź nazwę użytkownika i hasło administratora globalnego dla domeny usługi Azure AD, które są federowania w usłudze
6. W programie PowerShell wpisz:`Connect-MsolService -Credential $cred`
7. W programie PowerShell wpisz `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.  Jest to oryginalna domeny.  Przy użyciu powyższej domeny, które będzie:`Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`

Wykonaj następujące kroki, aby dodać nową domenę najwyższego poziomu przy użyciu programu PowerShell

1. Na komputerze, na którym ma [Azure Active Directory modułu dla środowiska Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) zainstalowanym uruchom następujące polecenie: `$cred=Get-Credential`.  
2. Wprowadź nazwę użytkownika i hasło administratora globalnego dla domeny usługi Azure AD, które są federowania w usłudze
3. W programie PowerShell wpisz:`Connect-MsolService -Credential $cred`
4. W programie PowerShell wpisz:`New-MsolFederatedDomain –SupportMultipleDomain –DomainName`

Wykonaj następujące kroki, aby dodać nową domenę najwyższego poziomu za pomocą usługi Azure AD Connect.

1. Uruchamianie usługi Azure AD Connect z pulpitu lub menu start
2. Wybierz opcję "Dodaj dodatkowe domenowych Azure AD" ![Dodawanie kolejnej domeny usługi Azure AD](./media/active-directory-multiple-domains/add1.png)
3. Wprowadź usługi Azure AD i poświadczeń usługi Active Directory
4. Wybierz drugiej domeny, który chcesz skonfigurować na potrzeby federacji.
   ![Dodawanie kolejnej domeny usługi Azure AD](./media/active-directory-multiple-domains/add2.png)
5. Kliknij przycisk Instaluj

### <a name="verify-the-new-top-level-domain"></a>Sprawdź nowej domeny najwyższego poziomu
Za pomocą polecenia programu PowerShell `Get-MsolDomainFederationSettings -DomainName <your domain>`można wyświetlić zaktualizowane IssuerUri.  Poniższy zrzut ekranu przedstawia Federacji ustawienia zostały zaktualizowane na naszych oryginalnego http://bmcontoso.com/adfs/services/trust domeny

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

I IssuerUri naszej nowej domeny została ustawiona jako https://bmfabrikam.com/adfs/services/trust

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/settings2.png)

## <a name="support-for-sub-domains"></a>Obsługa domen podrzędnych
Po dodaniu domenę podrzędną, ze względu na sposób Azure AD obsługi domen odziedziczy ustawienia elementu nadrzędnego.  Oznacza to, że IssuerUri musi być zgodna elementów nadrzędnych.

Dlatego umożliwia Powiedz, na przykład, że I bmcontoso.com i następnie dodać corp.bmcontoso.com.  Oznacza to, że IssuerUri dla użytkownika z corp.bmcontoso.com musi być **http://bmcontoso.com/adfs/services/trust.**  Jednak standardowa zasada zaimplementowana powyżej dla usługi Azure AD wygeneruje token z wystawcą jako **http://corp.bmcontoso.com/adfs/services/trust.** nie będzie odpowiadała domeny wymagane wartości, a uwierzytelnianie zakończy się niepowodzeniem.

### <a name="how-to-enable-support-for-sub-domains"></a>Jak włączyć obsługę poddomen
Aby obejść ten problem usług AD FS zaufania jednostki uzależnionej dla witryny Microsoft Online musi zostać zaktualizowany.  Aby to zrobić, należy skonfigurować niestandardowej reguły oświadczeń tak, aby taśmy Wyłącz wszelkie poddomen z sufiks nazwy UPN użytkownika podczas tworzenia niestandardowej wartości wystawcy. 

Następujące oświadczenie będzie wykonaj następujące czynności:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

[!NOTE]
Ostatni numer w wyrażeniu regularnym Ustaw liczbę domeny nadrzędnej istnieje w domenie katalogu głównego. W tym miejscu można mieć bmcontoso.com, niezbędne są dwie domeny nadrzędnej. Jeśli trzy nadrzędnego domeny zostały mają być przechowywane (tj.: corp.bmcontoso.com), następnie numer byłby trzy. Mogą być wskazywane Eventualy zakresu, zawsze będą dopasowania do dopasowania maksymalną liczbę domen. "{2,3}" będzie odpowiadała dwóch do trzech domenach (np.: bmfabrikam.com i corp.bmcontoso.com).

Wykonaj następujące kroki, aby dodać niestandardowe oświadczenia do obsługi domenami podrzędnymi.

1. Zarządzanie usługami Otwórz AD FS
2. Zaufanie RP Online firmy Microsoft kliknij prawym przyciskiem myszy i wybierz polecenie reguły Edycja oświadczeń
3. Wybierz trzecie reguły oświadczeń i Zastąp ![Edycja oświadczeń](./media/active-directory-multiple-domains/sub1.png)
4. Zastąp bieżący oświadczeń:
   
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));
   
       with
   
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

    ![Zastąp oświadczeń](./media/active-directory-multiple-domains/sub2.png)

5. Kliknij przycisk Ok.  Kliknij przycisk Zastosuj.  Kliknij przycisk Ok.  Zamknij przystawkę zarządzania usługami AD FS.

