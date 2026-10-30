# Changelog

Tutte le modifiche rilevanti a questo modulo sono documentate in questo file.

Il formato è basato su [Keep a Changelog](https://keepachangelog.com/it/1.1.0/)
e questo progetto aderisce al [Semantic Versioning](https://semver.org/lang/it/).

## [1.1.0] - 2026-10-14

### Added
- Validazione lato server dei dati fattura quando il cliente richiede la fattura (`Model/Validator/InvoiceDataValidator.php`, puro, + `Model/InvoiceDataValidation.php`):
  - privato: Codice Fiscale persona fisica obbligatorio (16 caratteri, omocodia e carattere di controllo);
  - azienda/professionista: Partita IVA obbligatoria (11 cifre con cifra di controllo, prefisso `IT` e spazi rimossi), CF facoltativo (16 caratteri o numerico 11);
  - PEC (se indicata) email valida; Codice destinatario SDI (se indicato) 7 caratteri alfanumerici (6 per la PA);
  - regole italiane (CF/P.IVA/SDI) applicate solo con paese di fatturazione IT; valori normalizzati (CF/SDI maiuscoli).
- Punti di controllo: before plugin su `PaymentInformationManagement` e `GuestPaymentInformationManagement::savePaymentInformationAndPlaceOrder` (errore REST leggibile, checkout Breeze/Luma) e before plugin su `QuoteManagement::submit` come rete di sicurezza per tutti i canali (GraphQL Hyvä/Magewire, admin).
- Messaggi d'errore traducibili (`i18n/it_IT.csv`). README.

### Fixed
- Il flag "Enable Module" ora è rispettato ovunque: plugin pagamento/billing, observer quote→ordine, `LayoutProcessor` del checkout Luma, plugin visibilità P.IVA (torna al comportamento core), override dei template registrazione/indirizzo (`ifconfig` sul `setTemplate`: da disabilitato si usano i template core, che mostrano di nuovo azienda e P.IVA), JS di validazione inerte se i campi fattura non sono presenti.
- `PaymentInformationInterceptor`: rimosso il log di debug che scriveva i dati fattura (CF) nel log `asf`.
- `CopyQuoteToOrder`: rimosso il `save()` anticipato dell'indirizzo ordine (poteva lasciare righe orfane in `sales_order_address` se il place falliva); usa il quote dell'evento.
- `LayoutProcessor`: `afterProcess()` restituiva `null` con valori di `display_billing_address_on` diversi da 0/1; protetto l'accesso a `payments-list` mancante.
- Template indirizzo My Account: `company` e `vat_id` stampati senza escape (XSS); etichette escapate; campo P.IVA `type="text"` (con `number` si perdevano gli zeri iniziali), PEC `type="email"`.
- Template admin "Invoice Information": etichette escapate.
- Mixin JS checkout: copiavano tutti i custom attribute dell'indirizzo in `extension_attributes` (attributi di altri moduli non dichiarati facevano fallire la REST con errore di input); ora solo i campi fattura.
- JS di validazione: controllo cifra di controllo P.IVA, SDI a 6/7 caratteri, traduzione "Sdi code not valid.".
- `Helper/Data`: rimosse proprietà tipizzate inutilizzate con classi non importate; `isEnabled()` accetta lo store id.

### Changed
- `composer.json`: rimossa la `version` hardcoded (la versione la dà il tag).

## [1.0.5] - 2026-07-08

### Fixed
- ACL: il file era in `etc/adminhtml/acl.xml` (percorso non letto da Magento, che carica gli ACL solo da `etc/acl.xml`) e usava come root `<acl>` invece di `<config><acl>`. Di conseguenza la risorsa `Threecommerce_ItalianInvoice::configuration` referenziata da `system.xml` non veniva registrata nell'albero ACL. Spostato in `etc/acl.xml` con la struttura `<config>` corretta.

## [1.0.4] - 2026-07-08

### Fixed
- Compatibilità PHP 8.4: parametro `$billingAddress` di `beforeSavePaymentInformationAndPlaceOrder()` in `Model/PaymentInformationInterceptor.php` reso nullable esplicito (`?AddressInterface`). Assorbe la patch composer PHP 8.4 che il progetto store applicava su questo modulo.

## [1.0.3]

## [1.0.2]

## [1.0.1]

## [1.0.0]
