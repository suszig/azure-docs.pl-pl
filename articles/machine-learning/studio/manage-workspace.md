---
title: "Zarządzanie obszaru roboczego usługi Machine Learning | Dokumentacja firmy Microsoft"
description: "Zarządzanie dostępem do usługi Azure Machine Learning obszary robocze oraz wdrażania i zarządzania nimi usług sieci web interfejsu API uczenia Maszynowego"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: daf3d413-7a77-4beb-9a7a-6b4bdf717719
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: garye
ms.openlocfilehash: 2f90234bdd5c917a502d24cd16256bc11c7fbed0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-an-azure-machine-learning-workspace"></a>Zarządzanie obszarem roboczym usługi Azure Machine Learning

> [!NOTE]
> Informacje na temat zarządzania usługami sieci Web w portalu usługi sieci Web usługi Machine Learning, zobacz [zarządzania usługi sieci Web przy użyciu portalu usługi sieci Web systemu Azure Machine Learning](manage-new-webservice.md).
> 
> 

Możesz zarządzać obszarów roboczych uczenia maszynowego w portalu Azure lub klasycznego portalu Azure.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="use-the-azure-portal"></a>Korzystanie z witryny Azure Portal

Aby zarządzać obszaru roboczego w portalu Azure:

1. Zaloguj się do [portalu Azure](https://portal.azure.com/) przy użyciu konta administratora subskrypcji platformy Azure.
2. W polu wyszukiwania w górnej części strony wprowadź "komputera learning obszary robocze", a następnie wybierz **obszarów roboczych Machine Learning**.
3. Kliknij obszar roboczy, którą chcesz zarządzać.

Oprócz informacji o zarządzaniu standardowych zasobów i dostępne opcje można:

- Widok **właściwości** — ta strona zawiera informacje dotyczące obszaru roboczego i zasobów, a można zmienić ten obszar roboczy jest połączony z grupą subskrypcji i zasobu.
- **Ponownej synchronizacji magazynu kluczy** -obszar roboczy obsługuje kluczy do konta magazynu. Jeśli zmienia się na koncie magazynu kluczy, a następnie kliknięcie **ponownej synchronizacji kluczy** zsynchronizować klucze z obszaru roboczego.

Aby zarządzać usługami sieci web skojarzony z tym obszarem roboczym, należy korzystać z portalu usługi sieci Web usługi Machine Learning. Zobacz [zarządzania usługi sieci Web przy użyciu portalu usługi sieci Web systemu Azure Machine Learning](manage-new-webservice.md) pełne informacje.

> [!NOTE]
> Aby wdrożyć lub zarządzać nowych usług sieci web musi mieć przypisaną rolę współautora lub administrator w subskrypcji, w której wdrażana jest usługa sieci web. Jeśli musisz poprosić innego użytkownika do obszaru roboczego uczenia maszynowego, należy je przypisać do roli współautora lub administratora dla subskrypcji przed wdrożeniem lub zarządzania usługami sieci web. 
> 
>Aby uzyskać więcej informacji o ustawianiu uprawnień dostępu, zobacz [Wyświetl przypisania dostępu dla użytkowników i grup w portalu Azure - publicznej wersji zapoznawczej](../../active-directory/role-based-access-control-manage-assignments.md).

## <a name="use-the-azure-classic-portal"></a>Użyj klasycznego portalu Azure

Przy użyciu klasycznego portalu Azure, można zarządzać do uczenia maszynowego obszarów roboczych:

* Monitorowanie, sposobu używania obszaru roboczego
* Konfiguruj obszar roboczy, aby zezwolić lub odmówić dostępu
* Zarządzanie usługami sieci Web utworzona w obszarze roboczym
* Usuwanie obszaru roboczego

Ponadto karty Pulpit nawigacyjny zawiera omówienie użycie obszaru roboczego i szybkiego dostępu informacji obszaru roboczego.  

> [!TIP]
> W usłudze Azure Machine Learning Studio na **usług sieci WEB** karcie, można dodać, zaktualizować lub usunąć usługi Machine Learning w sieci Web.
> 
> 

Aby zarządzać obszaru roboczego w klasycznym portalu Azure:

1. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com/) konta przy użyciu platformy Microsoft Azure — należy użyć konta, na którym jest skojarzony z subskrypcją platformy Azure.
2. W panelu usługi Microsoft Azure, kliknij przycisk **UCZENIA MASZYNOWEGO**.
3. Kliknij obszar roboczy, którą chcesz zarządzać.

Strona obszaru roboczego zawiera trzy karty:

* **Pulpit NAWIGACYJNY** — pozwala na wyświetlanie obszaru roboczego użycia i informacji
* **Konfiguruj** — umożliwia zarządzanie dostępem do obszaru roboczego
* **USŁUGI sieci WEB** — umożliwia zarządzanie usługami sieci Web, które zostały opublikowane z tego obszaru roboczego

### <a name="to-monitor-how-the-workspace-is-being-used"></a>Aby monitorować sposobu używania obszaru roboczego
Kliknij przycisk **pulpitu NAWIGACYJNEGO** kartę.

Na pulpicie nawigacyjnym możesz wyświetlić ogólne użycie obszaru roboczego i uzyskać szybkiego dostępu informacji obszaru roboczego.

* **OBLICZENIOWE** wykres zawiera zasoby obliczeniowe, używany przez obszar roboczy. Można zmienić widok, aby wyświetlić względną lub bezwzględną wartości, a można zmienić zakres czasu wyświetlane na wykresie.
* **Przegląd wykorzystania** Wyświetla magazynu Azure używany przez obszar roboczy.
* **Szybkiego dostępu** zawiera podsumowanie informacji obszaru roboczego i przydatne łącza.

> [!NOTE]
> **Zalogować się do ML Studio** łącze powoduje otwarcie przy użyciu Account Microsoft jest obecnie zarejestrowany w usłudze Machine Learning Studio. Account firmy Microsoft, używanego do logowania się do klasycznego portalu Azure do utworzenia obszaru roboczego nie ma automatycznie uprawnień do otwierania tego obszaru roboczego. Aby otworzyć obszar roboczy, użytkownik musi zalogować się Account firmy Microsoft, która została zdefiniowana jako właściciela obszaru roboczego lub konieczność odbierania zaproszenia od właściciela do obszaru roboczego.
> 
> 

### <a name="to-grant-or-suspend-access-for-users"></a>Aby przydzielić lub zawiesić dostęp dla użytkowników
Kliknij przycisk **Konfiguruj** kartę.

Na karcie Konfiguracja można:

* Zawiesić dostęp do obszaru roboczego uczenia maszynowego, klikając przycisk ODMÓW. Użytkownicy nie będą już mogli otworzyć obszar roboczy w usłudze Machine Learning Studio. Aby przywrócić dostęp, kliknij przycisk Zezwalaj.

Aby zarządzać dodatkowych kont, którzy mają dostęp do obszaru roboczego usługi Machine Learning Studio, kliknij przycisk **zalogować się do ML Studio** w **pulpitu NAWIGACYJNEGO** kartę (patrz Uwaga poprzedzających dotyczące **zalogować się do ML Studio**). Spowoduje to otwarcie obszaru roboczego usługi Machine Learning Studio. W tym miejscu, kliknij przycisk **ustawienia** kartę, a następnie **użytkowników**. Możesz kliknąć **ZAPROSIĆ użytkowników więcej** udzielać użytkownikom dostępu do obszaru roboczego, lub wybierz użytkownika i kliknij przycisk **Usuń**.

### <a name="to-manage-web-services-in-this-workspace"></a>Do zarządzania usługami sieci web, w tym obszarze roboczym
Kliknij przycisk **usług sieci WEB** kartę.

Spowoduje to wyświetlenie listy publikowane z tym obszarem roboczym usługi sieci web.
Aby zarządzać usługą sieci web, kliknij nazwę na liście, aby otworzyć stronę usługi sieci Web.

Usługi sieci Web może mieć co najmniej jeden zdefiniowanych punktów końcowych.

* Można zdefiniować więcej punktów końcowych oprócz punktu końcowego "Domyślny". Aby dodać punkt końcowy, kliknij przycisk **zarządzanie punktami końcowymi** w dolnej części pulpitu nawigacyjnego, aby otworzyć portal usługi sieci Web systemu Azure Machine Learning.
* Aby usunąć punkt końcowy (nie można usunąć punktu końcowego "Domyślne"), kliknij pole wyboru na początku wiersza punktu końcowego, a następnie kliknij przycisk **usunąć**. Spowoduje to usunięcie punkt końcowy usługi sieci Web.
  
  > [!NOTE]
  > Jeśli aplikacja używa punkt końcowy usługi sieci web po usunięciu punktu końcowego, aplikacji spowoduje wystąpienie błędu podczas kolejnej próby uzyskania dostępu do usługi.
  > 
  > 

Kliknij nazwę punktu końcowego usługi sieci Web, aby go otworzyć. 

Z poziomu pulpitu nawigacyjnego można wyświetlić ogólne użycie usługi sieci Web w danym okresie czasu. Można wybrać okres, aby wyświetlić z menu rozwijanego okresu, w prawym górnym rogu wykresy użycia. Pulpit nawigacyjny zawiera następujące informacje:

* **Żądań w czasie** wykres krok to liczba żądań przez wybrany okres czasu. Może pomóc ustalić, czy występują wzrostów użycia.
* **Żądanie-odpowiedź żądań** Wyświetla całkowitą liczbę wywołań żądań i odpowiedzi usługa odebrała przez wybrany okres czasu i ile z nich nie powiodło się.
* **Średni czas obliczeniowe żądanie-odpowiedź** wyświetla średni czas potrzebny na wykonanie odebranych żądań.
* **Partie żądania** Wyświetla całkowitą liczbę żądań wsadowych usługa odebrała przez wybrany okres czasu i ile z nich nie powiodło się.
* **Średni czas oczekiwania zadania** wyświetla średni czas potrzebny na wykonanie odebranych żądań.
* **Błędy** Wyświetla łączna liczba błędów, które wystąpiły na połączenia z usługą sieci web.
* **Koszty usługi** Wyświetla opłat za plan rozliczeniowy skojarzony z usługą.

Na stronie Konfiguracja można aktualizować następujące właściwości:

* **Opis elementu** umożliwia wprowadzenie opisu usługi sieci Web. Opis jest polem wymaganym.
* **Rejestrowanie** umożliwia włączanie lub wyłączanie rejestrowania w punkcie końcowym błędów. Aby uzyskać więcej informacji, zobacz Włącz [rejestrowania dla usług sieci web uczenie maszynowe](web-services-logging.md).
* **Włącz przykładowe dane** umożliwia podanie przykładowych danych, które służy do testowania usługi żądań i odpowiedzi. Jeśli utworzono usługę sieci web w usłudze Machine Learning Studio przykładowych danych jest pobierana z danych użytkownika używanych do uczenia modelu. Jeśli usługi zostały utworzone programowo, dane są pobierane z przykładowe dane podane jako część pakietu JSON.

