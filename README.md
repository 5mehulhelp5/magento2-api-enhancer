# Magento 2 API Enhancer — maintained fork (SISL)

Extends **Varnish-based caching for the Magento 2 REST API**: control over caching of API
responses, **cache tags** (precise invalidation when a product/category changes), cache-key
processors, and customer-authentication handling for cached responses. For headless/PWA stores and
integrations that hit the REST API hard, this offloads the backend through a Varnish layer.

Part of the **MageSpecialist Security Suite / API tools**. This is a maintained fork of the
abandoned `msp/apienhancer` (last release 2018, `php ^7.0|^7.1` — **does not run on any PHP 8.x**).
This fork loosens the dependencies and is verified on **Magento 2.4.9 / PHP 8.4** (di:compile,
instantiation of all classes, wiring of the API preferences).

## Compatibility
- Magento **2.4.4 – 2.4.9** (Open Source / Adobe Commerce)
- PHP **8.1 – 8.4**
- Requires `sisl-source/magento2-msp-common` (our fork) and a working **Varnish** for the API caching to actually take effect

## Installation

```bash
composer require sisl-source/magento2-api-enhancer
bin/magento module:enable MSP_Common MSP_APIEnhancer
bin/magento setup:upgrade
bin/magento setup:di:compile   # production mode
```

## How it works
- `Model\VarnishManagement` / `VarnishTokenProcessor` — talks to Varnish (PURGE/BAN by tags).
- `Model\Tag` + `Observer\Catalog*LoadAfter` — attaches cache tags to API responses based on the loaded products/categories, for targeted invalidation.
- `Model\CacheManagement` (`Api\CacheManagementInterface`) — controls caching of responses.
- `Model\CustomerAuth` — handles customer context for cached, personalised responses.

The Varnish address and rules are configured in the admin (MSP section) and via di.xml. Without
Varnish the module installs and does not break the store, but real API caching requires a
configured Varnish.

## Scope of the fork's verification
Confirmed on 2.4.9 / PHP 8.4: `setup:upgrade`, a clean `di:compile`, instantiation of all classes
(including the Varnish models), wiring of the API preferences, no impact on the store's REST API.
Actual Varnish cache purging requires a running Varnish to be fully tested in your environment.

## License
OSL-3.0 (same as upstream). Fork maintained by [SISL](https://sisl.pl).
