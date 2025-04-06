### Category Path Retrieval for Product

This SQL query retrieves the category path for a specific product (`product_id = 120`). It builds the category path by joining the `ca_category` and `ca_category_description` tables for multiple levels of categories (up to three levels). The query uses `COALESCE` to handle cases where there may be missing parent categories, ensuring the path is constructed correctly even for products with fewer than three categories.

- **COALESCE**: The query concatenates the names of the categories, with ' > ' separating each level of the path. If a parent category is missing, it omits that level from the path.
- **CONCAT**: The `CONCAT` function is used to join multiple strings into a single string. In this query, it combines the names of categories at different levels, separated by ' > '. For example, if a product has a category hierarchy like "Electronics > Computers > Laptops", the `CONCAT` function will concatenate these category names into one string: `"Electronics > Computers > Laptops"`.
- **JOINs**: It uses inner joins (`JOIN`) for categories and category descriptions and left joins (`LEFT JOIN`) for parent categories, ensuring flexibility in handling products with varying levels of category depth.

### Explanation of `CONCAT`:
The `CONCAT` function is used to combine multiple strings into one. In this query, it is used to build the `category_path` by combining the names of categories with ' > ' between them. If there are three categories, the result might look like: `"Electronics > Computers > Laptops"`. If a category is missing (e.g., the product only has a child category), the query will adjust the concatenation accordingly.

This format now provides a complete explanation of the `CONCAT` function along with the rest of the query's logic.

### SQL Example:

```sql
SELECT 
    c3.category_id,
    COALESCE(
        CONCAT(cd1.name, ' > ', cd2.name, ' > ', cd3.name), 
        CONCAT(cd2.name, ' > ', cd3.name), 
        cd3.name
    ) AS category_path
FROM 
    ca_product_to_category p2c 
    JOIN ca_category c3 ON p2c.category_id = c3.category_id 
    JOIN ca_category_description cd3 ON c3.category_id = cd3.category_id 
        AND cd3.language_id = 1
    LEFT JOIN ca_category c2 ON c3.parent_id = c2.category_id 
    LEFT JOIN ca_category_description cd2 ON c2.category_id = cd2.category_id 
        AND cd2.language_id = 1
    LEFT JOIN ca_category c1 ON c2.parent_id = c1.category_id 
    LEFT JOIN ca_category_description cd1 ON c1.category_id = cd1.category_id 
        AND cd1.language_id = 1
WHERE 
    p2c.product_id = 120

