---
title: "Wprowadzenie — narzędzia modelowania zagrożeń Microsoft - Azure | Dokumentacja firmy Microsoft"
description: "Jest to bardziej Przegląd wyróżnianie narzędzia modelowania zagrożeń w akcji."
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 2d940b42108948f4cd36a585f1e79def05fe8fd3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="getting-started-with-the-threat-modeling-tool"></a>Wprowadzenie do korzystania z narzędzi modelowania zagrożeń

Zespół Cloud i Enterprise zabezpieczeń narzędzia wydał Podgląd narzędzia do modelowania zagrożeń wcześniej w tym roku jako wolną  **[kliknij do pobierania](https://aka.ms/tmtpreview)**. Zmiana mechanizm dostarczania pozwala push najnowsze ulepszenia i poprawki dla klientów każdym otwarciu narzędzia, co ułatwia i obsługiwane.
W tym artykule przeprowadza użytkownika przez proces wprowadzenie zagrożeń Microsoft SDL modelowania podejścia i przedstawiono sposób użyć narzędzia umożliwiające tworzenie modeli dużą zagrożeń jako szkielet procesu zabezpieczeń.

W tym artykule opiera się na wiedzy na temat zagrożenia SDL modelowania podejście. Szybki przegląd, można znaleźć w temacie  **[aplikacji sieci Web modelowania zagrożeń](https://msdn.microsoft.com/library/ms978516.aspx)**  i zarchiwizowaną wersję  **[odkrywanie zabezpieczeń mogą przy użyciu podejścia krok](https://docs.google.com/viewer?a=v&pid=sites&srcid=ZGVmYXVsdGRvbWFpbnxzZWN1cmVwcm9ncmFtbWluZ3xneDo0MTY1MmM0ZDI0ZjQ4ZDMy)**  Opublikowany artykuł w witrynie MSDN w 2006 roku.

Do szybkiego podsumowywania podejście obejmuje tworzenie diagramu, identyfikowania zagrożeń, zmniejszenia ich i sprawdzanie poprawności każdej środki zaradcze. Poniżej przedstawiono diagram, który prezentuje ten proces:

![Proces SDL](./media/azure-security-threat-modeling-tool/sdlapproach.png)

## <a name="starting-the-threat-modeling-process"></a>Uruchamianie procesu modelowania zagrożeń

Podczas uruchamiania narzędzia modelowania zagrożeń można zauważyć kilka rzeczy, jak pokazano na rysunku:

![Strona początkowa pusta](./media/azure-security-threat-modeling-tool/tmtstart.png)

### <a name="threat-model-section"></a>Sekcja modelu zagrożeń

| Składnik                                   | Szczegóły                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Opinie, sugestie i przycisk problemów** | Przejście  **[MSDN Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=sdlprocess)**  dla wszystkich elementów SDL. Udostępnia możliwość zapoznaj się z artykułem co inne robią użytkownicy, a także rozwiązania i zalecenia. Jeśli nadal nie możesz znaleźć co, czego szukasz, Wyślij wiadomość e-mail tmtextsupport@microsoft.com na naszym zespołem pomocy technicznej ułatwić                                                                                                                            |
| **Tworzenie modelu**                          | Otwiera pustej kanwy można narysować diagramu. Pamiętaj wybrać szablon, którego chcesz użyć dla modelu                                                                                                                                                                                                                                                                                                                                                                       |
| **Szablon dla nowych modeli**                 | Musisz wybrać szablon przed utworzeniem modelu. Nasze główne szablon jest szablonu modelu zagrożeń Azure, który zawiera wzorników specyficzne dla platformy Azure, zagrożenia i metody ich rozwiązywania. Dla ogólnego modeli wybierz TM SDL wiedzy z menu rozwijanego. Czy chcesz utworzyć własny szablon lub Prześlij nową dla wszystkich użytkowników? Zapoznaj się z naszym  **[repozytorium szablonu](https://github.com/Microsoft/threat-modeling-templates)**  GitHub strony, aby dowiedzieć się więcej                              |
| **Otwieranie modelu**                            | <p>Zostanie otwarta wcześniej zapisany modeli zagrożeń. Funkcja ostatnio otworzyć modeli stanowi doskonałe rozwiązanie, jeśli trzeba otworzyć najnowsze pliki. Po umieszczeniu na wybór, znajdziesz 2 sposoby otwierania modeli:</p><p><ul><li>Otwórz z tego komputera — klasyczny sposób otwierania pliku przy użyciu lokalnej pamięci masowej</li><li>Otwieranie z poziomu usługi OneDrive — zespołom folderów w usłudze OneDrive zapisywanie i udostępnianie ich modeli zagrożeń w jednym miejscu, aby zwiększyć wydajność i współpracy</li></ul></p> |
| **Wprowadzenie — przewodnik**                   | Otwiera  **[narzędzia do modelowania zagrożeń Microsoft](./azure-security-threat-modeling-tool.md)**  strony głównej                                                                                                                                                                                                                                                                                                                                                                                            |

### <a name="template-section"></a>Sekcja szablonu

| Składnik               | Szczegóły                                                                                                                                                          |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Utwórz nowy szablon** | Otwiera pusty szablon do tworzenia. Jeśli nie masz doskonałej znajomości tworzenia szablonów od początku, zalecamy kompilacji z istniejących |
| **Otwórz szablon**       | Otwiera istniejących szablonów można wprowadzić zmiany w                                                                                                              |

Zespołu narzędzia modelowania zagrożeń jest ciągle pracuje, aby poprawić funkcje narzędzi i środowisko. Kilka drobne zmiany mogą mieć miejsce w ciągu roku, ale wszystkie istotne zmiany wymagają modyfikacji oprogramowania w przewodniku. Zapoznaj się często, aby upewnić się, że możesz uzyskać najnowsze anonsów.

## <a name="building-a-model"></a>Tworzenie modelu

W tej sekcji możemy wykonaj następujące czynności:

- Cristina (Deweloper)
- Ricardo (Menedżer programu) i
- Ashish (tester)

Wkrótce za pośrednictwem procesu tworzenia ich pierwszym modelu zagrożeń.

> Ricardo: Cześć Cristina, pracowano diagramu modelu zagrożeń i chce upewnij się, że dotarliśmy szczegóły prawo. Proszę o pomoc go przejrzeć?
> Cristina: absolutnie. Spójrzmy.
> Ricardo zostanie otwarte narzędzie i udostępnia jego ekranu Cristina.

![Podstawowe zagrożenia modelu](./media/azure-security-threat-modeling-tool/basictmt.png)

> Cristina: Ok wygląda prostego, ale należy zapoznać się z mnie przy jego użyciu?
> Ricardo: się! Oto podział:
> - Nasze człowieka użytkownika jest rysowane jako jednostka poza — kwadrat
> - Wysyłasz polecenia do serwera sieci Web — okręgu
> - Serwer sieci Web jest konsultacji bazy danych (dwie linie)

Co Ricardo tylko wykazało Cristina jest DPD, skrót  **[Diagram przepływu danych](https://en.wikipedia.org/wiki/Data_flow_diagram)**. Narzędzie modelowania zagrożeń umożliwia użytkownikom określanie granice zaufania, wskazują linie czerwony przerywana, gdzie są różne jednostki w formancie. Na przykład Administratorzy IT wymaga systemu usługi Active Directory na potrzeby uwierzytelniania, dzięki czemu usługi Active Directory znajduje się poza ich kontroli.

> Cristina: Wszystko wygląda w porządku do mnie. Jakie zagrożenia?
> Ricardo: Chcę wyświetlić.

## <a name="analyzing-threats"></a>Analizowanie zagrożeń

Po klika w widoku analizy z zaznaczenia menu ikona (plik z Lupa), jest on przekierowanie do listy wygenerowanego zagrożeń narzędzie modelowania zagrożeń znaleziono na podstawie szablonu domyślnego używający podejście SDL o nazwie  **[ STRIDE (Spoofing naruszeniu ujawnienie informacji, odmowę usługi i podniesienie uprawnień)](https://en.wikipedia.org/wiki/STRIDE_(security))**. Pomysł jest, że oprogramowanie objęty przewidywalną zbiór zagrożenia, które można znaleźć przy użyciu tych kategorii 6.

Ta metoda działa jak zabezpieczanie domu w celu zapewnienia każdego drzwi i okno ma mechanizm blokowania w miejscu przed dodaniem system alarmu lub sklejek po złodziej będzie.

![Podstawowe zagrożenia](./media/azure-security-threat-modeling-tool/basicthreats.png)

Ricardo rozpoczyna się po wybraniu pierwszy element na liście. Oto, co się stanie:

Po pierwsze została rozszerzona o interakcji między dwoma wzorników

![Interakcji](./media/azure-security-threat-modeling-tool/interaction.png)

Drugi, dodatkowe informacje o zagrożenie jest wyświetlany w oknie właściwości zagrożeń

![Informacje o interakcji](./media/azure-security-threat-modeling-tool/interactioninfo.png)

Wygenerowany zagrożeń pomaga go zrozumieć potencjalne wady projektowe. Kategoryzacji krok zapewnia, mu pomysł na potencjalnych ataków, podczas gdy dodatkowy opis informuje, mu dokładnie co to jest błąd, oraz potencjalnych sposobów, aby go unikać. Służy on edytowalnego pola notatki w szczegółach uzasadnienie lub zmienić priorytet oceny w zależności od jego organizacji usterki paska.

Szablony usługi Azure ma dodatkowe szczegóły, aby pomóc użytkownikom w zrozumieniu nie tylko na czym polega problem, ale także jak to naprawić, dodając opisy, przykłady i hiperłącza do dokumentacji specyficzne dla platformy Azure.

Opis wprowadzone mu znaczenie Dodawanie mechanizm uwierzytelniania, aby uniemożliwić użytkownikom fałszowania, należy pamiętać, ujawniania pierwszy zagrożeń można pracować. Kilka minut do dyskusji o Cristina, ich zrozumienie znaczenie wdrażanie kontroli dostępu i role. Ricardo wypełnione niektóre szybkie uwagi dotyczące upewnij się, że te zostały wprowadzone.

Jak Ricardo przeszła do zagrożeń w obszarze ujawnienie informacji, zrealizowane on plan kontroli dostępu, wymagane niektóre konta tylko do odczytu do generowania inspekcji i raportów. On zastanawiasz się, czy należy to nowe zagrożenia, ale środki zaradcze były takie same, więc on odpowiednio zagrożenia.
On również traktować o ujawnienie informacji bit więcej i zrealizowany taśm kopii zapasowych miał zostać wymagają szyfrowania, zadania dla zespołu operacji.

Zagrożenia nie ma zastosowania do projektu z powodu istniejących środki zaradcze lub zabezpieczeń gwarantuje z listy rozwijanej stanu może być zmieniona na "Brak". Istnieją trzy inne opcje: nie rozpoczęto — wybór domyślny, konieczne badanie — używana na potrzeby zapasów i Mitigated — po pełni jest praca nad.

## <a name="reports--sharing"></a>Udostępnianie & raportów

Po Ricardo przechodzi przez listy z Cristina i dodaje ważne uwagi, środki zaradcze/uzasadnienie, priorytet i zmiany stanu, he wybiera Raporty -> Utwórz pełny raport -> Zapisz raport, który wyświetla nieuprzywilejowany raportu dla niego podąża współpracownikom Aby zapewnić działanie właściwe zabezpieczenia jest zaimplementowana.

![Informacje o interakcji](./media/azure-security-threat-modeling-tool/report.png)

Jeśli Ricardo chce zamiast tego udziału pliku, on łatwo to zrobić przez zapisywanie w jego organizacji konta usługi OneDrive. Po on robi to on skopiuj łącze do dokumentu i udostępniać jego współpracowników. 

## <a name="threat-modeling-meetings"></a>Spotkań modelowania zagrożeń

Gdy Ricardo jego modelu zagrożeń są wysyłane do swojego współpracownika przy użyciu usługi OneDrive, Ashish, tester, został underwhelmed. Sprawiał, takich jak Ricardo i Cristina pominiętych kilka ważnych sytuacjach wyjątkowych, które można łatwo złamać. Jego skepticism jest uzupełnienie modeli zagrożeń.

W tym scenariuszu po Ashish przejął modelu zagrożeń kontaktował się w dwóch spotkań modelowania zagrożeń: jedno spotkanie do synchronizacji w procesie i przeprowadzenie diagramy, a następnie drugiego spotkania na zagrożenie Przejrzyj i podpisywania.

W pierwszym spotkania, Ashish poświęcony na 10 minut wszyscy za pośrednictwem procesu modelowania zagrożeń SDL przejście. Następnie on pobierane diagramu modelu zagrożeń i uruchomić wyjaśniający, szczegółowo. W ciągu pięciu minut gdyby zostały zidentyfikowane ważne brakuje składnika.

Kilka minut później, Ashish i Ricardo uzyskano w rozszerzonej omówienie jak serwer sieci Web został skompilowany. Nie było idealny spotkania kontynuować, ale ostatecznie uzgodnione wszyscy czy wczesne wykrywanie niezgodności został przechodząc do zapisywania ich w przyszłości czasu.

Drugi spotkania zespołu udał za pośrednictwem zagrożenia, opisano kilka metod umożliwiających im sprostać i wyrejestrowane na modelu zagrożeń. Wyewidencjonowany dokumentu do kontroli źródła, a nadal rozwoju.

## <a name="thinking-about-assets"></a>Analiza zasobów

Niektóre użytkownikom, którzy mają zagrożeń modelowane mogą pojawić się czy nie zostało to jeszcze wspomnieliśmy o zasobach w ogóle. Firma Microsoft już odnaleziony wielu inżynierów oprogramowania lepsze niż ich zrozumienie pojęcia zasobów i zasoby, które osoba atakująca może być zainteresowany zrozumienie ich oprogramowania.

Jeśli zamierzasz modelu zagrożeń domu, mogą rozpoczyna się od planowania rodziny, zdjęć niemożliwych lub cenne kompozycji. Prawdopodobnie może uruchomić krokiem jest wybranie bieżącego systemu zabezpieczeń i który mogą przestać działać w. Lub może rozpocząć przy uwzględnieniu fizycznych funkcje, takie jak puli lub porch frontonu. Są to odpowiednikiem analiza zasobów, osoby atakujące lub oprogramowanie — projekt. Z tych trzech metod pracę.

Podejście do zagrożenia modelowania, które firma Microsoft zostały przedstawione w tym miejscu jest znacznie prostsze niż Microsoft przeprowadził w przeszłości. Znaleziono się, że podejście do projektowania oprogramowania dobrze się sprawdza liczbę zespołów. Mamy nadzieję, że zawierające należy do Ciebie.

## <a name="next-steps"></a>Następne kroki

Wyślij swoje pytania, komentarze i zastrzeżenia co do tmtextsupport@microsoft.com. **[Pobierz](https://aka.ms/tmtpreview)**  narzędzie modelowania zagrożeń, aby rozpocząć.