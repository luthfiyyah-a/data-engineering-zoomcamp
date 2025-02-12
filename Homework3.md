3. Why are the estimated number of Bytes different? (1 point)


- BigQuery is a columnar database, and it only scans the specific columns requested in the query. Querying two columns (PULocationID, DOLocationID) requires reading more data than querying one column (PULocationID), leading to a higher estimated number of bytes processed.

- BigQuery duplicates data across multiple storage partitions, so selecting two columns instead of one requires scanning the table twice, doubling the estimated bytes processed.

- BigQuery automatically caches the first queried column, so adding a second column increases processing time but does not affect the estimated bytes scanned.

- When selecting multiple columns, BigQuery performs an implicit join operation between them, increasing the estimated bytes processed

answer: BigQuery is a columnar database, and it only scans the specific columns requested in the query. Querying two columns (PULocationID, DOLocationID) requires reading more data than querying one column (PULocationID), leading to a higher estimated number of bytes processed.


### Query to Retrieve `PULocationID` from the Table:
```sql
SELECT PULocationID
FROM your_table_name;
```

### Query to Retrieve `PULocationID` and `DOLocationID` from the Same Table:
```sql
SELECT PULocationID, DOLocationID
FROM your_table_name;
```

### Explanation of the Estimated Bytes Difference:
The estimated number of bytes processed is different because **BigQuery is a columnar database**. Here's why:

1. **Columnar Storage**:
   - BigQuery stores data in columns rather than rows. This means that when you query a specific column, BigQuery only reads the data for that column.
   - When you query one column (`PULocationID`), BigQuery only scans the data for that column.
   - When you query two columns (`PULocationID` and `DOLocationID`), BigQuery scans the data for both columns, which increases the amount of data read.

2. **Bytes Processed**:
   - Querying one column (`PULocationID`) requires reading only the data for that column.
   - Querying two columns (`PULocationID` and `DOLocationID`) requires reading the data for both columns, which results in more bytes being processed.

3. **No Data Duplication or Implicit Join**:
   - BigQuery does **not** duplicate data across multiple storage partitions. Each column is stored separately, and querying multiple columns simply means reading more columns.
   - There is **no implicit join** when selecting multiple columns. BigQuery reads the columns independently and combines them in the result set.

4. **Caching**:
   - BigQuery does **not** automatically cache the first queried column. Each query is processed independently unless explicitly cached by the user.

### Summary:
- Querying one column (`PULocationID`) scans less data.
- Querying two columns (`PULocationID` and `DOLocationID`) scans more data because BigQuery reads both columns.
- The estimated bytes processed are higher for the second query because more data is being read.


7. Where is the data stored in the External Table you created?

- Big Query
- Container Registry
- GCP Bucket
- Big Table

Data in an **External Table** in BigQuery is stored in a **GCP Bucket** (Google Cloud Storage Bucket). 

Here’s why:

- **External Tables** in BigQuery allow you to query data directly from external sources without loading it into BigQuery storage.
- The most common external source is **Google Cloud Storage (GCS)**, where data is stored in buckets.
- When you create an external table, BigQuery only stores the metadata (e.g., schema, file locations) about the data, while the actual data remains in the GCP Bucket.

---

### **Why Not the Other Options?**
1. **BigQuery**: Data is not stored in BigQuery storage because external tables are designed to query data directly from external sources.
2. **Container Registry**: This is used for storing Docker container images, not data for querying.
3. **Bigtable**: This is a NoSQL database service in GCP, not the storage location for external tables in BigQuery.

---

### **Correct Answer: GCP Bucket**

8. It is best practice in Big Query to always cluster your data:

- True
- False

The correct answer is: **False**.

---

### **Explanation:**
While **clustering** can significantly improve query performance and reduce costs in BigQuery, it is **not always necessary or the best practice** to cluster your data. Here’s why:

1. **When to Use Clustering**:
   - Clustering is useful for large tables where queries frequently filter or aggregate data based on specific columns (e.g., `date`, `customer_id`, `region`).
   - It helps reduce the amount of data scanned by organizing data into sorted blocks based on the clustered columns.

2. **When Clustering is Not Needed**:
   - For small tables, clustering may not provide significant benefits.
   - If your queries do not frequently filter or aggregate data based on specific columns, clustering may not be useful.
   - Clustering adds some overhead during data ingestion, so it’s not always the best choice for all use cases.

3. **Best Practice**:
   - Use clustering when it aligns with your query patterns and table size.
   - Evaluate whether clustering will actually improve performance and reduce costs for your specific workload.

---

### **Conclusion:**
Clustering is a powerful tool, but it is **not always required** or the best practice for every table in BigQuery. The decision to cluster should be based on your specific use case and query patterns.

**Answer: False**
