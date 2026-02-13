# Licencja

### KSeF Integrator addon **[WHMCS](https://puqcloud.com/link.php?id=77)**
#####  [Order now](https://puqcloud.com/whmcs-addon-puq-ksef-integrator.php) | [Download](https://download.puqcloud.com/WHMCS/addons/PUQ_WHMCS-KSeF-Integrator/) | [FAQ](https://community.puqcloud.com/)

## Jak dziala licencja

Modul PUQ KSeF Integrator wymaga aktywnej licencji do pelnego funkcjonowania. Licencja jest weryfikowana za posrednictwem serwera licencji PUQ Cloud.

### Weryfikacja licencji

- Modul okresowo sprawdza waznosc licencji na serwerze `https://license.puqcloud.com/`
- Wyniki weryfikacji sa cachowane w bazie danych, dzieki czemu modul nie wymaga stalego polaczenia z serwerem licencji
- Okres cachowania wynosi 5 dni — w tym czasie modul korzysta z zapisanego wyniku weryfikacji
- Jesli serwer licencji jest tymczasowo niedostepny, modul korzysta z ostatniego zapisanego wyniku

### Co jest wysylane do serwera licencji

Podczas weryfikacji modul wysyla:
- Klucz licencyjny
- Kod produktu
- Nazwe serwera (System URL z konfiguracji WHMCS)

---

## Ograniczenia bez aktywnej licencji

Gdy licencja jest nieaktywna lub wygasla, modul dziala w trybie ograniczonym:

### Co dziala bez licencji

- **Zbieranie faktur** — faktury sa nadal automatycznie zbierane i zapisywane w kolejce ze statusem "oczekuje" (pending) po oplaceniu w WHMCS
- **Przegladanie danych** — lista faktur, statystyki, podglad XML, pobieranie UPO
- **Sprawdzanie srodowiska** — strona diagnostyki srodowiska
- **Widget na dashboardzie** — statystyki faktur na stronie glownej

### Co jest zablokowane bez licencji

- **Wysylka faktur do KSeF** — przyciski "Wyslij do KSeF" i "Wyslij teraz" sa zablokowane
- **Ponowna wysylka** — przycisk "Ponow" dla faktur z bledem jest zablokowany
- **Automatyczna wysylka** — nawet w trybie "natychmiast po oplaceniu" faktury nie sa automatycznie wysylane, lecz zapisywane jako "oczekujace"
- **Zmiana ustawien** — zapis konfiguracji modulu jest zablokowany
- **Zarzadzanie certyfikatami** — dodawanie i usuwanie certyfikatow jest zablokowane
- **Oznaczanie jako nieoplacone** — przycisk "Oznacz nieoplacona" jest zablokowany

### Komunikaty

- Na kazdej stronie modulu wyswietlany jest czerwony baner z informacja o braku licencji i linkiem do zakupu
- Na dashboardzie WHMCS wyswietlane jest ostrzezenie
- Przy probie wykonania zablokowanej akcji uzytkownik otrzymuje komunikat: *"License is not active. Please activate your license to perform this action."*

---

## Po aktywacji licencji

Po wpisaniu prawidlowego klucza licencyjnego i zapisaniu konfiguracji:

1. Baner z ostrzezeniem znika
2. Wszystkie funkcje modulu sa dostepne
3. Faktury ze statusem "oczekuje" (pending), ktore zebraly sie podczas okresu bez licencji, moga byc wyslane recznie przyciskiem "Wyslij teraz" lub ponownie przetworzone

---

## Zakup licencji

Licencje mozna zakupic na stronie:

**[https://panel.puqcloud.com/index.php/store/whmcs-addon/ksef-integrator](https://panel.puqcloud.com/index.php/store/whmcs-addon/ksef-integrator)**

W razie pytan dotyczacych licencji prosimy o kontakt przez system zgloszeniowy:

**[https://puqcloud.com/submitticket.php](https://puqcloud.com/submitticket.php?step=2&deptid=1)**
