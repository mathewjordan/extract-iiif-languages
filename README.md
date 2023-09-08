# extract-iiif-languages

Find all possible BCP-47 language codes in a IIIF Presentation API 3.0 resource.

## Problem statement

To enhance user experience and provide language options in user interfaces, we want to determine possible languages of provided IIIF resources rendered within a viewport or application.

## Proposed solution

Create reusable functionality that will analyze provided resources and provide back all possible language codes as an array. Support could include fetching options using IIIF Commons packages and Presentation 2.x → 3.0 upgrade ability of resources as well.

### Why?

According to section [4.4 Language of Property Values](https://iiif.io/api/presentation/3.0/#language-of-property-values) defined in the most current specification:

> Language **MAY** be associated with strings that are intended to be displayed to the user for the `label` and `summary` properties, plus the `label` and `value` properties of the `metadata` and `requiredStatement` objects.

Given this, we have the following `label` propert that could be contained within a Manifest, Collection, or Canvas.

```json
{
  "label": {
    "en": [
      "Whistler's Mother",
      "Arrangement in Grey and Black No. 1: The Artist's Mother"
    ],
    "fr": [
      "Arrangement en gris et noir no 1",
      "Portrait de la mère de l'artiste",
      "La Mère de Whistler"
    ],
    "none": ["Whistler (1871)"]
  }
}
```

The resource would have at a minimum the following values:

- **en**
- **fr**
- **none**

The value _none_ is not a language code, however it is required by Presentation 3.0 API specification in certain cases.

> ...if the language is either not known or the string does not have a language, then the key **MUST** be the string `none`.

### How?

Given a case where a Manifest has `ar` (Arabic), `en` (English) as possible codes with possibility of `none` values.

```tsx
const iiifContent = "https://api.dc.library.northwestern.edu...";
const options = {
  // to be determined
};

const data = extractIIIFLanguages(manifest, options);
const { languageCodes, info } = data;

console.log(languageCodes);

// output: ["ar", "en", "none"]
```
