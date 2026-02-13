# Opis

### KSeF Integrator addon **[WHMCS](https://puqcloud.com/link.php?id=77)**
#####  [Order now](https://puqcloud.com/whmcs-addon-puq-ksef-integrator.php) | [Download](https://download.puqcloud.com/WHMCS/addons/PUQ_WHMCS-KSeF-Integrator/) | [FAQ](https://community.puqcloud.com/)

## PUQ KSeF Integrator — dodatek WHMCS

**PUQ KSeF Integrator** to zaawansowany modul dodatkowy (addon) dla systemu WHMCS, ktory automatyzuje komunikacje z **Krajowym Systemem e-Faktur (KSeF)** prowadzonym przez Ministerstwo Finansow RP.

Modul automatycznie generuje i wysyla ustrukturyzowane e-Faktury w formacie **FA(3)** po oplaceniu faktury w WHMCS, pobiera numery KSeF oraz UPO (Urzedowe Poswiadczenie Odbioru), obsluguje kody QR i prawidlowo przetwarza kompensacje salda kredytowego.

---

## Glowne funkcje

- **Automatyczna wysylka faktur do KSeF** — po oplaceniu faktury w WHMCS modul automatycznie generuje XML FA(3) i wysyla do KSeF
- **Zgodnosc z formatem FA(3)** — pelna zgodnosc ze struktura e-Faktury wymagana przez KSeF
- **Obsluga kompensacji salda kredytowego** — prawidlowe przetwarzanie faktur z czesciowa lub pelna kompensacja
- **Automatyczne pobieranie numeru KSeF i UPO** — po przyjciu faktury przez KSeF modul pobiera numer KSeF oraz UPO
- **Kody QR na fakturach PDF** — mozliwosc umieszczenia kodu QR z linkiem weryfikacyjnym KSeF na fakturze PDF
- **Srodowisko testowe i produkcyjne** — obsluga obu srodowisk KSeF z osobnymi certyfikatami
- **Szczegolowe logowanie komunikacji API** — tryb debugowania z rejestrowaniem kazdego kroku w dzienniku WHMCS
- **Obsluga wielu walut** — automatyczne przeliczanie kursow walut dla faktur w walutach obcych
- **Panel KSeF na stronie faktury** — szczegolowy panel z informacjami KSeF bezposrednio na stronie faktury w panelu administracyjnym
- **Widget na dashboardzie** — statystyki faktur KSeF na stronie glownej panelu administracyjnego
- **Zarzadzanie certyfikatami** — bezpieczne przechowywanie certyfikatow XAdES z szyfrowaniem
- **Wielojezycznosc** — polski i angielski interfejs

---

## Wymagania systemowe

| Wymaganie | Minimum |
|-----------|---------|
| WHMCS | 8.x |
| PHP | 7.4+ |
| OpenSSL | >= 1.1.1 |
| xmlsec1 | >= 1.2.31 |
| shell_exec | wlaczone |
| Rozszerzenie PHP GD | wymagane (do generowania QR) |
| Katalog tymczasowy | zapisywalny |

---

## Srodowiska KSeF

| Srodowisko | API URL | QR URL |
|------------|---------|--------|
| Testowe | `https://ksef-test.mf.gov.pl` | `https://qr-test.ksef.mf.gov.pl` |
| Produkcyjne | `https://ksef.mf.gov.pl` | `https://qr.ksef.mf.gov.pl` |

---

## Linki

- **Strona produktu:** [https://puqcloud.com/](https://puqcloud.com/)
- **Dokumentacja:** [https://doc.puq.info/books/ksef-integrator-whmcs-addon](https://doc.puq.info/books/ksef-integrator-whmcs-addon)
- **Wsparcie:** [https://puqcloud.com/submitticket.php](https://puqcloud.com/submitticket.php?step=2&deptid=1)
- **Spolecznosc:** [https://community.puqcloud.com/](https://community.puqcloud.com/)
