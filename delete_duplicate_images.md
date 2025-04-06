### Delete Duplicate Product Images

This SQL query deletes duplicate product images from the `oc_product_image` table while retaining the first image for each unique image. The query identifies duplicates by using the `MIN()` function to find the smallest `product_image_id` for each image and keeps only the first one. All other duplicate entries are deleted.

- **Subquery**: The subquery finds the minimum `product_image_id` for each unique image by grouping them with the `GROUP BY` clause.
- **DELETE**: The main query deletes all rows where the `product_image_id` is not in the list of minimum `product_image_id` values, effectively removing duplicate images.
  
### SQL Example:

```sql
DELETE FROM oc_product_image 
WHERE product_image_id NOT IN ( 
    SELECT min_id 
    FROM ( 
        SELECT MIN(product_image_id) as min_id 
        FROM oc_product_image 
        GROUP BY image 
    ) as subquery 
);
