# Flags in Google Sheets

Here's a nifty little formula you can use to spruce up your spreadsheets with flags from this repository! You can preview what this will look like in [this spreadsheet](https://docs.google.com/spreadsheets/d/1GoDDhtoDuKwDv9pB5hKFkcHzhI420z2lr4szovryXaU/edit#gid=0).

## Formulas

To place a flag in your document, you will need to have the country's name or country code somewhere in your document. Select one of the following formulas based on which system of country codes/naming you use.

### Two-letter country code

If you use two-letter country codes (specifically [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2#Officially_assigned_code_elements) codes), you can use the following formula:

```
=IMAGE(CONCATENATE("http://flags.ox3.in/svg/", LOWER(A1), ".svg"))
```

Just replace "A1" with the address of the cell where you have the country code, and you're good to go.


### Name of country

If you use country names or non-standard abbreviations or whatever else, this is the formula for you! It searches a list of country aliases and abbreviations and codes to try to find the right flag for you. Here's the formula:

```
=IF(ISBLANK(A1), NA(), IMAGE(CONCATENATE("http://flags.ox3.in/svg/", LOWER(IFERROR(ARRAY_CONSTRAIN(FILTER( IMPORTRANGE("1GoDDhtoDuKwDv9pB5hKFkcHzhI420z2lr4szovryXaU","Countries!C2:C"), IFERROR(SEARCH(CONCATENATE("%",A1,"%"), IMPORTRANGE("1GoDDhtoDuKwDv9pB5hKFkcHzhI420z2lr4szovryXaU","Countries!B2:B"))) ),1,1), "unknown")), ".svg")))
```

Replace both instances of "A1" with the address of the cell where you have the country name. If you're having trouble with this formula not working for some countries, make sure your country is in [this spreadsheet](https://docs.google.com/spreadsheets/d/1GoDDhtoDuKwDv9pB5hKFkcHzhI420z2lr4szovryXaU/edit#gid=0), and that you've spelled it exactly as it appears in there (it's not case-sensitive, but otherwise you have to have it exactly correct).


### Three-letter country codes

Here's the formula for three-letter country codes (specifically [ISO 3166-1 alpha-3](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-3#Current_codes) codes):

```
=IF(ISBLANK(A1), NA(), IMAGE(CONCATENATE("http://flags.ox3.in/svg/", LOWER(IFERROR(FILTER(  IMPORTRANGE("1GoDDhtoDuKwDv9pB5hKFkcHzhI420z2lr4szovryXaU","Countries!C2:C"), IMPORTRANGE("1GoDDhtoDuKwDv9pB5hKFkcHzhI420z2lr4szovryXaU","Countries!D2:D") = UPPER(A1) ), "unknown")), ".svg")))
```

Replace both instances of "A1" with the address of the cell where your country code is stored.


### Numeric country codes

Here's the formula for three-letter country codes (specifically [ISO 3166-1 numeric](https://en.wikipedia.org/wiki/ISO_3166-1_numeric#Current_codes) codes):

```
=IF(ISBLANK(A1), NA(), IMAGE(CONCATENATE("http://flags.ox3.in/svg/", LOWER(IFERROR(FILTER(  IMPORTRANGE("1GoDDhtoDuKwDv9pB5hKFkcHzhI420z2lr4szovryXaU","Countries!C2:C"), IMPORTRANGE("1GoDDhtoDuKwDv9pB5hKFkcHzhI420z2lr4szovryXaU","Countries!E2:E") = A1 ), "unknown")), ".svg")))
```

Replace both instances of "A1" with the address of the cell where your country code is stored.


### FIFA country codes

Here's the formula for [FIFA country codes](https://en.wikipedia.org/wiki/List_of_FIFA_country_codes):

```
=IF(ISBLANK(A1), NA(), IMAGE(CONCATENATE("http://flags.ox3.in/svg/", LOWER(IFERROR(FILTER(  IMPORTRANGE("1GoDDhtoDuKwDv9pB5hKFkcHzhI420z2lr4szovryXaU","Countries!C2:C"), IMPORTRANGE("1GoDDhtoDuKwDv9pB5hKFkcHzhI420z2lr4szovryXaU","Countries!F2:F") = UPPER(A1) ), "unknown")), ".svg")))
```

Replace both instances of "A1" with the address of the cell where your country code is stored.

## Notes

Unfortunately, these formulas use Google-specific features, so they will only work with Google Sheets.

Country flags come in a wide variety of aspect ratios, so there is no perfect cell size for displaying these flags. However, I have found that a row height of 21 pixels and a column width of 38 pixels is fairly optimal (you can precisely define the size by selecting the entire row/column, right-clicking, and choosing the "Resize" option).
