# Changelog

## [Unreleased] — fork SISL (2026-09)
- Zgodność z Magento **2.4.9 / PHP 8.4** (ostatni publiczny `msp/apienhancer` z 2018 pinuje `php ^7.0|^7.1` = blok całego 8.x).
- `composer.json`: `php ~8.1.0 || … || ~8.5.0`, `magento/framework >=103.0.4 <104`, `msp/common` (nasz fork).
- Bez zmian w kodzie — moduł kompiluje się na 2.4.9/8.4 bez modyfikacji.
- Zweryfikowane: setup:upgrade, czysty di:compile, instancjacja wszystkich klas (w tym Varnish), preferencje API wpięte, brak wpływu na REST API. Runtime purge Varnisha wymaga Varnisha do pełnego testu.

## Oryginał (msp/apienhancer) — MageSpecialist, porzucone 2018.
