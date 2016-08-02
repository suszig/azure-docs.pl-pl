<properties
   pageTitle="Reagowanie na alerty zabezpieczeń i zarządzanie nimi w Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Ten dokument ułatwia zarządzanie alertami zabezpieczeń i reagowanie na nie przy użyciu funkcji Centrum zabezpieczeń Azure."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/22/2016"
   ms.author="yurid"/>

# Reagowanie na alerty zabezpieczeń i zarządzanie nimi w Centrum zabezpieczeń Azure
Ten dokument ułatwia zarządzanie alertami zabezpieczeń i reagowanie na nie przy użyciu funkcji Centrum zabezpieczeń Azure.

> [AZURE.NOTE] Podane tu informacje odnoszą się do wersji zapoznawczej Centrum zabezpieczeń Azure.

## Co to jest Centrum zabezpieczeń Azure?
 Centrum zabezpieczeń Azure ułatwia zapobieganie zagrożeniom, ich wykrywanie i reagowanie na nie, a przy tym zapewnia lepszy wgląd i większą kontrolę w zakresie bezpieczeństwa zasobów na platformie Azure. Umożliwia zintegrowane monitorowanie zabezpieczeń i zarządzanie zasadami dla wszystkich subskrypcji, pomaga wykrywać zagrożenia, które w przeciwnym razie mogłyby pozostać niezauważone, a także współpracuje z szerokim ekosystemem rozwiązań zabezpieczających.

## Czym są alerty zabezpieczeń?
Centrum zabezpieczeń automatycznie gromadzi, analizuje i integruje dane dzienników z zasobów platformy Azure, sieci oraz zintegrowanych programów chroniących przed złośliwym kodem i zapór, aby wykrywać prawdziwe zagrożenia i redukować liczbę fałszywych alarmów. W Centrum zabezpieczeń jest wyświetlana lista alertów zabezpieczeń uporządkowanych według priorytetu (między innymi alertów ze zintegrowanych rozwiązań partnerskich) oraz informacje potrzebne do szybkiego analizowania ataku i zalecenia dotyczące postępowania w danej sytuacji.

Pracownicy naukowo-badawczy firmy Microsoft zajmujący się zabezpieczeniami stale analizują zagrożenia pojawiające się na całym świecie, w tym nowe obserwowane wzorce i trendy ataków na produkty konsumenckie i korporacyjne oraz usługi online. W efekcie Centrum zabezpieczeń może aktualizować algorytmy wykrywania w miarę odnajdywania nowych luk w zabezpieczeniach i pojawiania się programów wykorzystujących takie luki. Dzięki temu klienci są na bieżąco chronieni przed rozwijającymi się zagrożeniami. Typy zagrożeń wykrywanych przez Centrum zabezpieczeń obejmują między innymi:

- **Wykrywanie ataków siłowych za pośrednictwem danych sieci**: ten typ zabezpieczeń stosuje modele uczenia maszynowego, które rozumieją wzorce ruchu w sieci typowe dla aplikacji, a także umożliwia skuteczniejsze wykrywanie prób uzyskania dostępu podejmowanych przez nieupoważnione osoby, a nie autoryzowanych użytkowników.
- **Wykrywanie ataków siłowych za pośrednictwem danych punktów końcowych**: typ zabezpieczeń oparty na analizie dzienników komputera, który umożliwia rozróżnianie między próbami udanymi i nieudanymi.
- **Maszyny wirtualne komunikujące się ze złośliwymi adresami IP**: ten typ zabezpieczeń porównuje ruch w sieci z informacjami wynikającymi z globalnej analizy zagrożeń przeprowadzanej przez firmę Microsoft, a także umożliwia odnajdowanie komputerów, których bezpieczeństwo zostało naruszone i które komunikują się z serwerami command and control (C&C).
- **Maszyny wirtualne, których bezpieczeństwo zostało naruszone**: typ zabezpieczeń oparty na analizie behawioralnej dzienników maszyn i korelacji z innymi sygnałami, który identyfikuje nietypowe zdarzenia prawdopodobnie związane z naruszeniem bezpieczeństwa i wykorzystaniem luk zabezpieczeń maszyny.

## Zarządzanie alertami zabezpieczeń

Bieżące alerty można przeglądać przy użyciu kafelka **Alerty zabezpieczeń**. Wykonaj poniższe kroki, aby wyświetlić więcej szczegółowych informacji dotyczących poszczególnych alertów:

1. Na pulpicie nawigacyjnym Centrum zabezpieczeń widoczny jest kafelek **Alerty zabezpieczeń**.

    ![Kafelek Alerty zabezpieczeń w Centrum zabezpieczeń Azure](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig1-new.png)

2.  Kliknij kafelek, aby otworzyć blok **Alerty zabezpieczeń** zawierający więcej szczegółowych informacji o alertach, jak pokazano poniżej.

    ![Blok Alerty zabezpieczeń w Centrum zabezpieczeń Azure](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig2-new.png)

W dolnej części tego bloku znajdują się szczegółowe informacje o każdym alercie. Aby posortować dane, kliknij kolumnę, według której chcesz wykonać sortowanie. Poniżej znajdują się definicje poszczególnych kolumn:

- **Alert**: krótki opis alertu.
- **Liczba**: lista wszystkich alertów określonego typu, które zostały wykryte w określonym dniu.
- **Wykryte przez**: usługa odpowiedzialna za wyzwolenie alertu.
- **Data**: dzień, w którym wystąpiło zdarzenie.
- **Stan**: bieżący stan alertu. Możliwe typy stanów to:
    - **Aktywny**: alert zabezpieczeń został wykryty.
    - **Odrzucony**: alert zabezpieczeń został odrzucony przez użytkownika. Ten stan jest zwykle używany w przypadku alertów, które zostały zbadane, ale ryzyko związane z nimi zostało zminimalizowane albo alert nie dotyczył rzeczywistego ataku.

- **Ważność**: poziom ważności (wysoki, średni lub niski).

Alerty można filtrować na podstawie daty, stanu i ważności. Filtrowanie alertów może być przydatne w przypadku scenariuszy, w których należy zawęzić zakres wyświetlanych alertów zabezpieczeń. Możesz na przykład sprawdzić alerty zabezpieczeń, które wystąpiły w ciągu ostatnich 24 godzin, ponieważ badasz potencjalne naruszenie zabezpieczeń systemu.

1. Kliknij pozycję **Filtr** w bloku **Alerty zabezpieczeń**. Zostanie otwarty blok **Filtr**. Wybierz wartości daty, stanu i ważności, które chcesz wyświetlić.

    ![Filtrowanie alertów w Centrum zabezpieczeń Azure](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig5-new.png)

2.  Po zbadaniu alertu zabezpieczeń może okazać się, że było to fałszywe ostrzeżenie związane ze środowiskiem lub że wskazuje on oczekiwane zachowanie określonego zasobu. Niezależnie od przyczyny wyświetlenia, jeśli uważasz, że alert zabezpieczeń nie ma zastosowania, możesz go odrzucić i odfiltrować z widoku. Istnieją dwa sposoby odrzucania alertu zabezpieczeń. Kliknij alert prawym przyciskiem myszy i wybierz pozycję **Odrzuć** lub umieść wskaźnik myszy nad elementem, kliknij przycisk z wielokropkiem po prawej stronie i wybierz pozycję **Odrzuć**. Odrzucone alerty zabezpieczeń możesz wyświetlić, klikając pozycję **Filtr** i wybierając opcję **Odrzucone**.

    ![Filtrowanie alertów w Centrum zabezpieczeń Azure](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig6-new.png)

### Odpowiadanie na alerty zabezpieczeń
Wybierz alert zabezpieczeń, aby dowiedzieć się więcej o zdarzeniach, które go wywołały, oraz o czynnościach, które należy wykonać w celu wyeliminowania skutków ataku (jeśli ma to zastosowanie). Alerty zabezpieczeń są grupowane według typu i daty. Kliknięcie alertu zabezpieczeń spowoduje otwarcie bloku zawierającego listę pogrupowanych alertów.

![Odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig7.png)

W tym przypadku wyzwolone alerty dotyczą podejrzanego działania protokołu RDP (Remote Desktop Protocol). Pierwsza kolumna zawiera zaatakowane zasoby, drugi przedstawia czas wykrycia ataku, trzecia — stan alertu, a czwarta — ważność ataku. Po przejrzeniu tych informacji kliknij zaatakowany zasób. Zostanie otwarty nowy blok zawierający sugestie dotyczące dalszych kroków, jak pokazano w poniższym przykładzie.

![Sugestie dotyczące zalecanych działań związanych z alertami zabezpieczeń w Centrum zabezpieczeń Azure](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig8-1.png)

W polu **Alert** tego bloku znajdują się dalsze szczegółowe informacje na temat danego zdarzenia. Te dodatkowe szczegóły dotyczą przyczyn wyzwolenia alertu zabezpieczeń, zasobu docelowego, źródłowego adresu IP (jeśli ma to zastosowanie) i zaleceń dotyczących sposobu wyeliminowania skutków ataku.  W niektórych przypadkach źródłowy adres IP będzie pusty (niedostępny), ponieważ nie wszystkie dzienniki zdarzeń zabezpieczeń systemu Windows obejmują adres IP.

> [AZURE.NOTE] Czynności naprawcze sugerowane w Centrum zabezpieczeń będą różnić się w zależności od alertu zabezpieczeń. W niektórych przypadkach do wykonania zalecanych czynności naprawczych trzeba będzie użyć innych funkcji platformy Azure. Na przykład aby wyeliminować skutki takiego ataku, należy utworzyć listę niedozwolonych adresów IP generujących atak przy użyciu [sieciowej listy ACL](../virtual-network/virtual-networks-acl.md) lub reguły [sieciowej grupy zabezpieczeń](../virtual-network/virtual-networks-nsg.md).


## Następne kroki
W tym dokumencie przedstawiono sposób konfigurowania zasad zabezpieczeń w Centrum zabezpieczeń. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

- [Monitorowanie kondycji zabezpieczeń w Centrum zabezpieczeń Azure](security-center-monitoring.md) — informacje na temat monitorowania kondycji zasobów platformy Azure.
- [Monitorowanie rozwiązań partnerskich w Centrum zabezpieczeń Azure](security-center-partner-solutions.md) — informacje na temat monitorowania stanu kondycji rozwiązań partnerskich.
- [Centrum zabezpieczeń Azure — często zadawane pytania](security-center-faq.md) — odpowiedzi na najczęstsze pytania dotyczące korzystania z usługi.
- [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) — wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure.



<!--HONumber=Jun16_HO2-->


