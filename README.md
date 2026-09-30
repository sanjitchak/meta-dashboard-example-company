# Rebuild for another company

This template produces a separate static website and ZIP. Upload the generated files to GitHub Pages or shared hosting. No Node.js server or database is required. Python 3 is needed only on your computer when building.

1. Copy `template/company.json` and replace the company name, quoted account/business IDs, profile name, currency, country, dates and preset name. Optional `profileImage` and `accountImage` are local image paths relative to that JSON file.
2. Export three Meta reports for the same exact reporting range, without breakdowns. Save them in a folder as `campaigns.csv`, `adsets.csv`, `ads.csv`. Keep Meta's English column headings. Include Campaign ID, Ad set ID and Ad ID where available to link rows and previews. Keep IDs as full digits; do not let Excel convert them to scientific notation.
3. Optionally copy `template/example/creatives.json`, replacing the example with your creatives. `adId` must be `a:` followed by the exact Ad ID. Supply primary text, headline, destination and Page identity. Optional `image` and `pageAvatar` are local file paths relative to this JSON file. Only supplied creatives and images are included in the new site.
4. Run from the template folder:

```sh
python3 scripts/build-company.py \
  --config template/company.json \
  --reports template/example \
  --creatives template/example/creatives.json \
  --output output/example-company
```

Replace the input paths with yours. Omit `--creatives` when you have no creative data. Choose a new output directory for each build; existing directories are preserved. The command writes `output/example-company/` and `output/example-company.zip`.

The included Example Company CSVs are synthetic, with USD 42.50 spend, to demonstrate the format. Do not use them as real company results.

## Hosting

Upload the **contents** of the output folder to the root of a GitHub Pages repository, including `.nojekyll`, or to the chosen shared-hosting web directory. All runtime paths are relative and work under a GitHub project subpath. No build runs on the hosting server. The generated website is public wherever you publish it; publish only the report data you want visitors to see.

## Data rules

- Currency in CSV headings must match the configured currency. The builder normalizes internal metric keys; display and exports use the configured currency.
- Reporting dates must match across all inputs. Mixed date ranges and repeated object IDs are rejected, because they can double count totals.
- A new site's storage key includes account ID and report range, separating browser edits from other companies.
- Company-specific data, ad media, profile images and observed sample rows from the included VSL example are excluded from generated sites.
- Missing values remain unavailable. Do not infer account reach/frequency by adding ad rows. Optional `accountFrequency` can be set only when known from Meta's total row.
- Previews use supplied text and media. Layouts approximate Meta placements; videos in Meta exports are usually thumbnails. Live ad delivery, billing and notifications are not provided by a static template.
- Local editor changes remain in the browser. Use JSON backup for those changes; rebuild from source files for a new published snapshot.

`assets/company.js`, `assets/data.js`, and `assets/creatives.js` are the generated site's editable configuration and data files. Keep the full template folder if you intend to build again; the generated hosting ZIP contains runtime files only.
