# Argumenty wiersza polecenia, kod wyjścia, zmienne środowiskowe

(args-and_name)=
1. Napisz program, który wyświetli swoją nazwę i argumenty wiersza polecenia. Np. dla wywołania

    ```bash
    $ python zad01.py a b "c d" e
    ```

    program powinien wyświetlić:

    ```text
    Nazwa programu: zad01.py
    Argument nr 1: a
    Argument nr 2: b
    Argument nr 3: c d
    Argument nr 4: e
    ```

(args-sum-2)=
2. Napisz program, który doda dwie liczby całkowite otrzymane w argumentach wiersza poleceń i wyświetli wynik na standardowym wyjściu.

    Jeżeli program został wywołany z inną liczbą argumentów niż 2, to powinien wyświetlić odpowiedni komunikat na standardowym wyjściu błędów i zwrócić kod wyjścia 1 (użyj [`sys.exit`](https://docs.python.org/3/library/sys.html#sys.exit) z argumentem będącym napisem). 

    Jeżeli argumenty nie są poprawnymi liczbami całkowitymi, to oprócz komunikatu na standardowym wyjściu błędów program powininen zwrócić kod błędu 2.

    Przykład użycia (polecenie `echo %errorlevel%` wyświetla kod wyjścia ostatnio wykonanego polecenia w systemie Windows, w Linuksie odpowiednikiem jest `echo $?`):

    ```bash
    $ python zad02.py 30 55
    85

    $ echo %errorlevel%
    0

    $ python zad02.py 30
    Program wymaga dokładnie dwóch argumentów.

    $ echo %errorlevel%
    1

    $ python zad02.py 123 abc
    Argumenty muszą być liczbami całkowitymi.

    $ echo %errorlevel%
    2
    ```

(args-sum)=
3. Zmodyfikuj program z {ref}`poprzedniego zadania <args-sum-2>` tak, żeby otrzymywać dowolną liczbę argumentów. Tym razem argumenty niebędące poprawnymi liczbami mają być ignorowane, ale ich liczba ma być zgłaszana jako kod wyjścia programu.

    Przykłady działania programu:

    ```bash
    $ python zad03.py 2 3
    5

    $ echo %errorlevel%
    0

    $ python zad03.py 10
    10

    $ python zad03.py
    0

    $ python zad03.py 1 2 3 abc 5 6 def
    17

    $ echo %errorlevel%
    2

    $ python zad03.py a b c d
    0

    $ echo %errorlevel%
    4
    ```

(env-vars)=
5. Napisz program, który wyświetla wartości zmiennych środowiskowych, których nazwy zadane są jako argumenty wiersza poleceń.

    Jeżeli, któraś ze zmiennych nie jest zdefiniowana, to informacja o tym wyświetlana jest na standardowym wyjściu błędów (ale wartości pozostałych są normalnie wyświetlane).
    
    Kod wyjścia programu jest równy 1 jeżeli któraś ze zmiennych nie była zdefiniowana.

(env-path)=
6. Napisz program, który wyświetli po kolei wszystkie katalogi wymienione w zmiennej środowiskowej `PATH`.

    Do sprawdzenia znaku separatora kolejnych ścieżek w tej zmiennej użyj wartości [`os.pathsep`](https://docs.python.org/3/library/os.html#os.pathsep).

(env-user)=
7. Napisz program, który spróbuje wyświetlić nazwę użytkownika i jego katalog domowy na podstawie wartości odpowiednich zmiennych środowiskowych.

    - W przypadku nazwy użytkownika program najpierw próbuje odczytać wartość zmiennej `USERNAME` (zazwyczaj przechowuje nazwę użytkownika w systemie Windows), a jeżeli zmienna ta nie jest zdefiniowana, to wartość zmiennej `USER` (to samo w systemie Linux).
    
    - Katalog domowy program próbuje odczytać kolejno ze zmiennych `USERPROFILE` (katalog domowy w systemie Windows) i `HOME` (katalog domowy w systemie Linux).

    Jeżeli nie uda się odczytać nazwy użytkownika lub katalogu (odpowiednie zmienne nie są zdefiniowane), to program wyświetla odpowiedni komunikat o błędzie. Kod wyjścia jest w takiej sytuacji różny od zera.