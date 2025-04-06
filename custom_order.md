### Custom Ordering Based on `season_type`

This SQL `ORDER BY` clause sorts query results based on the value of the `season_type` column in the `p` table. The sorting is defined with a custom priority using a `CASE` statement:

- **'S'**: Records with `season_type = 'S'` will be sorted first (priority `1`).
- **'W'**: Records with `season_type = 'W'` will be sorted second (priority `2`).
- **NULL or Empty String**: Records where `season_type` is either `NULL` or an empty string will be sorted third (priority `3`).
- **Other Values**: All other values of `season_type` will be sorted last (priority `4`).

### SQL Example:

```sql
ORDER BY
    CASE
        WHEN p.season_type = 'S' THEN 1
        WHEN p.season_type = 'W' THEN 2
        WHEN p.season_type IS NULL OR p.season_type = '' THEN 3
        ELSE 4
    END
