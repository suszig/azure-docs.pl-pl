---
title: "Usługi Azure Active Directory B2C: Uwagi dla deweloperów przy użyciu zasad niestandardowych | Dokumentacja firmy Microsoft"
description: "Uwagi dla deweloperów na skonfigurowaniu i utrzymywaniu z niestandardowych zasad usługi Azure AD B2C"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 05/05/2017
ms.author: joroja
ms.openlocfilehash: a5f222e5b11e05286152a9f1cc55d2c3fc27a9dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="release-notes-for-azure-active-directory-b2c-custom-policy-public-preview"></a>Informacje o wersji dla publicznej wersji zapoznawczej usługi Azure Active Directory B2C zasad niestandardowych
Zestaw funkcji niestandardowych zasad jest teraz dostępna w wersji ewaluacyjnej w publicznej wersji zapoznawczej dla wszystkich usługi Azure Active Directory B2C (Azure AD B2C) klientów. Ten zestaw funkcji jest przeznaczona dla deweloperów zaawansowane tożsamości tworzenia najbardziej złożonych rozwiązań tożsamości.  

Obecnie ten zestaw funkcji wymaga deweloperom skonfigurowania struktury obsługi tożsamości bezpośrednio za pomocą edycji pliku XML. Ta metoda konfiguracji jest wydajny i złożone. Zaawansowane tożsamości deweloperzy przy użyciu platformy obsługi tożsamości należy zaplanować inwestycji pewien czas ukończenia przewodników i odczytywanie dokumentów odwołania. 

## <a name="features-included-in-this-public-preview"></a>Funkcje zawarte w tym publicznej wersji zapoznawczej
Z nowych funkcji wprowadzonych w publicznej wersji zapoznawczej deweloperzy mogą wykonywać następujące zadania:<br>

* Tworzenie i przekazywanie niestandardowe uwierzytelnianie użytkownika podróże za pomocą niestandardowych zasad. 
   * Opisano krok po kroku jako wymiany podróże użytkownika od dostawcy oświadczeń. 
   * Zdefiniuj rozgałęzień warunkowych w podróży użytkownika. 
* Integracja usług korzystających z interfejsu API REST w podróży użytkownika z uwierzytelniania niestandardowego.  
* Dodaj federacji z dostawców tożsamości, zgodnych ze standardowych OpenIDConnect. <br>
* Dodaj federacji z dostawców tożsamości, które jest zgodna z protokołu SAML 2.0. 

## <a name="terms-of-the-public-preview"></a>Warunki publicznej wersji zapoznawczej

* Firma Microsoft zachęca do korzystania z nowych funkcji wyłącznie do celów ewaluacyjnych.<br>
* Nowe funkcje nie są przeznaczone do użytku w środowisku produkcyjnym.<br>
* Nowe funkcje nie dotyczą umów dotyczących poziomu usług (SLA). <br>
* Żądania pomocy technicznej można złożyć za pośrednictwem kanałów regularne pomocy technicznej. <br>
* Brak bez uzgodnionej daty dla ogólnej dostępności.<br>
* Nasze uznania z dowolnej przyczyny Microsoft może Flaga i odrzucić lub ograniczyć scenariuszy i podróże użytkownika, które wykraczają poza karty produktu usługi Azure AD B2C do służy jako platforma zarządzania (CIAM) tożsamości i dostępu klienta.

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a>Obowiązki deweloperów zestaw funkcji zasad niestandardowych
Konfiguracja zasad ręczne udziela dostępu niższego poziomu podstawowej platformy Azure AD B2C i spowoduje powstanie framework unikatowy zaufania można swobodnie dostosowywać. Możliwych kombinacji dostawców tożsamości niestandardowej, relacje zaufania, integracji z usługami zewnętrznych i przepływy pracy krok po kroku umieścić większa zapotrzebowanie na zaawansowanych deweloperów korzystających z nich.

Aby w pełni skorzystać z publicznej wersji zapoznawczej, zalecamy deweloperom korzystanie z niestandardowych zasad zestaw funkcji jest zgodna z następującymi zasadami:
* Zapoznanie się z języka konfiguracji aparat obsługi tożsamości i klucz/klucze tajne zarządzania.
* Przejmowanie na własność scenariuszy i niestandardowej integracji.
* Testowanie metodyczny scenariusza.
* Wykonaj rozwoju oprogramowania i przejściowych najlepszych rozwiązań z co najmniej jeden programowanie i środowiska testowego i jednego środowiska produkcyjnego.
* Aktualne informacje o nowych rozwiązań z dostawców tożsamości i usługi, którą można zintegrować z. Na przykład śledzenie pracy w kluczy tajnych i planowanych i nieplanowanych zmiany w usłudze.
* Konfigurowanie aktywnego monitorowania i monitorowanie czasu reakcji środowisk produkcyjnych.
* Aktualizuj adresy e-mail kontaktów i pozostać odbierać wiadomości e-mail na żywo witryny zespołu firmy Microsoft.
* Podjąć działania odpowiednim przypadku zaleca się, aby to zrobić przez zespół live witryny firmy Microsoft. 


>[!NOTE]
>Te funkcje mogą ostatecznie dołączone wbudowane zasady usługi Azure AD, dzięki czemu łatwiej dostępne dla wszystkich deweloperów.

## <a name="next-steps"></a>Następne kroki
[Wprowadzenie do zasad niestandardowych](active-directory-b2c-get-started-custom.md).
