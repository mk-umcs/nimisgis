# Pixi tutorial

Menedżer pakietów [Pixi](https://pixi.prefix.dev/) jest jednym z prostszych i szybszych sposobów na instalowanie w jednym środowisku różnych bibliotek Pythona do przetwarzania danych przestrzennych.

Polecenia z poniższego tutoriala wykonywane będą w uruchomionym terminalu (program `cmd` lub PowerShell pod Windowsem, lub powłoka w terminalu pod Linuksem). Niektóre polecenia mogą nie działać w PowerShellu. W razie potrzeby można nawet z poziomu PowerShell po prostu uruchomić standardowy interpreter poleceń za pomocą polecenia `cmd`.

## 1. Instalacja

Najprościej zainstalować Pixi wklejając i uruchamiając odpowiednie polecenie z dokumentacji Pixi. Szczegóły podane są na stronie [Pixi &ndash; Installation](https://pixi.prefix.dev/latest/installation/). Instalacja nie wymaga uprawnień administratora i jest wykonywana dla pojednyczego użytkownika systemu (dla systemu Windows jest również zwykły instalator MSI, ale uruchamianie takich instalatorów często jest zablokowane np. w pracowniach komputerowych).

Po instalacji zwykle trzeba terminal uruchomić od nowa.

## 2. Tworzenie projektu (i środowiska)

Utwórz nowy projekt wykonując kolejno polecenia:

```bash
pixi init moj_projekt
cd moj_projekt
```
Polecenie `pixi init` tworzy katalog projektu. Wszystkie informacje o środowisku będą umieszczane wewnątrz tego katalogu. Podstawowe informacje o projekcie i środowisku zapisane są w utworzonym w nim pliku `pixi.toml`.

Można też utworzyć najpierw katalog projektu, a potem w utworzonym katalogu wykonać `pixi init` (bez nazwy projektu):
```bash
mkdir moj_projekt
cd moj_projekt
pixi init
```

## 3. Uruchamianie środowiska

Polecenie

```bash
pixi shell
```

w katalogu projektu, uruchamia nowy program interpretera poleceń z aktywowanym środowiskiem. W PowerShellu czasem to polecenie nie działa ze względu na pewne ustawienia (typowe w pracowniach komputerowych). Można wtedy po prostu uruchomić zwykły interpreter poleceń za pomocą polecenia `cmd`.

## 4. Instalowanie pakietów

Pixi domyślnie pobiera pakiety z [**Conda-forge**](https://conda-forge.org/) &mdash; społecznościowego zbioru pakietów dla ekosystemu conda. Pakiety mogą zawierać nie tylko biblioteki w języku Python, ale również inne narzędzia, np. skompilowane biblioteki napisane w C lub w C++ bądź same kompilatory czy interpretery. Instalacja pakietów mających tego typu zależności za pomocą Pixi jest zwykle dużo prostsza niż instalacja za pomocą innych sposobów (np. Pip).

Pakiety instaluje się za poleceniem `pixi add`. Polecenie to można wykonać w katalogu projektu również bez aktywowanego środowiska .

### Przykład
Polecenie
```bash
pixi add gdal
```

instaluje pakiet [`gdal`](https://gdal.org/) zawierający różne narzędzia do przetwarzania danych przestrzennych. Są to różne [programy wiersza polecenia](https://gdal.org/en/stable/programs/index.html) oraz [moduł w języku Python](https://gdal.org/en/stable/api/python/index.html).

Informacja o zainstalowanym pakiecie zapisywana jest w `pixi.toml`.

Wszystkie rzeczy instalowane przez Pixi umieszczane są w podkatalogu projektu nazwanym `.pixi`. W celu usunięcia niepotrzebnego już środowiska wystarczy usunąć ten katalog.

Uruchomienie
```bash
pixi install
```
w katalogu z plikiem `pixi.toml` ale bez zainstalowanego środowiska (bez podkatalogu `.pixi`, np. po jego usunięciu lub po przeniesieniu pliku `pixi.toml` na inny komputer lub do innego katalogu) spowoduje zainstalowanie całego środowiska. Aktywowanie środowiska (poleceniem `pixi-shell`) również w takim przypadku zainstaluje całe środowisko.

Instalacja pakietu powoduje zainstalowanie również tzw. *zależności* &mdash; innych pakietów od których zależy działanie tego pakietu. W przypadku pakietu `gdal` taką zależnością jest m.in. interpreter języka Python.

## 5. Sprawdzenie instalacji

Możemy sprawdzić jaka jest teraz ścieżka interpretera Pythona. Powinna być to ścieżka z aktywowanego środowiska, czyli 

Pod Linuksem:
```bash
which python
```

Pod Windowsem:
```bash
where python
```
Polecenie `where` może nie działać w PowerShellu. może wyświetlić 

Ścieżka powinna wskazywać na plik wewnątrz katalogu `.pixi`.

Pakiet `gdal` zawiera też różne programy do przetwarzania danych przestrzennych. Możemy np. sprawdzić czy działa polecenie `gdalinfo`:
```bash
gdalinfo --version
```
wyświetlające wersję GDAL.


## 6. Sprawdzenie modułu w Pythonie

Uruchom interpreter Pythona:

```bash
python
```

Następnie w interpreterze wypróbuj:

```python
>>> from osgeo import gdal
>>> print(gdal.__version__)
```

## 7. Wyjście ze środowiska

Wyjście ze środowiska polega po prostu na wyjściu z programu wiersza polecenia (terminala) w którym aktywowane jest środowisko (jeżeli uruchomiliśmy interpreter Pythona, to najpierw musimy wyjść też z niego), np. poleceniem:

```bash
exit
```

## 8. Porządki i odinstalowanie

Pixi przechowuje wszystkie ściągnięte przez siebie pakiety w katalogu (globalny cache pixi). Dzięki temu ponowna instalacja pakietu np. po usunięciu go ze środowiska, lub instalacja w innym projekcie nie wymaga ponownego ściągnięcia go, co czasem mocno wszystko mocno przyspiesza. Natomiast dane z tego katalogu nie są niezbędne do działania Pixi lub istniejących środowisk.

Zawartość katalogu cache może osiągnąć czasem spore rozmiary. Wyczyścić globalny cache pixi (np. gdy brakuje miejsca na dysku lub przed odinstalowaniem Pixi) można poleceniem:

```bash
pixi cache clean
```

Dokładniejsze instrukcje odinstalowania Pixi są w oficjalnej dokumentacji dotyczącej instalacji w sekcji [Uninstall](https://pixi.prefix.dev/latest/installation/#uninstall).

