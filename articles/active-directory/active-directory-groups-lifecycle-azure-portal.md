---
title: "Wygaśnięcie grup usługi Office 365 w usłudze Azure Active Directory w wersji Preview | Dokumentacja firmy Microsoft"
description: "Jak skonfigurować wygaśnięcia dla grup usługi Office 365 w usłudze Azure Active Directory (wersja zapoznawcza)"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro
ms.openlocfilehash: 8a43df84fd050d7b4bd8d937b8c55e744cb805d3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-office-365-groups-expiration-preview"></a>Skonfiguruj wygaśnięcia grup usługi Office 365 (wersja zapoznawcza)

Umożliwia teraz Zarządzanie cyklem życia grup usługi Office 365, ustawiając wygaśnięcia w wybranych grupach usługi Office 365. Po ustawieniu tej wygaśnięcia właścicieli tych grup są proszeni o odnowić ich grup, jeśli nadal potrzebujesz grup. Wszystkie grupy usługi Office 365, która nie zostanie odnowiony zostaną usunięte. W ciągu 30 dni można przywrócić żadnej grupy usługi Office 365, który został usunięty przez właścicieli grupy lub administratora.  


> [!NOTE]
> Można ustawić wygaśnięcie tylko grup usługi Office 365.
>
> Ustawianie ważności dla grup usługi Office 365 wymaga przypisania licencji usługi Azure AD Premium do
>   - Administrator, który konfiguruje ustawienia wygaśnięcia dla dzierżawcy
>   - Wszyscy członkowie grupy wybrana dla tego ustawienia

## <a name="set-office-365-groups-expiration"></a>Ustawienia okresu ważności grup usługi Office 365

1. Otwórz [Centrum administracyjnego usługi Azure AD](https://aad.portal.azure.com) przy użyciu konta, które jest administratorem globalnym w dzierżawie usługi Azure AD.

2. Otwórz usługę Azure AD, wybierz **użytkowników i grup**.

3. Wybierz **ustawienia grupy** , a następnie wybierz **wygaśnięcia** aby otworzyć ustawienia wygaśnięcia.
  
  ![Blok wygaśnięcia](./media/active-directory-groups-lifecycle-azure-portal/expiration-settings.png)

4. Na **wygaśnięcia** bloku, można wykonywać następujące czynności:

  * Ustaw okres istnienia grupy w dniach. Możesz wybrać jedną z istniejących wartości lub wartości niestandardowych (powinien być 31 dni lub więcej). 
  * Określ adres e-mail, w którym mają być wysyłane powiadomienia odnowienia i wygaśnięcia po grupie nie ma ono właściciela. 
  * Wybierz grupy, które usługi Office 365 wygaśnie. Można włączyć wygaśnięcia dla **wszystkie** grup usługi Office 365, możesz wybrać spośród grup usługi Office 365 lub wybrać **Brak** wyłączyć wygaśnięcia dla wszystkich grup.
  * Zapisz ustawienia, gdy wszystko będzie gotowe, wybierając **zapisać**.

Aby uzyskać instrukcje jak pobrać i zainstalować moduł PowerShell programu Microsoft do skonfigurowania wygaśnięcia dla grup usługi Office 365 za pomocą programu PowerShell, zobacz [Azure V2 PowerShell moduł usługi Active Directory - publicznej wersji zapoznawczej 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).

Powiadomienia e-mail, taką jak są wysyłane do właścicieli grupy usługi Office 365 30 dni, 15 dni i 1 dzień przed jego wygaśnięciem grupy.

![Powiadomienia e-mail o wygaśnięciu](./media/active-directory-groups-lifecycle-azure-portal/expiration-notification.png)

Właściciel grupy można następnie wybrać **grupy odnawiania** odnowić usługi Office 365 grupy lub wybrać **przejdź do grupy** elementów członkowskich i inne szczegółowe informacje o grupie.

Po wygaśnięciu grupy, grupa zostanie usunięta jeden dzień po dacie wygaśnięcia. Powiadomienie e-mail, taką jak są wysyłane do właścicieli grupy usługi Office 365 informująca o wygaśnięciu i kolejne usunięcie ich z grupy usługi Office 365.

![Powiadomienie e-mail usunięcia grupy](./media/active-directory-groups-lifecycle-azure-portal/deletion-notification.png)

Grupy można przywrócić, wybierając **grupy przywracania** lub za pomocą poleceń cmdlet programu PowerShell, zgodnie z opisem w [przywracanie usuniętych usługi Office 365 grupy w usłudze Azure Active Directory] (https://docs.microsoft.com/azure/active-directory/active-directory-groups-restore-azure-portal).
    
Jeśli grupy, które są przywracane zawiera dokumentów, witryn programu SharePoint lub inne obiekty trwałe, może potrwać do 24 godzin pełni przywrócić grupy i jego zawartość.

> [!NOTE]
> * Podczas wdrażania ustawień wygasania, może być niektórych grup, które są starsze niż okna wygaśnięcia. Te grupy nie są można natychmiast usunąć, ale są ustawione na 30 dni do wygaśnięcia. Pierwszy odnawiania powiadomienia e-mail jest wysyłane w ciągu dnia. Na przykład A grupa została utworzona 400 dni temu i interwał wygaśnięcia jest ustawiony na wartość 180 dni. Podczas stosowania ustawień wygasania grupy A ma 30 dni przed usunięciem, chyba że właściciel odnawia go.
> * Gdy dynamiczna grupa jest usuwane i przywrócić, jest traktowany jako nową grupę i ponownie wypełniane zgodnie z zasadą. Ten proces może potrwać do 24 godzin.

## <a name="next-steps"></a>Następne kroki
Te artykuły zawierają dodatkowe informacje o grup usługi Azure AD.

* [Zobacz istniejących grup](active-directory-groups-view-azure-portal.md)
* [Zarządzanie ustawieniami grupy](active-directory-groups-settings-azure-portal.md)
* [Elementy członkowskie grupy zarządzania](active-directory-groups-members-azure-portal.md)
* [Zarządzanie członkostwami grup](active-directory-groups-membership-azure-portal.md)
* [Dynamiczne reguły dla użytkowników w grupie zarządzania](active-directory-groups-dynamic-membership-azure-portal.md)
