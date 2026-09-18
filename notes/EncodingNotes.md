# File Encoding Notes

## acorn_details.csv: cp1252, not UTF-8

Default UTF-8 read fails: `UnicodeDecodeError: byte 0xa3 in position 0: invalid start byte`.
`0xa3` is not a valid UTF-8 lead byte, so the file isn't UTF-8. `0xa3` is `£` in
Windows-1252, and the file's `REFERENCE` column holds UK income-band labels, so a `£`
there is expected content. Retried with `encoding="cp1252"`, parsed cleanly (826 rows,
20 columns).

All other top-level files and every `block_*.csv` read fine under UTF-8, so the override
is scoped to this one file: `FILE_ENCODING = {"acorn_details.csv": "cp1252"}` in
[01_eda_smart_meters.ipynb](01_eda_smart_meters.ipynb).
