## URL structure

```
files.flag.place/
  v1/
    {{code_system}}/
      {{top_level_code}}/
        flag.svg
        {{second_level_code}}/
          flag.svg
```
Notes:
* `code_system` should be the identifier for the code system to be used. Valid code systems are listed below in the "Supported code systems" section.
* For most code systems, `top_level_code` means "country" and `second_level_code` means "subdivision" (i.e. state/province/etc).
  * For `fips-us`, the country is always United States, so `top_level_code` is state and `second_level_code` is county/place.
* 404 pages will load unknown.svg.
* Directory listing will be enabled.


## Supported code systems

* ISO 3166 [`iso`] (country+subdivision)
  * ISO 3166-1 alpha-2 (country)
  * ISO 3166-1 alpha-3 (country)
  * ISO 3166-1 numeric (country)
  * ISO 3166-2 (subdivision)
* [FIFA](https://en.wikipedia.org/wiki/List_of_FIFA_country_codes) [`fifa`] (country only)
* [International Olympic Committee](https://en.wikipedia.org/wiki/List_of_IOC_country_codes) [`ioc`] (country only)
* [UN/LOCODE](http://www.unece.org/cefact/locode/service/location) [`unlocode`] (country+subdivision)
* [FIPS 10-4 / GEC](https://en.wikipedia.org/wiki/List_of_FIPS_country_codes) [`fips-legacy`] (country+subdivision)
* [FIPS place codes (U.S. Census)](https://www.census.gov/geographies/reference-files/2016/demo/popest/2016-fips.html) [`fips-us`] (U.S.-specific, subdivision only)

useful links:
* [IOC vs FIFA vs ISO](https://en.wikipedia.org/wiki/Comparison_of_alphabetic_country_codes)
* [comparison of a crapton of systems](http://www.statoids.com/wab.html)

## Examples

### Countries (Germany)
```
files.flag.place/
  v1/
    iso/
      DE/
        flag.svg
      DEU/
        flag.svg
      276/
        flag.svg
    fifa/
      GER/
        flag.svg
    ioc/
      GER/
        flag.svg
```
full URLs:
* https://files.flag.place/v1/iso/DE/flag.svg
* https://files.flag.place/v1/iso/DEU/flag.svg
* https://files.flag.place/v1/iso/276/flag.svg
* https://files.flag.place/v1/fifa/GER/flag.svg
* https://files.flag.place/v1/ioc/GER/flag.svg

### Subdivisions (Ohio)
```
files.flag.place/
  v1/
    iso/
      US/
        OH/
          flag.svg
    fips-legacy/
      US/
        39/
          flag.svg
    fips-us/
      39/
        flag.svg
```
full URLs:
* https://files.flag.place/v1/iso/US/OH/flag.svg
* https://files.flag.place/v1/fips-legacy/US/39/flag.svg
* https://files.flag.place/v1/fips-us/39/flag.svg

### Counties/places (Cincinnati)
```
files.flag.place/
  v1/
    unlocode/
      US/
        CVG/
          flag.svg
    fips-us/
      39/
        15000/
          flag.svg
```
full URLs:
* https://files.flag.place/v1/unlocode/US/CVG/flag.svg
* https://files.flag.place/v1/fips-us/39/15000/flag.svg

## Potential expansions
* more country code systems
  * ISO 3166-3 (four-digit codes for deleted countries)
  * [telephone calling prefixes](https://en.wikipedia.org/wiki/E.164)
  * [ICAO airport codes](https://en.wikipedia.org/wiki/ICAO_airport_code)
  * [NATO trigrams](https://en.wikipedia.org/wiki/List_of_NATO_country_codes)
  *
  * [GENC](https://nsgreg.nga.mil/genc/discovery) (America-flavored ISO)
  * [ccTLDs](https://en.wikipedia.org/wiki/Country_code_top-level_domain) (mostly the same as ISO 3166-1 alpha-2, but a few exceptions)
  * [the other junk over here](http://www.statoids.com/wab.html)
  * english standard name?
* search by name?? (probably not happening unless this gets big enough to spend big bux on -- would require server-side processing)
* historical support
  * old flags
  * old codes
* more image types
  * flags forced into standard ratio (probably 1:2)
  * square/circle icons
  * coal of arms

## website theme ideas:
  * https://jekyllthemes.io/theme/30191476/grayscale-theme
  * https://jekyllthemes.io/theme/36226247/polar-bear-theme
