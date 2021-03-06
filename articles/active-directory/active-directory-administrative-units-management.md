---
title: "Podgląd Zarządzanie jednostkami administracyjnymi w usłudze Azure Active Directory"
description: "Przy użyciu jednostek administracyjnych do zapewnienia jeszcze bardziej precyzyjnej delegowania uprawnień w usłudze Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 8464cd6b-1d1a-470d-a4fb-ee29b8eab4c4
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/17/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: oldportal;it-pro;
ms.openlocfilehash: d657eda25f3b26cb793a7ba1a4546f98c08b7e65
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/22/2018
---
# <a name="administrative-units-management-in-azure-ad---public-preview"></a>Zarządzanie jednostkami administracyjnymi w usłudze Azure AD - publicznej wersji zapoznawczej
W tym artykule opisano jednostkami administracyjnymi — nowy kontener usługi Azure Active Directory zasobów, które mogą służyć do delegowania uprawnień administracyjnych za pośrednictwem podzbiór użytkowników i stosowania zasad do podzbioru użytkowników. W usłudze Azure Active Directory jednostki administracyjne Włącz centralnej Administratorzy delegować uprawnienia do administratorów regionalnych lub ustawienie zasad na poziomie szczegółowym.

Jest to przydatne w przypadku organizacji z działów niezależne, na przykład, który składa się z wielu szkoły autonomicznego (służbowe biznesowych, szkoły inżynierii itd.), niezależne od siebie uniwersytetu duże. Takie podziałów ma własnych administratorów IT, którzy kontroli dostępu, zarządzania użytkownikami i ustawić zasady specjalnie z myślą o ich dzielenia. Chcesz centralnej Administratorzy można przyznać tych działów uprawnień administratorów przez użytkowników w ich określonego działów. Dokładniej za pomocą tego przykładu, głównym administratorem można, na przykład utworzyć jednostkę administracyjne dla konkretnego szkole (służbowe Business) i wypełnić ją tylko użytkownicy biznesowi służbowe. Głównym administratorem można dodać służbowego firm personel działu informatycznego do zakresu roli nadawaj innymi słowy, pracownicy działu informatycznego uprawnień administracyjnych służbowe firm tylko za pośrednictwem jednostki administracyjne służbowe biznesowej.

> [!IMPORTANT]
> Do użycia jednostek administracyjnych wymaga administratora o zakresie jednostki administracyjnej mają licencji usługi Azure Active Directory Premium i Azure Active Directory — wersja podstawowa licencji dla wszystkich użytkowników w jednostce administracyjnej. Aby uzyskać więcej informacji, zobacz [wprowadzenie do korzystania z usługi Azure AD Premium](active-directory-get-started-premium.md).
>


Z punktu widzenia administratora centralnego jednostki administracyjne jest obiektu katalogu, które mogą być tworzone i wypełniane przy użyciu zasobów. **W tej wersji zapoznawczej te zasoby może być tylko użytkownicy.** Po tworzone i wypełniane, jednostki administracyjnej można używać jako zakres, aby ograniczyć uprawnienia nadanego tylko za pośrednictwem zasoby zawarte w tej jednostce administracyjnej.

## <a name="managing-administrative-units"></a>Zarządzanie jednostkami administracyjnymi
W tej wersji zapoznawczej można tworzyć i zarządzanie jednostkami administracyjnymi przy użyciu poleceń cmdlet programu Azure Active Directory modułu dla środowiska Windows PowerShell. Aby dowiedzieć się więcej o tym, jak to zrobić, zobacz [Praca z jednostkami administracyjnymi](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0)

Aby uzyskać więcej informacji na temat wymagań programowych i instalowanie modułu usługi Azure AD oraz informacje dotyczące poleceń cmdlet programu Azure AD modułu Zarządzanie jednostkami administracyjnymi, wraz ze składnią, opisy parametrów i przykłady, zobacz [usługi Azure Active PowerShell katalogu](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0).

## <a name="next-steps"></a>Kolejne kroki
[Wersje usługi Azure Active Directory](active-directory-editions.md)
