# Jak to dziala — ogolne zasady

### KSeF Integrator addon **[WHMCS](https://puqcloud.com/link.php?id=77)**
#####  [Order now](https://puqcloud.com/whmcs-addon-puq-ksef-integrator.php) | [Download](https://download.puqcloud.com/WHMCS/addons/PUQ_WHMCS-KSeF-Integrator/) | [FAQ](https://community.puqcloud.com/)

## Schemat dzialania modulu

PUQ KSeF Integrator automatyzuje caly proces wysylki faktur do Krajowego Systemu e-Faktur. Ponizej opisano caly cykl zycia faktury w kontekscie integracji z KSeF.

---

## Przepyw przetwarzania faktury

### 1. Oplacenie faktury w WHMCS

Gdy faktura w WHMCS otrzymuje status **Paid** (oplacona), automatycznie uruchamiany jest hook `InvoicePaid`.

### 2. Zbieranie danych

Modul automatycznie:
- Pobiera dane faktury z WHMCS (numer, kwoty, pozycje, stawka VAT)
- Pobiera dane klienta (nazwa firmy / osoba fizyczna, NIP, adres)
- Przygotowuje dane sprzedawcy z konfiguracji modulu
- Waliduje dane (stawka VAT musi byc zgodna z KSeF: 0, 3, 4, 5, 7, 8, 22, 23%)
- Generuje plik XML faktury w formacie FA(3)

### 3. Zapis w kolejce

Faktura jest zapisywana w tabeli `puq_ksef_invoices` ze statusem:
- **oczekuje** (pending) — jesli dane sa prawidlowe
- **blad** (error) — jesli walidacja nie powiodla sie (np. nieprawidlowa stawka VAT)

### 4. Wysylka do KSeF

W zaleznosci od trybu wysylki:

**Tryb natychmiastowy:** Modul automatycznie wysyla fakture do KSeF zaraz po oplaceniu.

**Tryb reczny:** Faktura czeka w kolejce ze statusem "oczekuje". Administrator moze ja wyslac recznie z poziomu listy faktur lub panelu KSeF na stronie faktury.

### 5. Proces wysylki (7 krokow)

1. **Uwierzytelnienie XAdES** — modul uwierzytelnia sie w KSeF za pomoca certyfikatu kwalifikowanego
2. **Szyfrowanie sesji** — generowanie klucza AES-256 i szyfrowanie go kluczem publicznym RSA KSeF
3. **Otwarcie sesji** — otwarcie interaktywnej sesji online FA(3) w KSeF
4. **Szyfrowanie faktury** — faktura XML jest szyfrowana algorytmem AES-256-CBC
5. **Wysylka** — zaszyfrowana faktura jest przesylana do KSeF
6. **Sprawdzanie statusu** — modul odpytuje KSeF o status przetwarzania (do 10 prob, co 2 sekundy)
7. **Pobranie UPO i zamkniecie sesji** — po przyjciu faktury modul pobiera UPO i zamyka sesje

### 6. Statusy faktur

| Status | Opis |
|--------|------|
| **Oczekuje** (pending) | Faktura w kolejce, czeka na wysylke |
| **Przetwarzanie** (processing) | Faktura wyslana, czeka na odpowiedz KSeF |
| **Przyjeta** (accepted) | Faktura przyjeta przez KSeF, numer KSeF przypisany |
| **Odrzucona** (rejected) | KSeF odrzucil fakture (blad walidacji po stronie KSeF) |
| **Blad** (error) | Blad po stronie modulu (np. nieprawidlowa stawka VAT, blad polaczenia) |

### 7. Po przyjciu faktury

Gdy KSeF przyjmie fakture:
- Przypisywany jest unikalny **numer KSeF** (np. `5252834345-20260212-15321C400000-25`)
- Pobierane jest **UPO** (Urzedowe Poswiadczenie Odbioru)
- Generowany jest **link QR** do weryfikacji faktury
- Wszystkie dane sa zapisywane w bazie danych

---

## Obslugiwane typy nabywcow

Modul automatycznie rozpoznaje typ nabywcy na podstawie danych klienta:

| Typ | Warunek | Stawka VAT |
|-----|---------|------------|
| Firma krajowa | Kraj = PL, firma | Standardowa (np. 23%) |
| Osoba krajowa | Kraj = PL, osoba fizyczna | Standardowa (np. 23%) |
| Firma UE (B2B) | Kraj UE (nie PL), firma | Konfigurowalna (domyslnie 0% EX) |
| Osoba UE (B2C) | Kraj UE (nie PL), osoba fizyczna | Konfigurowalna |
| Poza UE | Kraj spoza UE | Konfigurowalna (domyslnie 0% EX) |

---

## Obslugiwane stawki VAT KSeF

KSeF akceptuje tylko nastepujace stawki VAT:

**0%, 3%, 4%, 5%, 7%, 8%, 22%, 23%**

Jesli faktura w WHMCS ma inna stawke VAT (np. 20%), modul zglosi blad walidacji i faktura nie zostanie wyslana.

---

## Obsluga walut obcych

Jesli faktura jest wystawiona w walucie innej niz PLN (np. USD, EUR):
- Modul automatycznie pobiera oficjalny kurs waluty z **Narodowego Banku Polskiego (NBP)** za posrednictwem API: `https://api.nbp.pl/api/exchangerates/rates/A/{waluta}/last/`
- Jest to oficjalny kurs sredni NBP — wymagany przez KSeF do prawidlowego przeliczenia kwot na PLN
- Kurs jest zapisywany w rekordzie faktury KSeF
- Na panelu KSeF wyswietlany jest kurs przeliczeniowy (np. "Kurs: 1 USD = 3.54 PLN")
