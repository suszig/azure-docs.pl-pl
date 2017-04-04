---
title: "Zabezpieczanie haseł w usłudze Azure AD i resetowanie haseł blokowanych przez funkcję inteligentnej blokady haseł | Microsoft Docs"
description: "Wyjaśnienia dotyczące pojęcia „dzierżawa usługi Azure AD” i zarządzania nią za pomocą usługi Azure Active Directory"
services: active-directory
documentationcenter: 
author: markvi
writer: v-lorisc
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/02/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: 8e625a346c9495d436a99fcf9eadf8ffeffcfdff
ms.lasthandoff: 03/28/2017


---
# <a name="secure-passwords--in-azure-ad-and-reset-passwords-that-get-blocked-by-smart-password-lockout"></a>Zabezpieczanie haseł w usłudze Azure AD i resetowanie haseł blokowanych przez funkcję inteligentnej blokady haseł
W tym artykule omówiono najlepsze rozwiązania, które można wykorzystać jako użytkownik lub administrator, aby chronić usługę Azure Active Directory (Azure AD) i konta usług konta Microsoft. 

 >[!NOTE]
 >Administratorzy usługi Azure AD mogą resetować hasła użytkowników, klikając nazwę katalogu. W [portalu zarządzania Azure](https://manage.windowsazure.com) wybierz stronę Użytkownicy, kliknij nazwę użytkownika, a następnie opcję Resetuj hasło. 
 >

Usługa Azure AD zapewnia następujące popularne podejścia do zabezpieczania haseł:
 *    Wymagania dotyczące długości hasła
 *    Wymagania dotyczące „złożoności” hasła
 *    Regularne i okresowe wygasanie haseł 

Informacje o możliwościach zarządzania hasłami można znaleźć w dokumencie [Manage passwords in Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-manage-passwords) (Zarządzanie hasłami w usłudze Azure Active Directory). 

## <a name="azure-ad-password-protection"></a>Ochrona haseł w usłudze Azure AD
Usługa Azure AD i system kont Microsoft wykorzystują sprawdzone w branży metody zapewnienia ochrony haseł użytkowników i administratorów. 

W tej sekcji omówiono sposób ochrony haseł przez usługę Azure AD przy użyciu następujących metod:
 *    Hasła dynamicznie zakazane
 *    Inteligentna blokada haseł

Informacje dotyczące zarządzania hasłami oparte na bieżących badaniach można znaleźć w oficjalnym dokumencie [Password Guidance](http://aka.ms/passwordguidance) (Wytyczne dotyczące haseł). 

### <a name="dynamically-banned-passwords"></a>Hasła dynamicznie zakazane
Usługa Azure AD i system kont Microsoft zapewniają ochronę haseł przez dynamiczne zakazywanie wszystkich stosowanych powszechnie haseł. Zespół ds. usługi Azure ID Identity Protection regularnie analizuje listy zakazanych haseł, zapobiegając stosowaniu powszechnie używanych haseł przez użytkowników. Ta usługa jest dostępna dla klientów usługi Azure AD i usługi konta Microsoft. 

Podczas tworzenia hasła ważne jest, aby administratorzy zachęcali użytkowników do wybierania nietypowych haseł zawierających unikatową kombinację liter, cyfr i znaków. Pomaga to w zapewnieniu praktycznej niemożności złamania haseł użytkowników. 

**Listy naruszeń**

Zespół ds. usługi Azure AD nieustannie pracuje, by pozostawać na krok przed przestępcami. Jednym z wykorzystywanych sposobów jest zapobieganie tworzeniu haseł, które znajdują się na aktualnych listach ataków.

Zespół ds. usługi Azure AD Identity Protection nieustannie analizuje często używane hasła. Cyberprzestępcy również często stosują podobne strategie do przeprowadzania ataków, na przykład tworzą [tęczowe tablice](https://en.wikipedia.org/wiki/Rainbow_table) w celu łamania skrótów haseł. 

Firma Microsoft stale analizuje [naruszenia danych](https://www.privacyrights.org/data-breaches) w celu utrzymania dynamicznie aktualizowanej listy zakazanych haseł, dzięki której narażone hasła są zakazywane, zanim staną się prawdziwym zagrożeniem dla klientów usługi Azure AD. Więcej informacji o naszych bieżących działaniach związanych z zabezpieczeniami można znaleźć w raporcie analizy zabezpieczeń firmy Microsoft [Microsoft Security Intelligence Report](https://www.microsoft.com/security/sir/default.aspx). 

### <a name="smart-password-lockout"></a>Inteligentna blokada haseł

Gdy usługa Azure AD wykryje próbę złamania hasła użytkownika przez potencjalnego przestępcę, blokujemy konto użytkownika przy użyciu funkcji inteligentnej blokady haseł. Usługę Azure AD zaprojektowano z myślą o określaniu ryzyka związanego z konkretnymi sesjami logowania. 

Przy użyciu najbardziej aktualnych danych bezpieczeństwa stosujemy semantykę blokad do zagrożeń cybernetycznych. Umożliwia nam to uniknięcie blokowania użytkowników w przypadku złamania przez przestępcę haseł użytkowników w danej sieci.

W przypadku zablokowania użytkownika w usłudze Azure AD jego ekran przypomina następujący:

  ![Blokada w usłudze Azure AD](./media/active-directory-secure-passwords/locked-out-azuread.png)
  
W przypadku innych kont Microsoft ekran użytkownika przypomina następujący:

  ![Blokada konta Microsoft](./media/active-directory-secure-passwords/locked-out-ms-accounts.png)

Informacje o możliwościach zarządzania hasłami w usłudze Azure Active Directory można znaleźć w dokumencie [How password management works](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-how-it-works) (Jak działa zarządzanie hasłami).

  >![UWAGA] Jeśli jesteś administratorem usługi Azure AD, możesz użyć usługi [Windows Hello](https://www.microsoft.com/en-us/windows/windows-hello), aby całkowicie uniknąć tworzenia tradycyjnych haseł przez użytkowników.
  >

## <a name="next-steps"></a>Następne kroki
[Jak zaktualizować własne hasło](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-update-your-own-password)<br>
[The fundamentals of Azure identity management](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals-identity) (Podstawy zarządzania tożsamościami w usłudze Azure)<br>
[How to get operational insights with password management reports](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-get-insights#view-password-reset-activity) (Jak uzyskać informacje operacyjne przy użyciu raportów zarządzania hasłami)



