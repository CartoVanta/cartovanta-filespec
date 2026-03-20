# Registry of Recognized Standardized Versions of the CartoVanta filespec.

## Preparing the specification for sharing outside the repository

The specification document stored in this repository is the canonical maintained source copy and may contain placeholder text in fields such as `Document Revision:` and `Format Identifier:`.

To prepare a standalone copy of the specification for sharing outside the context of this repository, use the following procedure:

1. Locate the relevant registry entry for the CartoVanta format version in question.
2. Download the specification document from the canonical specification URL given in that entry.
3. In the downloaded specification document, replace the first occurrence of `Document Revision: <revision-id-here>` with `Document Revision: ` followed by the value of **Canonical Specification Revision** from the relevant registry entry.
4. In the downloaded specification document, replace the first occurrence of `Format Identifier: <format-string-here>` with `Format Identifier: ` followed by the value of **Format Identifier** from the relevant registry entry.

For example, for version 0.1 of the filespec, the first few lines prior to the modifications in steps `3` and `4` would be:
```
# CartoVanta Format Specification

**Document Revision:** `<revision-id-here>`  
**Format Specifier:** `<format-string-here>`
```
After the modifications in steps `3` and `4` they would be:
```
# CartoVanta Format Specification

**Document Revision:** `0.1`  
**Format Specifier:** `cartovanta-v0.1`
```
 
5. Distribute the resulting stamped copy as the standalone specification for that CartoVanta format version.

## Format Versions

#### CartoVanta Format Version 0.1

- **Format Version:** `0.1`
- **Format Identifier:** `cartovanta-v0.1`
- **Canonical Specification Revision:** `92c5b71d6c6ddff7980ecbad151aac2759d86376`
- **Canonical Specification URL:** https://github.com/CartoVanta/cartovanta-filespec/blob/92c5b71d6c6ddff7980ecbad151aac2759d86376/cartovanta-deck-filespec.md
