## <a name="microsoft-open-source-code-of-conduct"></a>Microsoft Open źródła Kodeks postępowania

W tym projekcie przyjęto [Kodeks postępowania oprogramowania Open Source firmy Microsoft](https://opensource.microsoft.com/codeofconduct/).
Aby uzyskać więcej informacji, zobacz [kodu z postępowania — często zadawane pytania](https://opensource.microsoft.com/codeofconduct/faq/) lub skontaktuj się z [ opencode@microsoft.com ](mailto:opencode@microsoft.com) z jakichkolwiek dodatkowych pytań lub komentarzy.

## <a name="contribute-to-azure-technical-documentation"></a>Współtworzenie dokumentacji technicznej platformy Azure
Chętnie poznamy wkładów z naszej społeczności (użytkowników, klientów, partnerów, pracownicy MSFT poza core produktu Azure jednostki, itp.) oraz z pracowników pracujących w jednostkach produktu Azure core. Jak współtworzenia zależy od tego, którzy są:

* **Społeczność — niewielkie aktualizacje**: Jeśli wymieniają się niewielkie aktualizacje poza zgodność dowolny, możesz Znajdź artykuł w tym repozytorium lub Wyświetl artykuł w [https://docs.microsoft.com/azure](https://docs.microsoft.com/azure) i kliknij przycisk **Edytuj** link w artykule prowadzącym do źródła serwisu GitHub dla tego artykułu. Następnie tylko za pomocą interfejsu użytkownika serwisu GitHub wprowadź aktualizacje. Lub Zapraszamy do rozwidlenia repozytorium i przesłać aktualizacji z rozwidlenia.

* **Społeczność — nowe artykuły**: Jeśli jest częścią społeczności platformy Azure i chcesz utworzyć nowy artykuł, potrzebujesz do pracy z pracownikiem, aby pomóc nam w tym nowe repozytorium zawartość przy użyciu kombinacji pracy w publicznej i prywatnej.

* **Pracownicy**: Jeśli są zapisywania technicznych, Menedżer programu lub deweloper z zespołu produktu dla usługi Azure i jest zadanie współtworzyć lub tworzyć artykuły techniczne, należy użyć prywatnym repozytorium (https://github.com/ MicrosoftDocs/azure-docs-pr). W przypadku wprowadzania istotne zmian istniejącego artykułu, dodawania lub zmieniana obrazów lub współtworzenia noweg artykuł, musisz rozwidlenie tego repozytorium, zainstalować programy Git Bash, Edytor markdown i dowiedzieć się niektórych poleceń usługi git. Zobacz [przewodnika dla współautorów wewnętrzny](https://review.docs.microsoft.com/en-us/help/contribute/?branch=master) Aby uzyskać więcej informacji.


## <a name="about-your-contributions-to-azure-content"></a>O współtworzeniu zawartości platformy Azure
### <a name="minor-corrections"></a>Drobne poprawki
Drobne poprawki lub wyjaśnienia wprowadzane do dokumentacji i przykładów kodu w tym repozytorium są objęte [docs.microsoft.com warunki użytkowania](https://docs.microsoft.com/legal/termsofuse).

### <a name="larger-submissions"></a>Większe zgłoszenia
Jeśli prześlesz żądanie ściągnięcia nowych lub znaczących zmian dokumentacji i przykładów kodu, wyślemy Ci komentarz w serwisie GitHub prośbą o przesłanie online umowy licencyjnej udziału (CLA), jeśli nie masz pracownika z firmy Microsoft. Przed zaakceptowaniem żądania ściągania konieczne jest wypełnienie formularza online.

## <a name="tools-and-setup"></a>Narzędzia i ustawienia
Uczestnicy społeczności można użyć interfejsu użytkownika serwisu GitHub lub rozwidlania repozytorium do ich współtworzenia. Odwiedzić pracowników [przewodnika dla współautorów wewnętrzny](https://review.docs.microsoft.com/en-us/help/contribute/?branch=master) uzyskać więcej informacji o sposobie współtworzenia dokumentacji technicznej ustawić.

## <a name="repository-organization"></a>Organizacja repozytorium
Zawartość w repozytorium azure-docs jest zgodna z organizacją dokumentacji w https://docs.microsoft.com/azure. To repozytorium zawiera dwa foldery główne:

### <a name="articles"></a>\articles
Folder *\articles* zawiera artykuły dokumentacji sformatowane jako pliki markdown z rozszerzeniem *md*. Artykuły zwykle są pogrupowane według usługi Azure.

*\Articles* folder zawiera *\media* dla artykułu plików multimedialnych katalogu głównego, w którym znajdują się podfoldery z obrazami dla każdego artykułu.  Foldery usług zawierają oddzielny folder z multimediami dla artykułów w ramach poszczególnych folderów usług. Foldery obrazów artykułu są nazywane tak samo jak plik artykułu, ale nie mają rozszerzenia pliku *md*.

### <a name="includes"></a>\includes
Możliwe jest tworzenie sekcji zawartości wielokrotnego użytku, które zostaną dołączone do co najmniej jednego artykułu. 

## <a name="how-to-use-markdown-to-format-your-topic"></a>Używanie języka znaczników markdown do formatowania tematu
We wszystkich artykułach w tym repozytorium jest używany język znaczników markdown specyficzny dla serwisu GitHub (GitHub flavored markdown).  Poniżej znajduje się lista zasobów.

* [Podstawy języka znaczników markdown](https://help.github.com/articles/markdown-basics/)
* [Ściągawka dotycząca języka znaczników markdown do druku](./contributor-guide/media/documents/markdown-cheatsheet.pdf?raw=true)


## <a name="labels"></a>Etykiety
W repozytorium publicznych docs azure automatyczne etykiety są przypisane do ściągnięcia żądania w celu ułatwienia zarządzania przepływ żądania ściągania i informacją o tym, co się dzieje z żądania ściągnięcia:

* Umowa licencyjna wkład powiązane
  * CLA niewymagany: zmiana jest relatywnie i nie wymaga Zaloguj się CLA.
  * wymagane CLA: konsekwencji tej zmiany jest stosunkowo dużej i wymaga, aby zarejestrować CLA.
  * podpisana CLA: Współautor podpisany CLA, więc żądanie ściągnięcia teraz przejść do przodu do przeglądu.
* Zmiany wysłane do tworzenia: Autor został powiadomiony o oczekującego żądania.
* gotowe do scalenia: gotowa do przeglądu przez nasz zespół Przegląd żądania ściągnięcia.


