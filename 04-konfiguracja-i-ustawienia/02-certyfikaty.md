# Certyfikaty

### KSeF Integrator addon **[WHMCS](https://puqcloud.com/link.php?id=77)**
#####  [Order now](https://puqcloud.com/whmcs-addon-puq-ksef-integrator.php) | [Download](https://download.puqcloud.com/WHMCS/addons/PUQ_WHMCS-KSeF-Integrator/) | [FAQ](https://community.puqcloud.com/)

Strona zarzadzania certyfikatami dostepna jest w: **Addons** > **PUQ KSeF Integrator** > **Ustawienia** > **Certyfikaty**

![Lista certyfikatow z informacjami o waznosci](../img/08-certyfikaty-lista.png)
*08-certyfikaty-lista.png*

Modul wymaga certyfikatow do uwierzytelniania w systemie KSeF za pomoca podpisu XAdES. Kazde srodowisko (testowe i produkcyjne) wymaga osobnego certyfikatu.

---

## Typy certyfikatow

### Certyfikat testowy

Certyfikat testowy sluzy do pracy w srodowisku testowym KSeF (`ksef-test.mf.gov.pl`). Jest wydawany przez Ministerstwo Finansow i mozna go wygenerowac w aplikacji testowej KSeF.

**Gdzie uzyskac certyfikat testowy:**

1. Przejdz do strony aplikacji testowej KSeF: [https://ap-test.ksef.mf.gov.pl/web/](https://ap-test.ksef.mf.gov.pl/web/)
2. Zaloguj sie lub utworz konto testowe
3. W ustawieniach konta wygeneruj certyfikat testowy
4. Pobierz plik certyfikatu (PEM) i klucz prywatny (PEM)

### Certyfikat produkcyjny

Certyfikat produkcyjny to kwalifikowany certyfikat elektroniczny uzywany w srodowisku produkcyjnym KSeF (`ksef.mf.gov.pl`).

**Gdzie uzyskac certyfikat produkcyjny:**

1. Przejdz do strony aplikacji produkcyjnej KSeF: [https://ap.ksef.mf.gov.pl/web/](https://ap.ksef.mf.gov.pl/web/)
2. Zaloguj sie za pomoca profilu zaufanego lub podpisu kwalifikowanego
3. W ustawieniach konta firmy skonfiguruj uprawnienia do wysylki faktur
4. Uzyj certyfikatu kwalifikowanego wydanego przez certyfikowane centrum certyfikacji

---

## Dodawanie certyfikatu

1. Na stronie certyfikatow kliknij przycisk **+ Dodaj certyfikat**
2. Wypelnij formularz:

| Pole | Opis | Wymagane |
|------|------|----------|
| **Nazwa** | Dowolna nazwa identyfikujaca certyfikat (np. "TEST", "Produkcja 2026") | Tak |
| **Typ** | Testowy lub Produkcyjny | Tak |
| **Certyfikat PEM** | Zawartosc pliku certyfikatu w formacie PEM (zaczyna sie od `-----BEGIN CERTIFICATE-----`) | Tak |
| **Klucz PEM** | Zawartosc pliku klucza prywatnego w formacie PEM (zaczyna sie od `-----BEGIN PRIVATE KEY-----` lub `-----BEGIN RSA PRIVATE KEY-----` lub `-----BEGIN ENCRYPTED PRIVATE KEY-----`) | Tak |
| **Haslo klucza** | Haslo do klucza prywatnego, jesli klucz jest zaszyfrowany | Nie |

3. Kliknij **Zapisz certyfikat**

![Formularz dodawania certyfikatu](../img/09-certyfikaty-dodaj.png)
*09-certyfikaty-dodaj.png*

> **Wazne:** Certyfikaty sa przechowywane w bazie danych w postaci zaszyfrowanej. Szyfrowanie odbywa sie za pomoca mechanizmu WHMCS `encrypt/decrypt`.

---

## Lista certyfikatow

Tabela wyswietla wszystkie zapisane certyfikaty z informacjami:

| Kolumna | Opis |
|---------|------|
| **ID** | Unikalny identyfikator certyfikatu |
| **Nazwa** | Nazwa nadana przy dodawaniu |
| **Typ** | TESTOWY / PRODUKCYJNY (kolorowe etykiety) |
| **Wystawca** | Nazwa wystawcy certyfikatu (np. "Ministerstwo Finansow") |
| **Podmiot** | Nazwa podmiotu, dla ktorego wydano certyfikat |
| **Wazny od** | Data poczatku waznosci certyfikatu |
| **Wazny do** | Data konca waznosci certyfikatu |
| **Wygasly** | WAZNY (zielony) lub WYGASLY (czerwony) |
| **Akcje** | Przycisk usuwania certyfikatu |

---

## Usuwanie certyfikatu

1. Kliknij czerwony przycisk usuwania w kolumnie **Akcje**
2. Potwierdz usuniecie w oknie dialogowym

> **Uwaga:** Nie mozna usunac certyfikatu, ktory jest aktualnie wybrany w konfiguracji (Certyfikat testowy lub Certyfikat produkcyjny). Najpierw zmien certyfikat w konfiguracji.

---

## Przypisanie certyfikatu do srodowiska

Po dodaniu certyfikatu:

1. Przejdz do **Ustawienia** > **Konfiguracja**
2. W sekcji "Srodowisko i certyfikaty" wybierz odpowiedni certyfikat z listy:
   - **Certyfikat testowy** — dla srodowiska testowego
   - **Certyfikat produkcyjny** — dla srodowiska produkcyjnego
3. Kliknij **Zapisz ustawienia**
