# Changelog

### KSeF Integrator addon **[WHMCS](https://puqcloud.com/link.php?id=77)**
#####  [Order now](https://puqcloud.com/whmcs-addon-puq-ksef-integrator.php) | [Download](https://download.puqcloud.com/WHMCS/addons/PUQ_WHMCS-KSeF-Integrator/) | [FAQ](https://community.puqcloud.com/)

## v1.1 — 2026-03-28

### Nowe funkcje

- **Pomijanie faktur AddFunds** — nowa opcja w ustawieniach pozwala automatycznie pomijac faktury za doladowanie salda klienta. Doladowania nie sa rzeczywista sprzedaza i nie powinny byc raportowane w KSeF. Domyslnie wlaczone.

### Poprawki i ulepszenia

- **Poprawiona klasyfikacja nabywcy z UE** — ulepszone czyszczenie numeru EU VAT ID z niewidocznych znakow (BOM, zero-width space itp.), ktore mogly powodowac bledna klasyfikacje klientow unijnych jako osob fizycznych zamiast firm
- **Lepsza walidacja danych podatkowych** — pole `tax_id` (numer VAT) jest teraz agresywniej oczyszczane ze znakow specjalnych, co eliminuje problemy z danymi importowanymi lub kopiowanymi z zewnetrznych zrodel

---

## v1.0 — 2026-02-12

Pierwsza wersja modulu.

### Nowe funkcje

- Automatyczna wysylka faktur do KSeF po oplaceniu (tryb natychmiastowy)
- Reczna wysylka faktur do KSeF z poziomu panelu administracyjnego (tryb reczny)
- Generowanie e-Faktur w formacie FA(3) zgodnym z KSeF
- Uwierzytelnianie XAdES z certyfikatem kwalifikowanym
- Szyfrowanie faktur AES-256 przed wysylka
- Automatyczne pobieranie numeru KSeF i UPO po przyjciu faktury
- Generowanie kodow QR z linkiem weryfikacyjnym KSeF
- Obsluga srodowiska testowego i produkcyjnego
- Zarzadzanie certyfikatami (dodawanie, usuwanie, walidacja)
- Panel KSeF na stronie szczegolowej faktury w WHMCS
- Widget statystyk na dashboardzie administratora
- Sprawdzanie srodowiska (shell_exec, xmlsec1, OpenSSL, katalog tymczasowy)
- Testowanie polaczenia z serwerem KSeF
- Ponowna wysylka faktur odrzuconych lub z bledem
- Oznaczanie faktur jako nieoplacone (cofanie do statusu Draft)
- Helper PHP do umieszczania kodu QR KSeF na fakturze PDF
- Obsluga wielu walut z automatycznym przeliczaniem kursow
- Tryb debugowania z logowaniem do dziennika modulow WHMCS
- Interfejs w jezyku polskim i angielskim
- System licencjonowania z weryfikacja online/offline
