We can associate a procedure function to our repository using `@Procedure` useful if our custom queries are complex
```sql
DELIMITER $$

CREATE PROCEDURE findProductsByPrice(
	minPrice DECIMAL(10, 2),
	maxPrice DECIMAL(10, 2)
)
BEGIN
	SELECT p.id, p.name, p.price FROM products p 
		WHERE p.price BETWEEN minPrice AND maxPrice;
END $$

DELIMITER ;
```
And we can associate like this
```java
interface ProductRepository extends CrudRepository<Product, Integer>{
	@Procedure("findProductsByPrice") // name of the procedure
	void findProductsByPrice(Decimal min, Decimal max);
}
```
