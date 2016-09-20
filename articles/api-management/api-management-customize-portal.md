<properties
    pageTitle="Dostosowywanie portalu dla deweloperów w usłudze Azure API Management | Microsoft Azure"
    description="Dowiedz się, jak dostosować portal dla deweloperów w usłudze Azure API Management."
    services="api-management"
    documentationCenter=""
    authors="steved0x"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/24/2016"
    ms.author="sdanie"/>

# Dostosowywanie portalu dla deweloperów w usłudze Azure API Management

W tym przewodniku przedstawiono sposób modyfikowania wyglądu i działania portalu dla deweloperów w usłudze Azure API Management w celu zachowania spójności z własną marką.

## <a name="change-page-headers"> </a>Zmiana tekstu lub logo w nagłówku strony

Jednym z kluczowych aspektów dostosowywania portalu jest zastąpienie tekstu w górnej części wszystkich stron nazwą lub logo firmy.

Zawartość portalu dla deweloperów jest modyfikowana przy użyciu portalu wydawcy, który jest dostępny za pośrednictwem klasycznego portalu Azure. Aby uzyskać dostęp do portalu wydawcy interfejsu API, kliknij przycisk **Zarządzaj** w klasycznym portalu Azure dla usługi API Management.

![Portal wydawcy][api-management-management-console]

Portal dla deweloperów jest oparty na systemie zarządzania zawartością, czyli systemie CMS. Nagłówek, który pojawia się na każdej stronie, to specjalny typ zawartości, tzw. widżet. Aby edytować zawartość tego widżetu, kliknij pozycję **Widżety** w menu **Portal dla deweloperów**, a następnie wybierz z listy widżet **Nagłówek**.

![Widżety — nagłówek][api-management-widgets-header]

Zawartość nagłówka można edytować w polu **Treść**. Zmień tekst na „Portal dla deweloperów firmy Fabrikam”, a następnie kliknij przycisk **Zapisz** u dołu strony.

Teraz nowy nagłówek powinien być widoczny na każdej stronie portalu dla deweloperów.

> Aby otworzyć portal dla deweloperów w portalu wydawcy, kliknij przycisk **Portal dla deweloperów** na górnym pasku.

## <a name="change-headers-styling"> </a>Zmiana stylu nagłówków

Kolory, czcionki, rozmiary, odstępy i inne elementy stylu związane z dowolną stroną portalu są definiowane przez reguły stylów. Aby edytować style, kliknij pozycję **Wygląd** w menu **Portal dla deweloperów** w portalu wydawcy. Następnie kliknij przycisk **Rozpocznij dostosowywanie**, aby włączyć edytor stylów.

Przeglądarka przełączy się na ukrytą stronę w portalu dla deweloperów, która zawiera próbki zawartości wraz z przykładami dla wszystkich reguł stylów używanych w dowolnym miejscu witryny. Aby otworzyć edytor stylów, przesuń kursor nad cienką, szarą, pionową linię w lewej części strony. Powinien pojawić się pasek narzędzi edytora.

![Pasek narzędzi do dostosowywania][api-management-customization-toolbar]

Dostępne są dwa główne tryby edycji reguł stylów — opcja **Edytuj wszystkie reguły** powoduje wyświetlenie listy wszystkich reguł stylów używanych w dowolnym miejscu, natomiast opcja **Wybierz element** pozwala na wybór elementu z otwartej strony, a następnie wyświetlenie stylów tylko dla tego elementu.

W tej części chcemy zmienić tylko style nagłówków. Kliknij opcję **Wybierz element** na pasku narzędzi edytora stylów, a następnie kliknij opcję **Wybierz element, aby dostosować**. Teraz umieszczenie kursora myszy nad elementami powoduje ich wyróżnienie, a kliknięcie wyróżnionego elementu pozwala rozpocząć edycję jego stylów. Ustaw wskaźnik myszy nad tekstem, który jest używany jako nazwa firmy w nagłówku („Portal dla deweloperów firmy Fabrikam”, jeśli zastosowano wcześniejsze instrukcje), a następnie kliknij go. W edytorze stylów pojawi się zestaw nazwanych i podzielonych na kategorie reguł stylów.

Każda reguła reprezentuje właściwość stylu wybranego elementu. Na przykład w wybranym powyżej tekście nagłówka rozmiar tekstu to @font-size-h1, a nazwa czcionki z alternatywami to @headings-font-family.

> Jeśli znasz środowisko [bootstrap][], zauważysz, że te reguły są w rzeczywistości [zmiennymi LESS][] w motywie bootstrap używanym w portalu dla deweloperów.

Zmienimy teraz kolor tekstu nagłówka. Zaznacz zawartość pola **@headings-color** i wpisz **#000000**. Jest to kod szesnastkowy koloru czarnego. Wtedy na końcu pola tekstowego pojawi się wskaźnik koloru w kształcie kwadratu. Po kliknięciu tego wskaźnika można wybrać kolor z selektora kolorów.

![Selektor kolorów][api-management-customization-toolbar-color-picker]

Po zakończeniu wprowadzania zmian w stylach wybranego elementu kliknij opcję **Podgląd zmian**, aby wyświetlić wyniki na ekranie. W tym momencie są one widoczne tylko dla administratorów. Aby zmiany stały się widoczne dla wszystkich użytkowników, kliknij przycisk **Publikuj** znajdujący się w edytorze stylów i zatwierdź zmiany.

![Menu Publikuj][api-management-customization-toolbar-publish-form]

> Aby zmienić reguły stylów, które są stosowane do innego elementu na stronie, wykonaj tę samą procedurę co dla nagłówka. Kliknij opcję **Wybierz element** w edytorze stylów, wybierz interesujący Cię element i zacznij modyfikować wartości reguł stylu wyświetlanych na ekranie.

## <a name="edit-page-contents"> </a>Edytowanie zawartości strony

Portal dla deweloperów składa się z automatycznie wygenerowanych stron, takich jak Interfejsy API, Produkty, Aplikacje, Problemy, a także z ręcznie wpisanej zawartości. Ponieważ jest oparty na systemie zarządzania zawartością, możesz tworzyć zawartość zgodnie z potrzebami.

Aby wyświetlić listę wszystkich istniejących stron zawartości, kliknij pozycję **Zawartość** w menu **Portal dla deweloperów** w portalu wydawcy.

![Zarządzanie zawartością][api-management-customization-manage-content]

Kliknij stronę **Zapraszamy**, aby edytować zawartość strony głównej portalu dla deweloperów. Wprowadź żądane zmiany, w razie potrzeby wyświetl ich podgląd, a następnie kliknij opcję **Publikuj teraz**, aby stały się widoczne dla wszystkich użytkowników.

> Strona główna używa specjalnego układu, który umożliwia wyświetlanie transparentu u góry. Transparentu nie można edytować w sekcji **Zawartość**. Aby edytować ten transparent, kliknij opcję **Widżety** w menu **Portal dla deweloperów**, wybierz opcję **Strona główna** z listy rozwijanej **Bieżąca warstwa**, a następnie otwórz element **Transparent** w obszarze **Wyróżniona sekcja**. Zawartość tego widżetu można edytować podobnie jak dowolnej innej strony.

## <a name="next-steps"> </a>Następne kroki

-   Dowiedz się, w jaki sposób dostosować zawartość portalu dla deweloperów przy użyciu [szablonów portalu dla deweloperów](api-management-developer-portal-templates.md).

[Zmiana tekstu lub logo w nagłówkach stron]: #change-page-headers
[Zmiana stylu nagłówków]: #change-headers-styling
[Edytowanie zawartości strony]: #edit-page-contents
[Następne kroki]: #next-steps

[Klasyczny portal Azure]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-customize-portal/api-management-management-console.png
[api-management-widgets-header]: ./media/api-management-customize-portal/api-management-widgets-header.png
[api-management-customization-toolbar]: ./media/api-management-customize-portal/api-management-customization-toolbar.png
[api-management-customization-toolbar-color-picker]: ./media/api-management-customize-portal/api-management-customization-toolbar-color-picker.png
[api-management-customization-toolbar-publish-form]: ./media/api-management-customize-portal/api-management-customization-toolbar-publish-form.png
[api-management-customization-manage-content]: ./media/api-management-customize-portal/api-management-customization-manage-content.png


[bootstrap]: http://getbootstrap.com/
[zmiennymi LESS]: http://getbootstrap.com/css/



<!--HONumber=sep16_HO1-->


