### Retrieve Categories with Product Count for a Specific Store

This SQL query retrieves categories along with the count of products assigned to each category for a specific store. It also filters categories based on a specific parent category, language, and store ID. The query ensures that only categories with products are returned, ordered by their sort order.

- **Joins**: The query uses multiple `LEFT JOIN` clauses to connect the `oc_category`, `oc_category_description`, `oc_product_to_category`, `oc_product`, and `oc_product_to_store` tables. This ensures that category data is joined with product and store information.
- **COUNT**: The `COUNT(p.product_id)` function counts the number of products associated with each category.
- **GROUP BY**: The query groups the results by `category_id`, ensuring that the count of products is calculated per category.
- **HAVING**: The `HAVING` clause filters out categories with no products (`total_products > 0`).
- **WHERE Clause**: Filters categories based on the `parent_id`, `language_id`, and `store_id`.

### SQL Example:

```sql
SELECT c.category_id, cd.name, COUNT(p.product_id) as total_products
FROM oc_category c
LEFT JOIN oc_category_description cd ON (c.category_id = cd.category_id)
LEFT JOIN oc_product_to_category pc ON (c.category_id = pc.category_id)
LEFT JOIN oc_product p ON (pc.product_id = p.product_id)
LEFT JOIN oc_product_to_store ps ON (p.product_id = ps.product_id)
WHERE c.parent_id = $specific_id 
AND cd.language_id = $language_id
AND ps.store_id = $store_id
GROUP BY c.category_id
HAVING total_products > 0
ORDER BY c.sort_order;
