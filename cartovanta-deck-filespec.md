# CartoVanta Format Specification

**Document Revision:** `<revision-id-here>`  
**Format Specifier:** `<format-string-here>`

## 1. Purpose

This specification defines the CartoVanta format for a deck of cards.  
The format stores references to card images and related metadata.  
The format does not embed image data in `deck.json` itself.  
A deck package also includes a separate `meta.json` manifest.  
The required value of any JSON `format` field is not defined inline by this document revision.  
Instead, it must be the CartoVanta format identifier specified by the CartoVanta Format Registry for the applicable version of the file format. The CartoVanta Format Registry (https://github.com/CartoVanta/cartovanta-filespec/blob/main/version-registry.md) maps each format identifier to its corresponding canonical specification revision.

## 2. Deck Layout

### 2.1. Deck Directory Layout

A deck directory must contain:

- `deck.json`
- `meta.json`
- `imagia/`

The required deck index file is always named:

- `deck.json`

The required outer manifest file is always named:

- `meta.json`

All card image files, including the shared back-of-card image, must be located inside `imagia/` and must be direct children of said directory.

### 2.2. Deck Archive Layout

A CartoVanta deck archive (`.cvdk`) SHALL be a gzip-compressed tar archive whose root contains the following required paths:

- `deck.json`
- `meta.json`
- `imagia/`

When extracted into an empty directory, a CartoVanta deck archive SHALL produce a valid Deck Directory.

## 3. File Types

The file `deck.json` must be valid JSON.  
The file `meta.json` must be valid JSON.

## 4. `deck.json` Top-Level Structure

The file `deck.json` must be a JSON object with these top-level fields:

- `format`
- `deckId`
- `deckName`
- `version`
- `backImage`
- `cardSize`
- `meta`
- `cards`

Example:

```json
{
  "format": "<this CartoVanta version's format identifier>",
  "deckId": "test-deck",
  "deckName": "Test Deck",
  "version": "2026-03-17-1",
  "backImage": "imagia/back.png",
  "cardSize": {
    "width": 180,
    "height": 300
  },
  "meta": {},
  "cards": [
    {
      "id": "card-001",
      "name": "Card One",
      "frontImage": "imagia/card-one.jpg",
      "meta": {}
    }
  ]
}
```

## 5. `deck.json` Top-Level Fields

### 5.1 `format`

Required.  
String.  
Identifies the CartoVanta format used by the deck.  
This value must be the CartoVanta format identifier defined by the CartoVanta Format Registry corresponding to the version of the file format in use.

### 5.2 `deckId`

Required.  
String.  
A machine-friendly identifier for the deck.

### 5.3 `deckName`

Required.  
String.  
A human-readable name for the deck.

### 5.4 `version`

Required.  
String.  
A version identifier for the deck file.

### 5.5 `backImage`

Required.  
String.  
Relative path to the shared back-of-card image.  
This path must:

- begin with `imagia/` and contain no additional path separators after `imagia/`
- point to a file directly inside `imagia/`
- not be a URL

### 5.6 `cardSize`

Required.  
Object.  
Specifies the uniform dimensions expected for all cards in the deck.

#### 5.6.1 `cardSize.width`

Required.  
Number.  
The width of each card image in pixels.

#### 5.6.2 `cardSize.height`

Required.  
Number.  
The height of each card image in pixels.

### 5.7 `meta`

Required.  
Object.  
A hash/object for additional metadata about the deck as a whole.  
The `meta` object may be empty.  
Example:

```json
"meta": {}
```

### 5.8 `cards`

Required.  
Array.  
An array of card objects.

## 6. Card Object Structure

Each element of the `cards` array must be a JSON object with these required fields:

- `id`
- `name`
- `frontImage`
- `meta`

Example card object:

```json
{
  "id": "card-001",
  "name": "Card One",
  "frontImage": "imagia/card-one.jpg",
  "meta": {}
}
```

## 7. Card Fields

### 7.1 `id`

Required.  
String.  
A unique identifier for the card within the deck.

### 7.2 `name`

Required.  
String.  
The display name of the card.

### 7.3 `frontImage`

Required.  
String.  
Relative path to the front image for that card.  
This path must:

- begin with `imagia/` and contain no additional path separators after `imagia/`
- point to a file inside `imagia/`
- not be a URL

### 7.4 `meta`

Required.  
Object.  
A hash/object for additional metadata about the card.  
The `meta` object may be empty.  
Example:

```json
"meta": {}
```

## 8. `meta.json` Structure

The file `meta.json` must be a JSON object with these top-level fields:

- `format`
- `meta`

It may also contain:

- `deckName`

Example:

```json
{
  "format": "<this CartoVanta version's format identifier>",
  "deckName": "Test Deck",
  "meta": {}
}
```

## 9. `meta.json` Fields

### 9.1 `format`

Required.  
String.  
Identifies the CartoVanta format used by the package.  
This value must be the CartoVanta format identifier defined by the CartoVanta Format Registry corresponding to the version of the file format in use.

### 9.2 `deckName`

Optional.  
String.  
If present, this must exactly match `deck.json`'s `deckName` value.

### 9.3 `meta`

Required.  
Object.  
A hash/object for additional package-level metadata.  
The `meta` object may be empty.  
Unknown or vendor-specific outer-manifest fields must go inside this object rather than appearing as unknown top-level fields.

## 10. Shared-Metadata Consistency Rules

Fields may appear in `deck.json`'s top-level `meta` object, in `meta.json`'s `meta` object, or in both.  
A field is not required to appear in both places.  
However, if the same field path appears in both objects, the values must be identical.  
Equality is exact JSON equality of type and content.  
No coercion, case-folding, or normalization is performed.

## 11. Image Filename Rules

For every image filename inside `imagia/`:

- the basename must contain only ASCII letters, ASCII digits, underscore (`_`), and hyphen (`-`)
- the basename must not be empty
- the basename must not contain spaces
- the basename must not contain any other punctuation
- the extension must begin with a period
- the extension must match the actual image type

Allowed image types and extensions:

- `.jpg` or `.jpeg` for JPEG
- `.png` for PNG
- `.webp` for WebP

GIF is not permitted.

## 12. Case Rules

File references in `deck.json` are case-sensitive and must match the corresponding on-disk path exactly.  
Additionally, no two filenames anywhere in a deck package may differ only by alphabetic case.

## 13. Notes

- The `backImage` field is shared by all cards.
- The `cardSize` field is specified once for the entire deck, not per card.
- The top-level `meta` field in `deck.json` is deck-wide metadata.
- The top-level `meta` field in `meta.json` is outer-manifest metadata.
- The file `meta.json` is intended to remain outside any future DRM payload layer.
- Each card in a deck is expected to have uniform dimensions.
- The reading script may ignore fields inside `meta` unless explicitly programmed to use them.

## 14. Validation Rules

A valid deck must satisfy all of the following:

- `deck.json` must exist at the deck root
- `meta.json` must exist at the deck root
- `imagia/` must exist at the deck root
- `deck.json` must be valid JSON
- `meta.json` must be valid JSON
- the top-level value of each JSON file must be an object
- `deck.json` `format` must be present and must be the recognized CartoVanta format identifier defined by the CartoVanta Format Registry corresponding to the version of the file format being validated
- `meta.json` `format` must be present and must be the recognized CartoVanta format identifier defined by the CartoVanta Format Registry corresponding to the version of the file format being validated
- `deck.json` `format` and `meta.json` `format` must be identical
- `deck.json` must contain `deckId`, `deckName`, `version`, `backImage`, `cardSize`, `meta`, and `cards`
- `meta.json` must contain `format` and `meta`
- `deck.json` `cardSize` must be an object
- `deck.json` `cardSize.width` and `deck.json` `cardSize.height` must be present
- `deck.json` `meta` must be an object
- `meta.json` `meta` must be an object
- `deck.json` `cards` must be an array
- each card must have `id`, `name`, `frontImage`, and `meta`
- each card `id` must be unique within the deck
- each per-card `meta` field must be an object
- `backImage` and every `frontImage` must reference a file that is a direct child of `imagia/` and therefore must contain no additional path separators after `imagia/`
- `backImage` and every `frontImage` must reference allowed image types only
- every referenced file must satisfy the filename-character rules
- no two filenames in the deck may differ only by alphabetic case
- if `meta.json` `deckName` is present, it must exactly equal `deck.json` `deckName`
- if the same field path appears in both `deck.json` `meta` and `meta.json` `meta`, the values must be identical
- `meta.json` must not contain unknown top-level fields other than `format`, optional `deckName`, and `meta`
