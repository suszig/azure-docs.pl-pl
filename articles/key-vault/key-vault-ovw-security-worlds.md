---
ms.assetid: 
title: "Względem zabezpieczeń usługi Azure Key Vault | Dokumentacja firmy Microsoft"
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 07/03/2017
ms.openlocfilehash: 921bbd109c9ea98d8b5c286a7512bed026412d26
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-key-vault-security-worlds-and-geographic-boundaries"></a>Środowiska usługi Azure Key Vault security World i geograficzne granice

Usługa Azure Key Vault jest usługą wielodostępnych i korzysta z puli sprzętowych modułów zabezpieczeń (HSM) w każdej lokalizacji platformy Azure. 

Wszystkie moduły HSM w lokalizacjach Azure w tym samym regionie geograficznym udostępnianie tej samej granicy kryptograficznych (środowiska zabezpieczeń firmy Thales). Na przykład wschodnie stany USA i zachodnie stany USA udziału tego samego środowiska zabezpieczeń security world, ponieważ należą do lokalizacja geograficzna Stanów Zjednoczonych. Podobnie wszystkich lokalizacji platformy Azure w Japonii współużytkowanie tego samego środowiska zabezpieczeń security world i wszystkich lokalizacji platformy Azure w Australii, Indie i tak dalej. 

## <a name="backup-and-restore-behavior"></a>Zachowanie kopii zapasowej i przywracania

Do magazynu kluczy w innej lokalizacji platformy Azure, można przywrócić kopii zapasowej klucza z magazynu kluczy w jednej lokalizacji platformy Azure, dopóki oba te warunki są spełnione:

- Azure lokalizacji należeć do tej samej lokalizacji geograficznej
- Zarówno magazynów kluczy należą do tej samej subskrypcji platformy Azure

Na przykład kopii zapasowej, w ramach danej subskrypcji klucza w magazynie kluczy w Indie Zachodnie można przywrócić tylko do innego magazynu kluczy w tej samej subskrypcji i lokalizacji geo. Indie Zachodnie, Indie środkowe lub Indie Południowe

## <a name="regions-and-products"></a>Regiony i produktów

- [Regiony platformy Azure](https://azure.microsoft.com/regions/)
- [Produkty firmy Microsoft według regionu](https://azure.microsoft.com/regions/services/)

Regiony są mapowane do środowiska security World, wyświetlane jako głównych pozycji w tabelach:

W produktach artykule region, na przykład **Americas** karta zawiera wschód US, ŚRODKOWE stany USA, zachodnie stany USA wszystkie mapowania do obszaru Americas. 

>[!NOTE]
>Wyjątek jest nam wschodnie DOD i nam DOD centralnej własnych środowiska security World. 

Podobnie na **Europy** karcie, Europa Północna, EUROPA i EUROPA ZACHODNIA, są mapowane na regionie Europy. Dotyczy to również na **Azja i Pacyfik** kartę.



