Gdy host funkcji działa lokalnie, zapisuje dzienniki na następującej ścieżce:

```
<DefaultTempDirectory>\LogFiles\Application\Functions
```

W systemie Windows `<DefaultTempDirectory>` jest pierwszą wartość znaleziono USERPROFILE TMP, TEMP, zmienne środowiskowe lub katalogu systemu Windows.
System MacOS lub Linux `<DefaultTempDirectory>` jest zmienną środowiskową TMPDIR.

[!Note]
Po uruchomieniu hosta funkcji zastąpią istniejące struktury plików w katalogu.