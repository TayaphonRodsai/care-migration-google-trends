# Seed terms (round 2)

Seeds for keyword discovery round 2 (from Notebook 02).
✔︎ = found in round-1 related queries · ✎ = common local word, only a starting point.
A seed is kept only if Google Trends has data for it and its related queries confirm it.

| Code | Care seed | Training seed | Work seed |
|---|---|---|---|
| VN | điều dưỡng đức ✎ | du học nghề đức ✎ | xuất khẩu lao động đức ✎ |
| PH | nurse germany ✎ | ausbildung in germany ✔︎ | jobs in germany ✎ |
| IN | nursing jobs in germany ✎ | ausbildung in germany ✎ | jobs in germany ✔︎ |
| TR | almanya hemşire ✎ | almanya ausbildung ✎ | almanya iş ilanları ✔︎ |
| MA | infirmier allemagne ✎ | ausbildung ✔︎ | travail en allemagne ✎ |
| TN | salaire infirmier en allemagne ✔︎ | ausbildung en allemagne ✔︎ | travail en allemagne ✎ |
| ID | perawat jerman ✎ | ausbildung jerman ✎ | kerja di jerman ✎ |
| BA | medicinska sestra njemačka ✎ | ausbildung njemačka ✎ | posao u njemačkoj ✔︎ |
| CM | infirmier allemagne ✎ | ausbildung allemagne ✎ | travailler en allemagne ✎ |

## Social media check

The same seed words are used to **find** public Facebook groups, Meta Ad Library ads and TikTok posts.
Words that appear often there are added to `keyword_log.csv` with their source type.
Only words are recorded — no names, images or personal data.
Facebook and TikTok results depend on the account and location; searches were done from an account based in Germany.

## How to fill `keyword_log.csv`

| Column | Example |
|---|---|
| date_found | 2026-11-14 |
| country | PH |
| language | en |
| term | nurse jobs germany |
| category | care / training / work / language / visa |
| source | trends_round1 / trends_round2 / fb_group / fb_ad_library / tiktok / native_speaker |
| source_detail | e.g. "rising query of seed 'nurse germany'" — no personal names |
| in_google_trends | yes / no (mostly zeros) |
| kept | yes / no |
| reason | e.g. "highest line among synonyms" |
