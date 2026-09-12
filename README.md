# Magento 2 API Enhancer — utrzymywany fork (SISL)

Rozszerza obsługę **cache dla REST API Magento 2 w oparciu o Varnish**: zarządzanie cache'owaniem
odpowiedzi API, **tagi cache** (precyzyjne unieważnianie po zmianie produktu/kategorii), procesory
klucza cache oraz obsługę uwierzytelnienia klienta dla cache'owanych odpowiedzi. Dla sklepów
headless/PWA i integracji, które mocno odpytują REST API, to sposób na odciążenie backendu przez
warstwę Varnish.

Część **MageSpecialist Security Suite / API tools**. To utrzymywany fork porzuconego
`msp/apienhancer` (ostatnie wydanie 2018, `php ^7.0|^7.1` — **nie wchodzi na żadne PHP 8.x**).
Ten fork rozluźnia zależności i jest zweryfikowany na **Magento 2.4.9 / PHP 8.4**
(di:compile, instancjacja wszystkich klas, wpięcie preferencji API).

## Zgodność
- Magento **2.4.4 – 2.4.9** (Open Source / Adobe Commerce)
- PHP **8.1 – 8.4**
- Wymaga `msp/common` (nasz fork) oraz działającego **Varnisha** do faktycznego cache'owania API

## Instalacja
```bash
composer config repositories.sisl-msp-common vcs https://github.com/SISL-source/magento2-msp-common
composer config repositories.sisl-apienhancer vcs https://github.com/SISL-source/magento2-api-enhancer
composer require msp/apienhancer:dev-main
bin/magento module:enable MSP_Common MSP_APIEnhancer
bin/magento setup:upgrade
bin/magento setup:di:compile   # tryb produkcyjny
```

## Jak działa
- `Model\VarnishManagement` / `VarnishTokenProcessor` — komunikacja z Varnishem (PURGE/BAN po tagach).
- `Model\Tag` + `Observer\Catalog*LoadAfter` — dokładanie tagów cache do odpowiedzi API na podstawie
  załadowanych produktów/kategorii, żeby unieważniać punktowo.
- `Model\CacheManagement` (`Api\CacheManagementInterface`) — sterowanie cache'owaniem odpowiedzi.
- `Model\CustomerAuth` — obsługa kontekstu klienta dla cache'owanych, spersonalizowanych odpowiedzi.

Konfiguracja adresu Varnisha i reguł — w panelu (sekcja MSP) oraz przez di.xml. Bez Varnisha moduł
instaluje się i nie psuje sklepu, ale realne cache'owanie API wymaga skonfigurowanego Varnisha.

## Zakres weryfikacji forka
Potwierdzone na 2.4.9 / PHP 8.4: `setup:upgrade`, czysty `di:compile`, instancjacja wszystkich klas
(w tym modeli Varnish), wpięcie preferencji API, brak wpływu na działanie REST API sklepu. Samo
purge'owanie cache Varnisha wymaga uruchomionego Varnisha do pełnego przetestowania w Twoim środowisku.

## Licencja
OSL-3.0 (jak oryginał). Fork utrzymywany przez [SISL](https://sisl.pl).
