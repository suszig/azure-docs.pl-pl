---
title: "Grupuje ważności dla usługi Office 365 w usłudze Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Jak skonfigurować wygaśnięcia dla grup usługi Office 365 w usłudze Azure Active Directory (wersja zapoznawcza)"
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro
ms.openlocfilehash: d485d2e7d22ea79a87dc52dbc063a811f4a1a2ec
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="configure-expiration-for-office-365-groups-preview"></a>Skonfiguruj wygaśnięcia dla grup usługi Office 365 (wersja zapoznawcza)

Umożliwia teraz Zarządzanie cyklem życia grup usługi Office 365 przez ustawienie dla nich funkcji wygaśnięcia. W usłudze Azure Active Directory (Azure AD), można ustawić wygaśnięcie tylko grup usługi Office 365. Po ustawieniu grupy wygaśnie:
-   Właścicieli grupy są powiadamiani o odnowić grupy, jak będzie bliski wygaśnięcia
-   Wszystkie grupy, która nie zostanie odnowiony jest usuwany.
-   Można przywrócić żadnej grupy usługi Office 365, która zostanie usunięta w ciągu 30 dni przez właścicieli grupy lub administrator

> [!NOTE]
> Ustawianie ważności dla grup usługi Office 365 wymaga licencji usługi Azure AD Premium lub licencji usługi Azure AD podstawowa Education dla wszystkich członków grupy, do której są stosowane ustawienia wygaśnięcia.
> 
> Dla usługi Azure AD podstawowa Education licencjonowane klientów: Aby po raz pierwszy, należy skonfigurować te zasady, użyj poleceń cmdlet środowiska PowerShell usługi Azure Active Directory. Po tym należy zaktualizować ustawienia wygaśnięcia przy użyciu programu PowerShell lub portalu usługi Azure AD przy użyciu konta, które jest administratorem konta użytkownika lub administratora globalnego dzierżawy usługi Azure AD.

Aby uzyskać informacje na temat Pobierz i zainstaluj polecenia cmdlet programu PowerShell usługi Azure AD, zobacz [Azure Active Directory PowerShell wykresu — publicznej wersji zapoznawczej 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).

## <a name="set-group-expiration"></a>Ustaw grupy wygaśnięcia

1. Otwórz [Centrum administracyjnego usługi Azure AD](https://aad.portal.azure.com) przy użyciu konta, które jest administratorem globalnym w dzierżawie usługi Azure AD.

2. Otwórz usługę Azure AD, wybierz **użytkowników i grup**.

3. Wybierz **ustawienia grupy** , a następnie wybierz **wygaśnięcia** aby otworzyć ustawienia wygaśnięcia.
  
  ![Blok wygaśnięcia](./media/active-directory-groups-lifecycle-azure-portal/expiration-settings.png)

4. Na **wygaśnięcia** bloku, można wykonywać następujące czynności:

  * Ustaw okres istnienia grupy w dniach. Możesz wybrać jedną z istniejących wartości lub wartości niestandardowych (powinien być 31 dni lub więcej). 
  * Określ adres e-mail, w którym mają być wysyłane powiadomienia odnowienia i wygaśnięcia po grupie nie ma ono właściciela. 
  * Wybierz grupy, które usługi Office 365 wygaśnie. Można włączyć wygaśnięcia dla **wszystkie** grup usługi Office 365, możesz wybrać spośród grup usługi Office 365 lub wybrać **Brak** wyłączyć wygaśnięcia dla wszystkich grup.
  * Zapisz ustawienia, gdy wszystko będzie gotowe, wybierając **zapisać**.


Powiadomienia e-mail, taką jak są wysyłane do właścicieli grupy usługi Office 365 30 dni, 15 dni i 1 dzień przed jego wygaśnięciem grupy.

![Powiadomienia e-mail o wygaśnięciu](./media/active-directory-groups-lifecycle-azure-portal/expiration-notification.png)

Właściciel grupy można następnie wybrać **grupy odnawiania** odnowić usługi Office 365 grupy lub wybrać **przejdź do grupy** elementów członkowskich i inne szczegółowe informacje o grupie.

Po wygaśnięciu grupy, grupa zostanie usunięta jeden dzień po dacie wygaśnięcia. Powiadomienie e-mail, taką jak są wysyłane do właścicieli grupy usługi Office 365 informująca o wygaśnięciu i kolejne usunięcie ich z grupy usługi Office 365.

![Powiadomienie e-mail usunięcia grupy](./media/active-directory-groups-lifecycle-azure-portal/deletion-notification.png)

Grupy można przywrócić, wybierając **grupy przywracania** lub za pomocą poleceń cmdlet programu PowerShell, zgodnie z opisem w [przywracanie usuniętych usługi Office 365 grupy w usłudze Azure Active Directory] (https://docs.microsoft.com/azure/active-directory/active-directory-groups-restore-azure-portal).
    
Jeśli grupy, które są przywracane zawiera dokumentów, witryn programu SharePoint lub inne obiekty trwałe, może potrwać do 24 godzin pełni przywrócić grupy i jego zawartość.

> [!NOTE]
> * Podczas pierwszej konfiguracji wygaśnięcia tych grup, które są starsze niż interwał ważności są ustawiane na 30 dni do wygaśnięcia. Pierwszy odnawiania powiadomienia e-mail jest wysyłane w ciągu dnia. 
>   Na przykład A grupa została utworzona 400 dni temu i interwał wygaśnięcia jest ustawiony na wartość 180 dni. Podczas stosowania ustawień wygasania grupy A ma 30 dni przed usunięciem, chyba że właściciel odnawia go.
> * Gdy dynamiczna grupa jest usuwane i przywrócić, jest traktowany jako nową grupę i ponownie wypełniane zgodnie z zasadą. Ten proces może potrwać do 24 godzin.

## <a name="next-steps"></a>Następne kroki
Te artykuły zawierają dodatkowe informacje o grup usługi Azure AD.

* [Zobacz istniejących grup](active-directory-groups-view-azure-portal.md)
* [Zarządzanie ustawieniami grupy](active-directory-groups-settings-azure-portal.md)
* [Elementy członkowskie grupy zarządzania](active-directory-groups-members-azure-portal.md)
* [Zarządzanie członkostwami grup](active-directory-groups-membership-azure-portal.md)
* [Dynamiczne reguły dla użytkowników w grupie zarządzania](active-directory-groups-dynamic-membership-azure-portal.md)
