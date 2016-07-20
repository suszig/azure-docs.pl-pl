Platforma Azure określi, że aplikacja używa języka Python, **jeśli są spełnione oba te warunki**:

- plik requirements.txt znajduje się w folderze głównym
- w folderze głównym znajduje się dowolny plik PY LUB plik runtime.txt, który określa wartość python

W takim przypadku zostanie użyty określony skrypt wdrażania języka Python, który wykonuje standardową synchronizację plików, a także dodatkowe operacje języka Python, takie jak:

- Automatyczne zarządzanie środowiskiem wirtualnym
- Instalacja pakietów wymienionych w pliku requirements.txt przy użyciu adresu pip
- Tworzenie odpowiedniego pliku web.config na podstawie wybranej wersji języka Python.
- Zbieranie plików statycznych dla aplikacji Django

Możesz kontrolować niektóre aspekty domyślnych kroków wdrażania bez konieczności dostosowywania skryptu.

Jeśli chcesz pominąć wszystkie kroki wdrażania właściwe dla języka Python, możesz utworzyć ten pusty plik:

    \.skipPythonDeployment

Jeśli chcesz pominąć zbieranie plików statycznych dla aplikacji Django:

    \.skipDjango 

Aby uzyskać większą kontrolę nad wdrożeniem, można zastąpić domyślny skrypt wdrożenia, tworząc następujące pliki:

    \.deployment
    \deploy.cmd

Do utworzenia plików można użyć [interfejsu wiersza polecenia platformy Azure][].  Użyj tego polecenia w folderze projektu:

    azure site deploymentscript --python

Jeśli te pliki nie istnieją, platforma Azure tworzy tymczasowy skrypt wdrażania i uruchamia go.  Jest on identyczny z tworzonym za pomocą polecenia powyżej.

[interfejsu wiersza polecenia platformy Azure]: http://azure.microsoft.com/downloads/



<!--HONumber=Jun16_HO2-->


