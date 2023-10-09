# Patternfly customizations and notes

Right now, only exporting from Figma is working - importing causes a memory error.
1. Output variables from Figma using the export plugin, and paste the output into AsExported.text
1. Copy each mode (marked with comments) into separate token .json files.
1. `$type` and `$value` have to have the `$` removed for style-dictionary. However, this is the draft W3C recommendation, so style-dictionary should handle this somehow?
1. Run style dictionary. This does a deep merge of all token .JSON files in the path and generates one file. Run build 3 times, with each of the config files listed below.

    ```bash
    style-dictionary build
    ```

NOTE: To generate individual files, we will need to use filters. Theo creates separate files and this feature is requested for Style Dictionary as well. **It's unclear currently what to filter on to create the structure we want.**

  - Currently there are 3 config.json files - one for palette colors, one for default tokens, and one for dark tokens.
  - The palette config filters to just export the palette colors to `_tokens-palette.scss`
  - The default config filters to export tokens in the default directory and starting with `global` (excluding palette colors) into `_tokens-default.scss`
  - The dark config filters to export tokens in the dark directory that start with `global` (excluding palette colors) to `_tokens-dark.scss`

5. To correct the delimiters in the output files:
    1. replace `-` with `--`
    1. replace `----pf--t--` with `--pf-t-`
    1. replace `--on--` with `--on-`
    1. Dimensions need to have px appended
    1. Tokens will need to be versioned
  
Long term, we should be able to write our own formatter to create the variable names correctly.

**NOTE:** Dark semantic tokens can't be processed at the same time because their names conflict. ~~We'll either need to put "dark" into the token hierarchy or generate them separately.~~
- 8/14/23 Lucia added /global/dark/.. to dark base tokens so they get names unique from the default ones. We can't do that with dark semantic because they need to be two modes in Figma.
- 8/7/23 decision with Coker and Lucia - add dark wrapping dark tokens to generate `--pf-t--dark--`. ~~Problem - values are not updated to include dark~~
- Also, we need to decide about versioning tokens at some point. We probably will, but maybe a separate stream from PF versions?
- 8/7/23 Let's generate tokens in one file for now and we'll figure out later how to filter them into layers (and sort them logically). Base have 100-200-300 names but semantic don't.


Future reference: Built-in [transforms](https://amzn.github.io/style-dictionary/#/transforms?id=pre-defined-transforms) and [formats](https://amzn.github.io/style-dictionary/#/formats?id=pre-defined-formats).
