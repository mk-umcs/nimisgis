# Uruchamianie programów

(run-gdalinfo)=
1. Napisz program, który uruchomi polecenie [`gdalinfo`](https://gdal.org/programs/gdalinfo.html) z argumentami wziętymi z argumentów wiersza poleceń programu.

   Program próbuje z danych wyświetlonych na standardowym wyjściu przez [`gdalinfo`](https://gdal.org/programs/gdalinfo.html) wyciągnąć informację o rozmiarze rastra (liczba wierszy, liczba kolumn) w zadanym pliku i wyświetlić te informacje w czytelnej postaci na swoim standardowym wyjściu.

   Jeżeli polecenia [`gdalinfo`](https://gdal.org/programs/gdalinfo.html) nie da się w ogóle uruchomić (nie został znaleziony odpowiedni plik wykonywalny), to program wyświetla odpowiedni komunikat i kończy działanie z niezerowym kodem wyjścia.

   Funkcja [`subprocess.run`](https://docs.python.org/3/library/subprocess.html#subprocess.run) wyrzuca wyjątek [`FileNotFoundError`](https://docs.python.org/3/library/exceptions.html#FileNotFoundError) jeżeli plik wykonywalny polecenia nie zostanie znaleziony.

   Jeżeli kod wyjścia polecenia [`gdalinfo`](https://gdal.org/programs/gdalinfo.html) jest różny od zera, to program ma po prostu wyświetlić standardowe wyjście polecenia [`gdalinfo`](https://gdal.org/programs/gdalinfo.html) na swoim standardowym wyjściu, standardowe wyjście błędów [`gdalinfo`](https://gdal.org/programs/gdalinfo.html) na swoim standardowym wyjściu błędów i zwrócić kod wyjścia [`gdalinfo`](https://gdal.org/programs/gdalinfo.html) jako swój kod wyjścia.

   **Przydatne zasoby:**
   * [polecenie `gdalinfo`](https://gdal.org/programs/gdalinfo.html)
   * [moduł `json`](https://docs.python.org/3/library/json.html)
   * [przykładowe pliki rastrowe](https://box.pionier.net.pl/d/bf9ba81bb43744ccbbbc/)

(run-sequence)=
2. Napisz program, który uruchomi kolejno następujący ciąg poleceń:

   ```
   gdalwarp dane/plik1.tif dane/plik2.tif ... tmp/dem.tif
   gdaldem slope tmp/dem.tif tmp/slope.tif
   gdalwarp -t_srs epsg:4326 tmp/slope.tif wyniki/slope4326.tif
   ```

   Polecenia te mają za zadanie na podstawie zbioru plików z numerycznym modelem terenu utworzyć jeden plik `slope4326.tif` z wartościami nachylenia dla obszaru opisywanego przez te pliki. Plik wynikowy ma używać układu odniesienia EPSG:4326.

   W pierwszym poleceniu ([`gdalwarp`](https://gdal.org/programs/gdalwarp.html)) fragment `dane/plik1.tif dane/plik2.tif ...` powinien być zastąpiony przez listę wszystkich plików z rozszerzeniem `.tif` z katalogu `dane`. Do stworzenia tej listy możesz np. użyć funkcji [`glob.glob`](https://docs.python.org/3/library/glob.html#glob.glob) w następujący sposób: `glob.glob("dane/*.tif")`.

   Nazwy katalogów w powyższych poleceniach powinny być zastąpione przez nazwy faktycznych katalogów. Nazwy katalogu z danymi oraz katalogu na wyniki przekazane są przez użytkownika w argumentach wiersza poleceń. Nazwa katalogu na pliki tymczasowe (`tmp`) powinna być zastąpiona przez rzeczywistą nazwę katalogu przeznaczonego do przechowywania plików tymczasowych w danym systemie (możesz użyć funkcji [`tempfile.gettempdir`](https://docs.python.org/3/library/tempfile.html#tempfile.gettempdir)).

   Pliki tymczasowe (`dem.tif` i `slope.tif`) powinny być usunięte po zakończeniu działania programu.

   Jeżeli wywołanie któregokolwiek z poleceń się nie powiedzie, to program ma wyświetlić odpowiedni komunikat na standardowym wyjściu błędów i zakończyć działanie z kodem błędu 1.

   **Przydatne zasoby:**
   * [polecenie `gdalwarp`](https://gdal.org/programs/gdalwarp.html)
   * [polecenie `gdaldem`](https://gdal.org/programs/gdaldem.html)
   * [funkcja `glob.glob`](https://docs.python.org/3/library/glob.html#glob.glob)
   * [funkcja `tempfile.gettempdir`](https://docs.python.org/3/library/tempfile.html#tempfile.gettempdir)
   * [przykładowe pliki z numerycznym modelem terenu](https://box.pionier.net.pl/d/f01c92f35504427d9e04/?p=/rastry/dem)

(run-sequence-size)=
3. Popraw program z {ref}`poprzedniego zadania <run-sequence>` tak, żeby dodatkowo wyświetlał informację o rozmiarach rastrów w utworzonych przez siebie plikach (`dem.tif`, `slope.tif`, `slope4326.tif`). Rozmiary rastrów mają być uzyskane za pomocą polecenia [`gdalinfo`](https://gdal.org/programs/gdalinfo.html) (tak jak w pierwszym zadaniu).

(run-ogrinfo)=
4. Polecenie [`ogrinfo`](https://gdal.org/programs/ogrinfo.html) (jeden z programów biblioteki GDAL) umożliwia geokodowanie. Np. polecenie

   ```
   ogrinfo :memory: -q -sql "SELECT ST_Centroid(ogr_geocode('Piotrków, województwo lubelskie'))"
   ```

   tzn. polecenie [`ogrinfo`](https://gdal.org/programs/ogrinfo.html) z czterema argumentami:
   * `:memory:`
   * `-q`
   * `-sql`
   * `SELECT ST_Centroid(ogr_geocode('Piotrków, województwo lubelskie'))`

   wyświetli na standardowym wyjściu

   ```
   Layer name: SELECT
   OGRFeature(SELECT):0
     POINT (22.6479285098317 51.0473462614526)
   ```

   Napisz program, który pobiera od użytkownika nazwę miejscowości (lub innego obiektu) w województwie lubelskim i wyświetli jego współrzędne geograficzne w postaci:

   ```
   Piotrków
     długość geograficzna:   22.6479285098317
     szerokość geograficzna: 51.0473462614526
   ```

   **Przydatne zasoby:**
   * [polecenie `ogrinfo`](https://gdal.org/programs/ogrinfo.html)

(run-gdaltransform)=
5. Polecenie [`gdaltransform`](https://gdal.org/programs/gdaltransform.html) umożliwia przeliczanie współrzędnych między układami odniesienia. Np. polecenie

   ```
   gdaltransform -s_srs EPSG:4326 -t_srs EPSG:2180
   ```

   oczekuje na wpisywanie przez użytkownika w kolejnych wierszach par współrzędnych w układzie WGS84 (długość, szerokość) oddzielonych spacją i wyświetla te współrzędne przekształcone na układ EPSG:2180 (też w kolejnych wierszach oddzielone spacją). Program kończy działanie, gdy skończą się dane na standardowym wejściu (w konsoli systemu Windows koniec pliku na standardowym wejściu można zasymulować wciskając Ctrl-Z, w systemie Linux — Ctrl-D).

   Napisz program, który otrzyma w argumentach wiersza poleceń parę współrzędnych geograficznych (WGS84) i wyświetli te współrzędne przekształcone na EPSG:2180. Program ma użyć polecenia [`gdaltransform`](https://gdal.org/programs/gdaltransform.html) do przeliczenia współrzędnych.

   Np. po uruchomieniu z argumentami `22.6479285098317` i `51.0473462614526`, program powinien wyświetlić:

   ```
   Współrzędne w układzie EPSG:2180:
     755600.21470691, 359724.360893924
   ```

   **Przydatne zasoby:**
   * [polecenie `gdaltransform`](https://gdal.org/programs/gdaltransform.html)

(zad-6)=
6. Połącz działanie poprzednich dwóch programów ({ref}`zadanie 4<run-ogrinfo>` i {ref}`zadanie 5 <run-gdaltransform>`), tzn. program ma pobierać od użytkownika nazwę obiektu w województwie lubelskim i wyświetlać jego współrzędne w układzie EPSG:2180.

   Jeżeli nie uda się uzyskać współrzędnych miejscowości lub którekolwiek z używanych poleceń zwróci kod błędu inny niż zero, to program powinien wyświetlić komunikat o tym na standardowym wyjściu błędów i zakończyć działanie z niezerowym kodem wyjścia.

   Program ma w przypadku nieznalezienia któregoś z plików do uruchomienia ([`ogrinfo`](https://gdal.org/programs/ogrinfo.html) lub [`gdaltransform`](https://gdal.org/programs/gdaltransform.html)) przerywać działanie z odpowiednim komunikatem na wyjściu błędów i kodem wyjścia różnym od zera (funkcja [`subprocess.run`](https://docs.python.org/3/library/subprocess.html#subprocess.run) wyrzuca wyjątek [`FileNotFoundError`](https://docs.python.org/3/library/exceptions.html#FileNotFoundError) jeżeli plik wykonywalny polecenia nie zostanie znaleziony).

(zad-7)=
7. W pliku [`lubelskie.tif`](https://box.pionier.net.pl/d/f01c92f35504427d9e04/files/?p=%2Frastry%2Flubelskie.tif) znajduje się numeryczny model terenu dla województwa lubelskiego w układzie odniesienia EPSG:2180. Napisz program, który dla pobranej od użytkownika nazwy miejscowości spróbuje wyciągnąć z pliku kwadratowy obszar o boku 25 km, którego środek znajduje się w zadanej miejscowości. Obszar ten ma być zapisany do pliku `miejscowosc.tif`, gdzie fragment `miejscowosc` ma być zastąpiony faktyczną nazwą miejscowości (np. `Lublin.tif`).

   Do pobrania współrzędnych miejscowości użyj odpowiednio polecenia [`ogrinfo`](https://gdal.org/programs/ogrinfo.html). Do przekształcenia współrzędnych na układ EPSG:2180 użyj polecenia [`gdaltransform`](https://gdal.org/programs/gdaltransform.html). Do wycięcia zadanego obszaru użyj polecenia [`gdal_translate`](https://gdal.org/programs/gdal_translate.html).

   Polecenia [`gdal_translate`](https://gdal.org/programs/gdal_translate.html) można użyć w następujący sposób

   ```
   gdal_translate -projwin ulx uly lrx lry lubelskie.tif miejscowosc.tif
   ```

   gdzie `ulx`, `uly`, `lrx`, `lry` są odpowiednio współrzędną *x* lewego górnego wierzchołka, współrzędną *y* lewego górnego wierzchołka, współrzędną *x* prawego dolnego wierzchołka i współrzędną *y* prawego dolnego wierzchołka wycinanego fragmentu. Współrzędne są w układzie odniesienia pliku.

   Jeżeli nie uda się uzyskać współrzędnych miejscowości lub którekolwiek z używanych poleceń zwróci kod błędu inny niż zero, to program powinien wyświetlić komunikat o tym na standardowym wyjściu błędów i zakończyć działanie z niezerowym kodem wyjścia.

   Program ma w przypadku nieznalezienia któregoś z plików do uruchomienia ([`ogrinfo`](https://gdal.org/programs/ogrinfo.html), [`gdaltransform`](https://gdal.org/programs/gdaltransform.html) lub [`gdal_translate`](https://gdal.org/programs/gdal_translate.html)) przerywać działanie z odpowiednim komunikatem na wyjściu błędów i kodem wyjścia różnym od zera.

   **Przydatne zasoby:**
   * [plik `lubelskie.tif`](https://box.pionier.net.pl/d/f01c92f35504427d9e04/files/?p=%2Frastry%2Flubelskie.tif)