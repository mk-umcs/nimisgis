# Narzędzia wiersza polecenia, uruchamianie innych procesów

## Najprostsze mechanizmy interakcji programu ze swoim środowiskiem uruchomieniowym:

* argumenty wiersza polecenia &mdash; [Command-line interface &mdash; Wikipedia](https://en.wikipedia.org/wiki/Command-line_interface#Arguments),
* standardowe strumienie (wejścia, wyjścia, wyjścia błędów) &mdash; [Standard streams &mdash; Wikipedia](https://en.wikipedia.org/wiki/Standard_streams),
* kod wyjścia programu &mdash; [Exit status &mdash; Wikipedia](https://en.wikipedia.org/wiki/Exit_status),
* zmienne środowiskowe &mdash; [Environment variable &mdash; Wikipedia](https://en.wikipedia.org/wiki/Environment_variable).

## Zmienne i funkcje umożliwiające używanie powyższych mechanizmów w Pythonie:

* [`sys.argv`](https://docs.python.org/3/library/sys.html#sys.argv) &mdash; argumenty wiersza poleceń,
* [`sys.stdin`](https://docs.python.org/3/library/sys.html#sys.stdin), [`sys.stdout`](https://docs.python.org/3/library/sys.html#sys.stdout), [`sys.stderr`](https://docs.python.org/3/library/sys.html#sys.stderr) &mdash; standardowe wejście, wyjście i wyjście błędów,
* [`sys.exit`](https://docs.python.org/3/library/sys.html#sys.exit) &mdash; zakończenie programu z ustawionym kodem wyjścia,
* [`os.environ`](https://docs.python.org/3/library/os.html#os.environ) &mdash; zmienne środowiskowe.

## Uruchamianie programów w Pythonie:

* [`os.system`](https://docs.python.org/3/library/os.html#os.system) &mdash; uruchomienie programu; całe polecenie (program i argumenty) przekazane jest jako jeden napis; przydatne w prostych przypadkach, ale raczej należy unikać używania,
* [`subprocess.run`](https://docs.python.org/3/library/subprocess.html#subprocess.run) &mdash; uruchomienie programu; program i argumenty przekazane są jako lista (lub inna sekwencja); możliwość przekazania danych jako standardowe wejście i przechwycenia standardowego wyjścia oraz wyjścia błędów.
  
  **Wybrane parametry `supbrocess.run`:**
  
  * `stdout`, `stderr` &mdash; obiekty plikowe do których ma iść standardowe wyjście i wyjście błędów, wartość [`subprocess.PIPE`](https://docs.python.org/3/library/subprocess.html#subprocess.PIPE) oznacza, że wyjście będzie zapisane jako odpowiednie pola zwracanego obiektu,
  * `capture_output` &mdash; czy zapisywać wyjście w polach zwracanego obiektu,
  * `input` &mdash; dane przekazywane do uruchamianego programu przez standardowe wejście,
  * `encoding` &mdash; kodowanie tekstu w wyjściu programu; przydatne, jeżeli obiekty plikowe są otwarte w trybie tekstowym lub jeżeli pola w zwracanym obiekcie mają być napisami, a nie obiektami typu `bytes`,
  * `env` &mdash; zmienne środowiskowe uruchamianego programu; domyślnie dziedziczone jest środowisko programu uruchamiającego,
  * `timeout` &mdash; czas na wykonanie programu w sekundach; jeżeli proces nie zakończy się po zadanym czasie, to zostanie zatrzymany i zostanie wyrzucony wyjątek [`subprocess.TimeoutExpired`](https://docs.python.org/3/library/subprocess.html#subprocess.TimeoutExpired),
  * `check` &mdash; jeżeli ma wartość `True`, to w przypadku niezerowego kodu wyjścia zgłaszany jest wyjątek [`subprocess.CalledProcessError`](https://docs.python.org/3/library/subprocess.html#subprocess.CalledProcessError).

* [`subprocess.CompletedProcess`](https://docs.python.org/3/library/subprocess.html#subprocess.CompletedProcess) &mdash; klasa obiektu zwracanego przez `subprocess.run`
  
  **Wybrane pola obiektu `subprocess.CompletedProcess`:**

  * `returncode` &mdash; kod wyjścia procesu,
  * `stdout` &mdash; standardowe wyjście procesu (jako napis lub typ `bytes` &mdash; w zależności od użytych parametrów `subprocess.run`),
  * `stderr` &mdash; standardowe wyjście błędów procesu (jako napis lub typ bytes &mdash; w zależności od użytych parametrów `subprocess.run`).

* [`subprocess.CalledProcessError`](https://docs.python.org/3/library/subprocess.html#subprocess.CalledProcessError) &mdash; klasa wyjątku wyrzucanego przez `subprocess.run` w przypadku niezerowego kodu wyjścia i użytego argumentu `check=True`.
  
  Obiekt wyjątku `CalledProcessError` ma takie same pola jak obiekt `CompletedProcess` (m.in. `returncode`, `stdout`, `stderr`).