<properties
   pageTitle="Wprowadzenie do usługi Microsoft Power BI Embedded Preview"
   description="Usługa Power BI Embedded — dodawanie interaktywnych raportów usługi Power BI do aplikacji analizy biznesowej"
   services="power-bi-embedded"
   documentationCenter=""
   authors="dvana"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="04/24/2016"
   ms.author="derrickv"/>

# Wprowadzenie do usługi Microsoft Power BI Embedded Preview

**Microsoft Power BI Embedded** to usługa Azure, która umożliwia programistom dodawanie interaktywnych raportów usługi Power BI do własnych aplikacji. **Power BI Embedded** współpracuje z istniejącymi aplikacjami bez konieczności ponownego projektowania tych aplikacji ani zmieniania sposobu logowania ich użytkowników.

Aby dowiedzieć się więcej o usłudze Power BI Embedded, zobacz temat [What is Power BI Embedded](power-bi-embedded-what-is-power-bi-embedded.md) (Co to jest usługa Power BI Embedded).

Zgodnie z opisem w temacie [What is Power BI Embedded](power-bi-embedded-what-is-power-bi-embedded.md) (Co to jest usługa Power BI Embedded) zasoby są dostarczane do usługi **Microsoft Power BI Embedded** przez interfejsy [API Azure ARM](https://msdn.microsoft.com/library/mt712306.aspx). W tym przypadku dostarczanym zasobem jest **kolekcja obszarów roboczych usługi Power BI**. W następnej części pokazano, jak utworzyć kolekcję obszarów roboczych.

![](media\power-bi-embedded-get-started\introduction.png)

## Tworzenie kolekcji obszarów roboczych
**Kolekcja obszarów roboczych** jest najwyższego poziomu zasobem platformy Azure i kontenerem zawartości, która zostanie osadzona w aplikacji. **Kolekcję obszarów roboczych** można utworzyć na dwa sposoby:

   -    Ręcznie przy użyciu Portalu Azure
   -    Programowo przy użyciu interfejsów API Azure Resource Manager (ARM)

Przejdźmy do kroków tworzenia **kolekcji obszarów roboczych** przy użyciu Portalu Azure.

   1.   Otwórz **Portal Azure**: [http://portal.azure.com](http://portal.azure.com) i zaloguj się.

   2.   Kliknij przycisk **+ Nowy** na górnym panelu.

       ![](media\power-bi-embedded-get-started\create-workspace-1.png)

   3.   W obszarze **Dane i analizy** kliknij opcję **Power BI Embedded**.
   4.   W **bloku tworzenia** wprowadź wymagane informacje. Aby poznać **ceny**, zobacz [Usługa Power BI Embedded — cennik](http://go.microsoft.com/fwlink/?LinkID=760527).

       ![](media\power-bi-embedded-get-started\create-workspace-2.png)

   5. Kliknij przycisk **Utwórz**.

Dostarczenie  **kolekcji obszarów roboczych** potrwa parę chwil. Po jego ukończeniu nastąpi przekierowanie do **bloku kolekcji obszarów roboczych**.

   ![](media\power-bi-embedded-get-started\create-workspace-3.png)

Ten **blok tworzenia** zawiera informacje potrzebne do wywoływania interfejsów API służących do tworzenia obszarów roboczych i wdrażania do nich zawartości.

W następnej części opisano sposób używania **kluczy dostępu**  do generowania **tokenów aplikacji** służących do uwierzytelniania żądań interfejsu API.

<a name="view-access-keys"/>
## Wyświetlanie kluczy dostępu interfejsu API usługi Power BI

Jedną z najważniejszych informacji potrzebnych do wywoływania interfejsów API REST usługi Power BI są **klucze dostępu**. Są one używane do generowania **tokenów aplikacji** służących do uwierzytelniania żądań interfejsu API. Aby wyświetlić **klucze dostępu**, kliknij przycisk **Klucze dostępu** w **bloku Ustawienia**. Aby uzyskać więcej informacji na temat **tokenów aplikacji**, zobacz [How does app token flow work](power-bi-embedded-app-token-flow.md) (Działanie przepływu pracy tokenu aplikacji).

   ![](media\power-bi-embedded-get-started\access-keys.png)

Możesz zauważyć, że istnieją dwa klucze.

   ![](media\power-bi-embedded-get-started\access-keys-2.png)

Skopiuj te klucze i przechowuj je bezpiecznie w aplikacji. Bardzo ważne jest, aby traktować te klucze jak hasła, ponieważ zapewniają one dostęp do całej zawartości **kolekcji obszarów roboczych**.

Chociaż wyświetlane są dwa klucze, aktualnie potrzebny jest tylko jeden. Drugi klucz jest dostarczany, aby można było okresowo generować ponownie klucze bez przerywania dostępu do usługi.

Teraz, gdy masz wystąpienie usługi Power BI dla danej aplikacji oraz **klucze dostępu**, możesz zaimportować raport do aplikacji. Przed zapoznaniem się z instrukcją importowania raportu przeczytaj informacje o tworzeniu zestawów danych i raportów usługi Power BI w celu osadzenia w aplikacji zawarte w następnej części.

## Tworzenie zestawów danych i raportów usługi Power BI w celu osadzenia w aplikacji

Teraz, gdy masz utworzone wystąpienie usługi Power BI dla aplikacji i **klucze dostępu**, trzeba utworzyć zestawy danych i raporty usługi Power BI potrzebne do osadzenia. Zestawy danych i raporty można tworzyć za pomocą programu **Power BI Desktop**. Możesz pobrać program [Power BI Desktop za darmo](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/). Możesz też pobrać przykładowy plik [Retail Analysis Sample PBIX] (http://go.microsoft.com/fwlink/?LinkID=780547), aby szybko rozpocząć pracę. Aby dowiedzieć się więcej o sposobie używania programu **Power BI Desktop**, zobacz stronę [Getting Started With Power BI Desktop](https://powerbi.microsoft.com/en-us/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop) (Wprowadzenie do programu Power BI Desktop).

W programie **Power BI Desktop** łączysz się ze źródłem danych, importując kopię danych do programu **Power BI Desktop**, lub korzystasz bezpośrednio ze źródła danych przy użyciu **trybu DirectQuery**.

Poniżej przedstawiono różnice między **importem** a **trybem DirectQuery**.

|Import | Tryb DirectQuery
|---|---
|Tabele, kolumny, *i dane* są importowane lub kopiowane do programu **Power BI Desktop**. Podczas pracy nad wizualizacjami program **Power BI Desktop** wykonuje zapytania na kopii danych. Aby zobaczyć wszelkie zmiany, które nastąpiły w danych źródłowych, trzeba odświeżyć lub ponownie zaimportować pełny bieżący zestaw danych.|Tylko *tabele i kolumny* są importowane lub kopiowane do programu **Power BI Desktop**. Podczas pracy nad wizualizacjami program **Power BI Desktop** wysyła zapytania do oryginalnego źródła danych, więc widoczne dane są zawsze aktualne.

Aby poznać szczegółowe informacje na temat nawiązywania połączenia ze źródłem danych, zobacz temat [Nawiązywanie połączenia ze źródłem danych](power-bi-embedded-connect-datasource.md).

Po zapisaniu pracy w programie **Power BI Desktop** tworzony jest plik PBIX. Ten plik zawiera raport. Ponadto w przypadku importowania danych plik PBIX zawiera pełen zestaw danych. Jeśli natomiast używasz **trybu DirectQuery**, plik PBIX zawiera tylko schemat zestawu danych. Programowe wdrażanie pliku PBIX do obszaru roboczego przy użyciu interfejsu [API Import usługi Power BI](https://msdn.microsoft.com/library/mt711504.aspx).

> [AZURE.NOTE] **Power BI Embedded** ma dodatkowe interfejsy API, które pozwalają zmienić serwer i bazę danych wskazywane przez zestaw danych, a także ustawić poświadczenia konta usługi używane przez zestaw danych do łączenia się z bazą danych. Zobacz artykuły dotyczące operacji [Post SetAllConnections](https://msdn.microsoft.com/library/mt711505.aspx) i [Patch Gateway Datasource](https://msdn.microsoft.com/library/mt711498.aspx).

## Następne kroki
W poprzednich krokach utworzono kolekcję obszarów roboczych oraz pierwszy raport i zestaw danych. Teraz nadszedł czas, aby dowiedzieć się, jak pisać kod **Power BI Embedded**. Aby pomóc Ci rozpocząć, utworzyliśmy przykładową aplikację sieci Web: [Rozpoczęcie pracy z przykładem](power-bi-embedded-get-started-sample.md). Przykład obejmuje:

  - Udostępnianie zawartości
      - Tworzenie obszaru roboczego
      - Importowanie pliku PBIX
      - Aktualizowanie parametrów połączenia i ustawianie poświadczeń dla zestawów danych.

  - Bezpieczne osadzanie raportu

## Zobacz też
- [Rozpoczęcie pracy z przykładem](power-bi-embedded-get-started-sample.md)
- [Co to jest usługa Power BI Embedded](power-bi-embedded-what-is-power-bi-embedded.md)
- [Wprowadzenie do programu Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started)
- [Program Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)
- [Usługa Power BI Embedded — cennik](http://go.microsoft.com/fwlink/?LinkID=760527)



<!--HONumber=Jun16_HO2-->


