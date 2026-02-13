# Kod QR KSeF na fakturze PDF

### KSeF Integrator addon **[WHMCS](https://puqcloud.com/link.php?id=77)**
#####  [Order now](https://puqcloud.com/whmcs-addon-puq-ksef-integrator.php) | [Download](https://download.puqcloud.com/WHMCS/addons/PUQ_WHMCS-KSeF-Integrator/) | [FAQ](https://community.puqcloud.com/)

Po przyjciu faktury przez KSeF generowany jest URL weryfikacyjny z kodem QR. Mozesz osadzic ten kod QR w swoim niestandardowym szablonie PDF faktury WHMCS, tak aby kazda wydrukowana faktura zawierala skanowalny link weryfikacyjny KSeF.

![Przyklad faktury PDF z kodem QR KSeF, numerem KSeF i linkiem weryfikacyjnym](../img/20-faktura-pdf-qr.png)
*20-faktura-pdf-qr.png*

---

## Wymagania wstpne

- Modul PUQ KSeF Integrator musi byc aktywny
- Faktura musi byc przyjta przez KSeF (status: accepted)
- Rozszerzenie PHP GD musi byc zainstalowane (do generowania obrazu QR)

---

## 1. Znajdz swoj szablon faktury

Szablony PDF faktur WHMCS znajduja sie zazwyczaj w:

```
templates/[template_name]/   (WHMCS root)
  invoicepdf.tpl      — szablon Smarty
  lub niestandardowa klasa .php PDF
```

---

## 2. Dolacz helper i pobierz dane

Dodaj ponizszy kod na poczatku szablonu PDF (lub w miejscu, gdzie potrzebujesz danych KSeF):

```php
require_once ROOTDIR . '/modules/addons/puq_ksef_integrator/lib/puqKSeFIntegratorPdfHelper.php';
$ksef = puq_ksef_get_invoice_data($invoiceid);
```

---

## 3. Parametry funkcji

Funkcja przyjmuje opcjonalne parametry kontrolujace generowanie kodu QR:

```php
puq_ksef_get_invoice_data(int $invoiceId, int $qrSize = 4, int $qrMargin = 2): array
```

| Parametr | Typ | Domyslnie | Opis |
|----------|-----|-----------|------|
| `$invoiceId` | int | — | ID faktury WHMCS |
| `$qrSize` | int | 4 | Rozmiar piksela na modul QR. Wieksza wartosc = wiekszy obraz QR. |
| `$qrMargin` | int | 2 | Strefa ciszy wokol kodu QR w modulach (biala ramka). |

Przyklad — wiekszy QR z minimalna ramka:

```php
$ksef = puq_ksef_get_invoice_data($invoiceid, 6, 1);
```

---

## 4. Dostpne zmienne

Funkcja zwraca tablice asocjacyjna z nastpujacymi kluczami:

| Klucz | Typ | Opis |
|-------|-----|------|
| `ksef_found` | bool | `true` jesli faktura zostala znaleziona w KSeF ze statusem accepted |
| `ksef_number` | string | Numer KSeF faktury (np. `5252834345-20260211-0100001BC403-59`) |
| `ksef_reference` | string | Numer referencyjny KSeF |
| `ksef_status` | string | Status KSeF (zawsze `accepted` gdy `ksef_found=true`) |
| `ksef_qr_url` | string | Pelny URL weryfikacyjny KSeF dla kodu QR |
| `ksef_qr_png_base64` | string | Obraz PNG kodu QR zakodowany jako string base64 |
| `ksef_qr_png_data_uri` | string | Kod QR jako data URI (`data:image/png;base64,...`) do uzycia w HTML |
| `ksef_invoice_hash` | string | Hash faktury z KSeF |
| `ksef_issue_date` | string | Data wystawienia faktury (YYYY-MM-DD) |
| `ksef_accepted_at` | string | Data/godzina przyjcia faktury przez KSeF |

---

## 5. Przyklad A — z plikiem tymczasowym

Zapisuje QR jako tymczasowy plik PNG, przekazuje go do TCPDF, a nastpnie usuwa:

```php
// --- Blok kodu QR KSeF (dodaj na dole szablonu PDF) ---
require_once ROOTDIR . '/modules/addons/puq_ksef_integrator/lib/puqKSeFIntegratorPdfHelper.php';
$ksef = puq_ksef_get_invoice_data($invoiceid);

if ($ksef['ksef_found'] && $ksef['ksef_qr_png_base64']) {
    // Zapisz QR PNG do pliku tymczasowego
    $qrTmpFile = tempnam(sys_get_temp_dir(), 'ksef_qr_');
    file_put_contents($qrTmpFile, base64_decode($ksef['ksef_qr_png_base64']));

    // Wycentruj obraz QR (30x30mm) poziomo
    $pageWidth = $pdf->getPageWidth();
    $qrSize = 30;
    $qrX = ($pageWidth - $qrSize) / 2;
    $qrY = $pdf->GetY() + 5;
    $pdf->Image($qrTmpFile, $qrX, $qrY, $qrSize, $qrSize, 'PNG');

    // Numer KSeF wycentrowany pod QR
    $pdf->SetY($qrY + $qrSize + 2);
    $pdf->SetFont('', 'B', 8);
    $pdf->Cell(0, 5, 'KSeF: ' . $ksef['ksef_number'], 0, 1, 'C');

    // Bezposredni link KSeF wycentrowany pod numerem
    if (!empty($ksef['ksef_qr_url'])) {
        $pdf->SetFont('', '', 7);
        $pdf->Cell(0, 4, $ksef['ksef_qr_url'], 0, 1, 'C', false, $ksef['ksef_qr_url']);
    }

    // Usun plik tymczasowy
    @unlink($qrTmpFile);
}
```

---

## 6. Przyklad B — bez pliku tymczasowego (base64_decode)

Uzywa prefiksu `@` TCPDF do przekazania surowych danych obrazu bezposrednio — bez pliku tymczasowego:

```php
// --- Blok kodu QR KSeF — bez pliku tymczasowego ---
require_once ROOTDIR . '/modules/addons/puq_ksef_integrator/lib/puqKSeFIntegratorPdfHelper.php';
$ksef = puq_ksef_get_invoice_data($invoiceid);

if ($ksef['ksef_found'] && $ksef['ksef_qr_png_base64']) {
    // Przekaz surowe dane PNG bezposrednio przez prefiks '@'
    $qrRaw = '@' . base64_decode($ksef['ksef_qr_png_base64']);

    // Wycentruj obraz QR (30x30mm) poziomo
    $pageWidth = $pdf->getPageWidth();
    $qrSize = 30;
    $qrX = ($pageWidth - $qrSize) / 2;
    $qrY = $pdf->GetY() + 5;
    $pdf->Image($qrRaw, $qrX, $qrY, $qrSize, $qrSize, 'PNG');

    // Numer KSeF wycentrowany pod QR
    $pdf->SetY($qrY + $qrSize + 2);
    $pdf->SetFont('', 'B', 8);
    $pdf->Cell(0, 5, 'KSeF: ' . $ksef['ksef_number'], 0, 1, 'C');

    // Bezposredni link KSeF wycentrowany pod numerem
    if (!empty($ksef['ksef_qr_url'])) {
        $pdf->SetFont('', '', 7);
        $pdf->Cell(0, 4, $ksef['ksef_qr_url'], 0, 1, 'C', false, $ksef['ksef_qr_url']);
    }
}
```

---

## 7. Przyklad C — plik tymczasowy z automatycznym fallbackiem

Probuje podejscia z plikiem tymczasowym; jesli katalog tymczasowy nie jest zapisywalny, przechodzi na metode `@`:

```php
// --- Blok kodu QR KSeF — plik tymczasowy z fallbackiem ---
require_once ROOTDIR . '/modules/addons/puq_ksef_integrator/lib/puqKSeFIntegratorPdfHelper.php';
$ksef = puq_ksef_get_invoice_data($invoiceid);

if ($ksef['ksef_found'] && $ksef['ksef_qr_png_base64']) {
    $qrPngData = base64_decode($ksef['ksef_qr_png_base64']);
    $qrImageArg = null;
    $qrTmpFile = null;

    // Najpierw probuj plik tymczasowy
    $qrTmpFile = @tempnam(sys_get_temp_dir(), 'ksef_qr_');
    if ($qrTmpFile && @file_put_contents($qrTmpFile, $qrPngData)) {
        $qrImageArg = $qrTmpFile;
    } else {
        // Fallback: przekaz surowe dane przez prefiks '@'
        $qrImageArg = '@' . $qrPngData;
        $qrTmpFile = null;
    }

    // Wycentruj obraz QR (30x30mm) poziomo
    $pageWidth = $pdf->getPageWidth();
    $qrSize = 30;
    $qrX = ($pageWidth - $qrSize) / 2;
    $qrY = $pdf->GetY() + 5;
    $pdf->Image($qrImageArg, $qrX, $qrY, $qrSize, $qrSize, 'PNG');

    // Numer KSeF wycentrowany pod QR
    $pdf->SetY($qrY + $qrSize + 2);
    $pdf->SetFont('', 'B', 8);
    $pdf->Cell(0, 5, 'KSeF: ' . $ksef['ksef_number'], 0, 1, 'C');

    // Bezposredni link KSeF wycentrowany pod numerem
    if (!empty($ksef['ksef_qr_url'])) {
        $pdf->SetFont('', '', 7);
        $pdf->Cell(0, 4, $ksef['ksef_qr_url'], 0, 1, 'C', false, $ksef['ksef_qr_url']);
    }

    // Usun plik tymczasowy jesli uzyty
    if ($qrTmpFile) {
        @unlink($qrTmpFile);
    }
}
```

---

## Uwagi

- Przyklad A tworzy plik tymczasowy, poniewaz metoda `Image()` TCPDF normalnie wymaga sciezki do pliku. Plik jest usuwany natychmiast po uzyciu.
- Przyklady B i C uzywaja prefiksu `@` — TCPDF akceptuje surowe dane obrazu w ten sposob, bez dostepu do systemu plikow.
- Jesli modul nie jest aktywny lub faktura nie jest w KSeF, funkcja zwraca bezpieczne wartosci domyslne (`ksef_found=false`, puste stringi) — nie zostanie zgloszony zaden blad.

---

## Rozwiazywanie problemow

- **Nic sie nie pojawia i brak bledow** — sprawdz, czy faktura zostala przyjeta przez KSeF (status: accepted). Funkcja zwraca dane tylko dla zaakceptowanych faktur.
- **Upewnij sie, ze uzywasz prawidlowej zmiennej:** `$invoiceid` (wewnetrzny ID faktury WHMCS, dostepny w szablonach PDF).
- **Aby debugowac**, tymczasowo dodaj przed blokiem `if`:

```php
require_once ROOTDIR . '/modules/addons/puq_ksef_integrator/lib/puqKSeFIntegratorPdfHelper.php';
$ksef = puq_ksef_get_invoice_data($invoiceid);

// Tymczasowy debug — usun po testowaniu
error_log('PUQ KSeF PDF debug: invoiceid=' . $invoiceid . ' ksef_found=' . var_export($ksef['ksef_found'], true) . ' status=' . $ksef['ksef_status']);
```
