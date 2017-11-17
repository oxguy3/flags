## Build system
I'm going to package this up with NPM + Lerna. I'll have one package where the raw, unoptimized SVGs live, probably organized by ISO-3166 alpha-2, ISO 3166-2, and UN/LOCODE (for countries, subdivisions, cities, respectively). (TODO: figure out how to organize files that don't have ISO or LOCODE entries)

Then I'll have a JSON/CSV/Sqlite/idk database file that maps all the codes to each other. There will have to be some sort of script/process for pulling the codes from official sources and updating the database as needed.

Then there will be the build script, for rearranging/processing the image files to your liking. More info below....

### Build script
Here's what the `./build.sh --help` might look like:
```
./build.sh [flags] [destination]

ARGUMENTS
  - destination: path to where the files will be dumped (all code system paths will be relative to this path) (default: build/)

BASIC FLAGS
  --help: Show this text
  --defaults: Shortcut for --iso-1-alpha-2 --optimize-svg --split-subdivisions

CODE SYSTEM FLAGS
  Specifies what code system(s) should be used to name the output files. You must specify at least one of these.

  --all-codes: shortcut for all code systems
  --iso: all ISO code systems
  --iso-1: shortcut for all three ISO 3166-1 code systems
  --iso-1-alpha-2: ISO 3166-1 alpha-2 country codes
  --iso-1-alpha-3: ISO 3166-1 alpha-3 country codes
  --iso-1-numeric: ISO 3166-1 numeric country codes
  --iso-2: ISO 3166-2 subdivision codes
  --fifa: FIFA country codes
  --ioc: IOC (Olympics) country codes
  --unlocode: UN/LOCODE city codes
  --fips-legacy: all FIPS 10-4 (aka GEC) code systems
  --fips-legacy-ge: FIPS 10-4 (aka GEC) country codes
  --fips-legacy-as: FIPS 10-4 (aka GEC) subdivision codes
  --fips-us: all U.S. Census FIPS codes
  --fips-us-state: U.S. Census FIPS state codes
  --fips-us-county: U.S. Census FIPS county codes
  --fips-us-place: U.S. Census FIPS place codes (cities/townships/etc)

OUTPUT FOLDER FLAGS
  --folder-divisions=<none|some|all>: specifies how the code system folders should be structured (default: best guess)
      'none' = no folders, completely flat output (may cause collisions if multiple code systems are enabled)
      'some' = separate folders, but iso and fips-us share folders
      'all' = separate folders for every code system
  --split-subdivisions: if set, subdivision codes will divided into parts to make a hierarchical folder structure instead of a flat one
  --use-symlinks: create symlinks instead of creating duplicate output files

FILE FORMAT FLAGS
  --make-pngs=<comma-delimited integer(s)>: will generate PNGs at each of the specified pixel widths
  --no-svg: if set, SVG files will be excluded/deleted from the final output
  --optimize-svg: if set, SVG files will be optimized with svgo
```

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
* Use content negotiation to serve _unknown.svg_ as 404 page when appropriate.
* Directory listing will be enabled to allow easy browsing.


## Supported code systems

* [ISO 3166](https://en.wikipedia.org/wiki/ISO_3166) [`iso`] (country + subdivision)
  * [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) (country)
  * [ISO 3166-1 alpha-3](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-3) (country)
  * [ISO 3166-1 numeric](https://en.wikipedia.org/wiki/ISO_3166-1_numeric) (country)
  * [ISO 3166-2](https://en.wikipedia.org/wiki/ISO_3166-2) (subdivision)
* [FIFA](https://en.wikipedia.org/wiki/List_of_FIFA_country_codes) [`fifa`] (country only)
* [International Olympic Committee](https://en.wikipedia.org/wiki/List_of_IOC_country_codes) [`ioc`] (country only)
* [UN/LOCODE](http://www.unece.org/cefact/locode/service/location) [`unlocode`] (subdivision only)
* [FIPS 10-4 / GEC](https://en.wikipedia.org/wiki/List_of_FIPS_country_codes) [`fips-legacy`] (country + subdivision)
* [FIPS place codes (U.S. Census)](https://www.census.gov/geographies/reference-files/2016/demo/popest/2016-fips.html) [`fips-us`] (U.S.-specific, state + county/place)

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
* More country code systems
  * [ISO 3166-3](https://en.wikipedia.org/wiki/ISO_3166-3) (four-digit codes for deleted countries)
  * [telephone calling prefixes](https://en.wikipedia.org/wiki/E.164)
  * [ICAO airport codes](https://en.wikipedia.org/wiki/ICAO_airport_code)
  * [NATO trigrams](https://en.wikipedia.org/wiki/List_of_NATO_country_codes)
  * [GENC](https://nsgreg.nga.mil/genc/discovery) (America-flavored ISO)
  * [ccTLDs](https://en.wikipedia.org/wiki/Country_code_top-level_domain) (mostly the same as ISO 3166-1 alpha-2, but a few exceptions)
  * [the other junk over here](http://www.statoids.com/wab.html)
  * english standard name?
* Search by name?? (probably not happening unless this gets big enough to spend big bux on -- would require server-side processing)
* Support for historical info
  * old flags
  * old codes
* Add PNGs? (worry: anything bigger than the minis will use more bandwidth and potentially get expensive)
* Add more image types
  * flags cropped/fit to a consistent ratio (probably 1:2)
  * square/circle icons
  * coal of arms

## website theme ideas:
  * https://jekyllthemes.io/theme/30191476/grayscale-theme
  * https://jekyllthemes.io/theme/36226247/polar-bear-theme
