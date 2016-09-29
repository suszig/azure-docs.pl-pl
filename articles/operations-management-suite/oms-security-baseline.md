<properties
   pageTitle="Ocena linii bazowej w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie Operations Management Suite | Microsoft Azure"
   description="Ten dokument wyjaśnia, jak korzystać z rozwiązania Zabezpieczenia i inspekcja w pakiecie OMS w celu oceny linii bazowej wszystkich monitorowanych komputerów pod kątem zgodności i zabezpieczeń."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/08/2016"
   ms.author="yurid"/>


# Ocena linii bazowej w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie Operations Management Suite

Ten dokument ułatwia korzystanie z funkcji oceny linii bazowej w [rozwiązaniu Zabezpieczenia i inspekcja w pakiecie Operations Management Suite (OMS)](operations-management-suite-overview.md) w celu zapewnienia bezpieczeństwa monitorowanych zasobów.

## Co to jest ocena linii bazowej?

Firma Microsoft, wraz z instytucjami branżowymi i rządowymi na całym świecie, zdefiniowała konfigurację systemu Windows, która reprezentuje wdrożenia serwera o wysokim poziomie zabezpieczeń. Konfiguracja ta stanowi zestaw kluczy rejestru, ustawień zasad inspekcji i ustawień zasad zabezpieczeń wraz z zalecanymi przez firmę Microsoft wartościami tych ustawień. Ten zestaw reguł jest określany jako linia bazowa zabezpieczeń. Ocena linii bazowej w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie OMS umożliwia bezproblemowe skanowanie wszystkich komputerów pod kątem zgodności. 

Możliwe typy reguł to:

- **Reguły rejestru**: sprawdzenie, czy klucze rejestru są prawidłowo ustawione.
- **Reguły zasad inspekcji**: reguły dotyczące zasad inspekcji.
- **Reguły zasad zabezpieczeń**: reguły dotyczące uprawnień użytkownika na komputerze.

> [AZURE.NOTE] Krótkie omówienie tej funkcji zawiera artykuł [Dostęp do linii bazowej konfiguracji zabezpieczeń w rozwiązaniu Zabezpieczenia w pakiecie OMS](https://blogs.technet.microsoft.com/msoms/2016/08/12/use-oms-security-to-assess-the-security-configuration-baseline/).

## Ocena linii bazowej zabezpieczeń

Korzystając z pulpitu nawigacyjnego, można przeglądać bieżące oceny linii bazowej zabezpieczeń dla wszystkich komputerów, które są monitorowane przy użyciu rozwiązania Zabezpieczenia i inspekcja w pakiecie OMS.  Aby przejść do pulpitu nawigacyjnego oceny linii bazowej zabezpieczeń, wykonaj te kroki:

1. Na głównym pulpicie nawigacyjnym **pakietu Microsoft Operations Management Suite** kliknij kafelek **Zabezpieczenia i inspekcja**.
2. Na pulpicie nawigacyjnym **Zabezpieczenia i inspekcja** kliknij przycisk **Ocena bazowa** w obszarze **Domeny zabezpieczeń**. Pojawi się pulpit nawigacyjny **Ocena linii bazowej zabezpieczeń**, jak pokazano na poniższej ilustracji:
    
    ![Ocena linii bazowej w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie OMS](./media/oms-security-baseline/oms-security-baseline-fig1.png)

Ten pulpit nawigacyjny dzieli się na trzy główne obszary:

- **Komputery w porównaniu z wartościami bazowymi**: ta część zawiera podsumowanie liczby przeanalizowanych komputerów i odsetka komputerów, które przeszły ocenę pomyślnie. Widać też 10 najlepszych komputerów i wynik procentowy oceny.
- **Stan wymaganych reguł**: ta sekcja ma zwracać uwagę na niespełnione reguły, klasyfikując je według ważności i typu. Spoglądając na pierwszy wykres, można szybko ustalić, czy większość reguł z błędami ma znaczenie krytyczne, czy nie. Widać też listę 10 najczęściej występujących niespełnionych reguł oraz ich ważności. Drugi wykres przedstawia typy reguł, w których wykryto błędy podczas oceny. 
- **Komputery, dla których brak oceny linii bazowej**: w tej części widać komputery, do których nie uzyskano dostępu z powodu błędów lub niezgodności systemu operacyjnego. 

### Uzyskiwanie dostępu do komputerów w porównaniu do linii bazowej

W idealnym przypadku wszystkie komputery powinny pomyślnie przejść ocenę linii bazowej zabezpieczeń. Jednak można się spodziewać, że w pewnych okolicznościach tak nie będzie. W ramach procesu zarządzania zabezpieczeniami ważne jest dokonanie przeglądu komputerów, które nie przeszły pomyślnie wszystkich testów oceny zabezpieczeń. Aby szybko wyświetlić wizualizację tych danych, można wybrać opcję **Komputery, do których uzyskano dostęp** w sekcji **Komputery w porównaniu z wartościami bazowymi**. Powinny zostać wyświetlone wyniki wyszukiwania dziennika z listą komputerów, jak pokazano na poniższym ekranie:

![Wyniki wyszukiwania komputerów, do których uzyskano dostęp](./media/oms-security-baseline/oms-security-baseline-fig2.png)

Wyniki wyszukiwania mają postać tabeli, której pierwsza kolumna zawiera nazwę komputera, a druga — liczbę reguł z błędami. Aby uzyskać informacje dotyczące typów niespełnionych reguł, kliknij liczbę reguł z błędami obok nazwy komputera. Powinien pojawić się wynik podobny do przedstawionego na poniższej ilustracji:

![Szczegółowe wyniki wyszukiwania komputerów, do których uzyskano dostęp](./media/oms-security-baseline/oms-security-baseline-fig3.png)

Te wyniki wyszukiwania zawierają łączną liczbę reguł, do których uzyskano dostęp, oraz liczbę niespełnionych reguł krytycznych, reguł z ostrzeżeniami i niespełnionych reguł dotyczących informacji.

### Uzyskiwanie dostępu do stanu wymaganych reguł

Po uzyskaniu informacji dotyczących procentowej liczby komputerów, które z powodzeniem przeszły ocenę, można uzyskać więcej informacji o niespełnionych regułach według ich ważności. Taka wizualizacja pomaga ustalić, którymi komputerami należy zająć się w pierwszej kolejności, aby przeszły one pomyślnie następną ocenę. Na kafelku **Niespełnione reguły według ważności** w obszarze **Stan wymaganych reguł** umieść kursor nad częścią wykresu Krytyczne i kliknij ją. Powinny pojawić się wyniki podobne do następujących:

![Niespełnione reguły według ważności — szczegóły](./media/oms-security-baseline/oms-security-baseline-fig4.png) 

W tym wyniku dziennika widać typ niespełnionej reguły linii bazowej, jej opis oraz identyfikator CCE. Te informacje powinny wystarczyć do naprawienia problemu na komputerze docelowym.

> [AZURE.NOTE] Więcej informacji na temat identyfikatorów CCE zawiera witryna [National Vulnerability Database](https://nvd.nist.gov/cce/index.cfm).

### Dostęp do komputerów, dla których brak oceny linii bazowej

Pakiet OMS obsługuje profil linii bazowej członka domeny w systemie Windows Server od wersji 2008 R2 do wersji 2012 R2. Ostateczna wersja linii bazowej dla systemu Windows Server 2016 nie jest jeszcze opracowana i zostanie dodana natychmiast po jej opublikowaniu. Wszystkie inne systemy operacyjne skanowane za pomocą oceny linii bazowej w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie OMS są widoczne w obszarze **Komputery, dla których brak oceny linii bazowej**.

## Zobacz też

Ten dokument przedstawia informacje na temat oceny linii bazowej w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie OMS. Więcej informacji na temat zabezpieczeń w pakiecie OMS zawierają następujące artykuły:

- [Omówienie pakietu Operations Management Suite (OMS)](operations-management-suite-overview.md)
- [Monitorowanie alertów zabezpieczeń i reagowanie na nie w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie Operations Management Suite](oms-security-responding-alerts.md)
- [Monitorowanie zasobów w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie Operations Management Suite](oms-security-monitoring-resources.md)




<!--HONumber=Sep16_HO3-->


