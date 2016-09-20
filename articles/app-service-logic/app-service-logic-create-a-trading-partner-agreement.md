<properties 
   pageTitle="Tworzenie umowy z partnerem handlowym w usłudze Azure App Service | Microsoft Azure" 
   description="Tworzenie umów z partnerami handlowymi" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
    ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="08/23/2016"
   ms.author="rajram"/>

# Tworzenie umowy z partnerem handlowym   

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Partnerzy handlowi to jednostki biorące udział w komunikacji B2B (Business-to-Business). Relacja ustanowiona między dwoma partnerami jest nazywana *umową*. Zdefiniowana umowa jest oparta na komunikacji, którą chcą nawiązać dwie jednostki partnerskie. Umowa odnosi się do danego protokołu lub transportu. Różne metody transportu i protokoły B2B obsługiwane przez usługę Azure App Service obejmują:

- AS2 (Applicability Statement 2)
- EDIFACT (United Nations/Electronic Data Interchange For Administration, Commerce and Transport (UN/EDIFACT))
- X12 (ASC X12)

### Aplikacje API Apps w BizTalk, które obsługują scenariusze B2B
Poniższe aplikacje API Apps oferują wymienione możliwości w zaawansowanym i intuicyjnym środowisku pracy w witrynie Azure Portal:


## Zarządzanie partnerami handlowymi BizTalk (TPM, Trading Partner Management)
- Tworzenie partnerów, profilów i tożsamości oraz zarządzanie nimi
- Przechowywanie schematów EDI i zarządzanie nimi
- Przechowywanie certyfikatów (używanych w protokole AS2) i zarządzanie nimi
- Tworzenie umów AS2 i zarządzanie nimi
- Tworzenie umów EDIFACT (w tym przetwarzanie wsadowe stronie wysyłania) i zarządzanie nimi
- Tworzenie umów X12 (w tym przetwarzanie wsadowe po stronie wysyłania) i zarządzanie nimi

![][1]


## Łącznik AS2
- Wykonuje umowy AS2 zgodnie z definicją w powiązanym wystąpieniu aplikacji interfejsu API TPM.
- Wydobywa informacje dotyczące przetwarzania/śledzenia informacji AS2 na potrzeby rozwiązywania problemów.


## BizTalk EDIFACT
- Wykonuje umowy EDIFACT zgodnie z definicją w powiązanym wystąpieniu aplikacji interfejsu API TPM.
- Wydobywa informacje dotyczące przetwarzania/śledzenia informacji EDIFACT na potrzeby rozwiązywania problemów.
- Umożliwia zarządzanie stanem partii zadań (rozpoczęcie i zakończenie) zgodnie z definicją w umowach EDIFACT w powiązanym wystąpieniu aplikacji interfejsu API TPM.


## BizTalk X12
- Wykonuje umowy X12 zgodnie z definicją w powiązanym wystąpieniu aplikacji interfejsu API TPM. 
- Wydobywa informacje dotyczące przetwarzania/śledzenia informacji X12 na potrzeby rozwiązywania problemów.
- Umożliwia zarządzanie stanem partii zadań (rozpoczęcie i zakończenie) zgodnie z definicją w umowach X12 w powiązanym wystąpieniu aplikacji interfejsu API TPM.

Jak podano wcześniej, aby aplikacje API Apps AS2, X12 i EDIFACT działały zgodnie z oczekiwaniami, wymagają aplikacji interfejsu API TPM.


## Wprowadzenie
Aby tworzyć umowy z partnerami handlowymi:

1. Utwórz wystąpienie łącznika **Zarządzanie partnerami handlowymi BizTalk**. Do prawidłowego działania wymaga on pustej bazy danych SQL Database. Przed rozpoczęciem upewnij się, że pusta baza danych jest dostępna i gotowa do użycia.
2. Przekaż schematy i certyfikaty wymagane przez umowy. W tym celu przejdź do utworzonego wystąpienia łącznika TPM, a następnie do części „Schematy” i/lub „Certyfikaty”.
3. Przejdź do utworzonego wystąpienia łącznika TPM, a następnie do części **Partnerzy**.
4. Utwórz partnerów zgodnie z potrzebami. Możesz również w razie potrzeby edytować profile i dodawać wymagane tożsamości.
5. Teraz utwórz umowy, korzystając z części **Umowy**. Podczas tworzenia umowy musisz wybrać protokół do użycia. Pozostałe opcje konfiguracji zależą od wybranego protokołu.

![][2]

![][3]

<!--Image references-->
[1]: ./media/app-service-logic-create-a-trading-partner-agreement/TPMResourceView.png
[2]: ./media/app-service-logic-create-a-trading-partner-agreement/ProtocolSelection.png
[3]: ./media/app-service-logic-create-a-trading-partner-agreement/X12AgreementCreation.png
 



<!--HONumber=sep16_HO1-->


