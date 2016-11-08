# Przewodnik dla współautorów dokumentacji technicznej platformy Azure
Znajdujesz się w repozytorium serwisu GitHub będącym źródłem dokumentacji technicznej publikowanej w centrum dokumentacji platformy Azure pod adresem [http://azure.microsoft.com/documentation](http://azure.microsoft.com/documentation).

To repozytorium zawiera również wskazówki ułatwiające współtworzenie naszej dokumentacji technicznej.  Aby uzyskać listę artykułów przewodnika dla współautorów, przejdź do [indeksu](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).

## Współtworzenie dokumentacji platformy Azure
Dziękujemy za zainteresowanie współtworzeniem dokumentacji platformy Azure!

* [Sposoby współtworzenia](#ways-to-contribute)
* [Kodeks postępowania](#code-of-conduct)
* [O współtworzeniu zawartości platformy Azure](#about-your-contributions-to-azure-content)
* [Organizacja repozytorium](#repository-organization)
* [Używanie serwisu GitHub, usługi Git i tego repozytorium](#use-github-git-and-this-repository)
* [Używanie języka znaczników markdown do formatowania tematu](#how-to-use-markdown-to-format-your-topic)
* [Opinie, komentarze i pomoc techniczna](./contributor-guide/feedback-and-comments.md)
* [Więcej zasobów](#more-resources)
* [Indeks wszystkich artykułów przewodnika dla współautorów](./contributor-guide/contributor-guide-index.md) (powoduje otworzenie nowej strony)

## Sposoby współtworzenia
[Dokumentację platformy Azure](http://azure.microsoft.com/documentation/) można współtworzyć na kilka różnych sposobów:

* Uczestnictwo w [dyskusjach prowadzonych na forum](http://social.msdn.microsoft.com/Forums/windowsazure/home).
* Przesyłanie komentarzy w usłudze Disqus poniżej artykułów.
* W łatwy sposób można współtworzyć artykuły techniczne w interfejsie użytkownika serwisu GitHub. Znajdź artykuł w tym repozytorium lub wyświetl artykuł w witrynie [http://azure.microsoft.com/documentation](http://azure.microsoft.com/documentation), a następnie kliknij link w artykule prowadzącym do źródła tego artykułu w serwisie GitHub.
* W przypadku wprowadzania istotne zmian istniejącego artykułu, dodawania lub zmieniana obrazów lub współtworzenia noweg artykuł, należy przeprowadzić rozwidlenie tego repozytorium, zainstalować programy Git Bash i MarkdownPad oraz nauczyć się niektórych poleceń usługi Git.

## Kodeks postępowania
W tym projekcie przyjęto [Kodeks postępowania oprogramowania Open Source firmy Microsoft](https://opensource.microsoft.com/codeofconduct/). Aby uzyskać więcej informacji, zobacz [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) (Często zadawane pytania dotyczące kodeksu postępowania) lub wyślij wiadomość e-mail na adres [opencode@microsoft.com](mailto:opencode@microsoft.com) w przypadku jakichkolwiek dodatkowych pytań lub komentarzy.

## O współtworzeniu zawartości platformy Azure
### Drobne poprawki
Drobne poprawki lub wyjaśnienia wprowadzane do dokumentacji i przykładów kodu w tym repozytorium są objęte [Warunkami korzystania z witryny Azure](http://azure.microsoft.com/support/legal/website-terms-of-use/).

### Większe zgłoszenia
Jeśli prześlesz żądanie ściągnięcia nowych treści lub znaczących zmian dokumentacji i przykładów kodu, wyślemy Ci komentarz w serwisie GitHub z prośbą o przesłanie Umowy licencyjnej dotyczącej materiałów przekazywanych online w przypadku gdy jesteś w jednej z następujących grup:

* Członkowie grupy Microsoft Open Technologies.
* Współautorzy, którzy nie pracują dla firmy Microsoft.

Przed zaakceptowaniem żądania ściągania konieczne jest wypełnienie formularza online.

Szczegółowe informacje są dostępne pod adresem [http://azure.github.io/guidelines/#cla](http://azure.github.io/guidelines/#cla).

## Organizacja repozytorium
Zawartość w repozytorium azure-content jest zgodna z organizacją dokumentacji w witrynie [Azure.Microsoft.com](http://azure.microsoft.com). To repozytorium zawiera dwa foldery główne:

### \articles
Folder *\articles* zawiera artykuły dokumentacji sformatowane jako pliki markdown z rozszerzeniem *md*.

Artykuły w katalogu głównym są publikowane w witrynie Azure.Microsoft.com w ścieżce *http://azure.microsoft.com/documentation/articles/{nazwa_artykułu_bez_rozszerzenia_md}/*.

* **Nazwy plików artykułów:** zobacz [nasze wskazówki dotyczące nazw plików](./contributor-guide/file-names-and-locations.md).

Artykuły znajdujące się w swoich własnych folderach usług są publikowane w witrynie Azure.Microsoft.com w ścieżce *http://azure.microsoft.com/documentation/articles/folder-usługi/{nazwa_artykułu_bez_rozszerzenia_md}/*

* **Podfoldery z multimediami:** folder *\articles* zawiera folder *\media* na potrzeby plików multimedialnych artykułów z katalogu głównego, w którym znajdują się podfoldery z obrazami dla każdego artykułu.  Foldery usług zawierają oddzielny folder z multimediami dla artykułów w ramach poszczególnych folderów usług. Foldery obrazów artykułu są nazywane tak samo jak plik artykułu, ale nie mają rozszerzenia pliku *md*.

### \includes
Możliwe jest tworzenie sekcji zawartości wielokrotnego użytku, które zostaną dołączone do co najmniej jednego artykułu. Zobacz [Custom extensions used in our technical content](./contributor-guide/custom-markdown-extensions.md) (Niestandardowe rozszerzenia używane w naszych artykułach technicznych).

### \markdown templates
Ten folder zawiera nasz standardowy szablon markdown z podstawowym formatowaniem języka znaczników markdown na potrzeby artykułu.

### \contributor-guide
Ten folder zawiera artykuły, które są częścią naszego przewodnika dla współautorów.  

## Używanie serwisu GitHub, usługi Git i tego repozytorium
Aby uzyskać informacje o sposobie współtworzenia, sposobie korzystania z interfejsu użytkownika serwisu GitHub do współtworzenia drobnych zmian oraz sposobie rozwidlania i klonowania repozytorium na potrzeby bardziej znaczących wkładów, zobacz [Install and set up tools for authoring in GitHub](./contributor-guide/tools-and-setup.md) (Instalowanie i konfigurowanie narzędzi używanych do tworzenia w serwisie GitHub).

Jeśli zainstalowano program GitBash i wybrano pracę lokalną, procedura tworzenia nowej lokalnej gałęzi roboczej, wprowadzania zmian i przesyłania zmian z powrotem do gałęzi głównej znajduje się w artykule [Git commands for creating a new article or updating an existing article](./contributor-guide/git-commands-for-master.md) (Polecenia usługi Git związane z tworzeniem nowego artykułu lub aktualizowaniem istniejącego artykułu).

### Gałęzie
Zaleca się tworzenie lokalnych gałęzi roboczych przeznaczonych dla określonego zakresu zmian. Każda gałąź powinna być ograniczona do jednej koncepcji lub artykułu, aby usprawnić przepływ pracy i zmniejszyć prawdopodobieństwo wystąpienia konfliktów scalania.  Następujące prace mają odpowiedni zakres dla nowej gałęzi:

* Nowy artykuł (i skojarzone obrazy)
* Zmiany dotyczące pisowni i gramatyki artykułu
* Stosowanie pojedynczej zmiany formatowania w dużych zestawach artykułów (np. nowa stopka praw autorskich)

## Używanie języka znaczników markdown do formatowania tematu
We wszystkich artykułach w tym repozytorium jest używany język znaczników markdown specyficzny dla serwisu GitHub (GitHub flavored markdown).  Poniżej znajduje się lista zasobów.

* [Podstawy języka znaczników markdown](https://help.github.com/articles/markdown-basics/)
* [Ściągawka dotycząca języka znaczników markdown w wersji do druku](./contributor-guide/media/documents/markdown-cheatsheet.pdf?raw=true)
* Lista edytorów języka znaczników markdown znajduje się w [temacie dotyczącym narzędzi i konfiguracji](./contributor-guide/tools-and-setup.md#install-a-markdown-editor).

## Metadane artykułu
Metadane artykułu umożliwiają użycie pewnych funkcji w witrynie azure.microsoft.com, takich jak informacje o autorach, informacje o współautorach, obszar nawigacji, opisy artykułów i optymalizacje SEO, a także raportowanie używane przez firmę Microsoft do oceny wydajności zawartości. Metadane są zatem bardzo ważne. [Oto wskazówki, które pozwalają sprawdzić, czy metadane zostały poprawnie utworzone](./contributor-guide/article-metadata.md).

## Więcej zasobów
Zobacz [indeks przewodnika dla współautorów](./contributor-guide/contributor-guide-index.md), aby wyświetlić wszystkie tematy zawierające wskazówki.

<!--HONumber=Aug16_HO1-->


