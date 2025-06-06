

# 🔍 Why We Use Full-Text Search Instead of `icontains` (or `LIKE '%query%'`)

## 📌 Problem Statement

In our application, we have a large dataset (100,000+ rows) where users need to **search for text-based fields** such as:

* `plot_code`
* `plot_categ`

Initially, we implemented search using Django’s `icontains` filter, which translates to SQL `ILIKE '%query%'`. This approach is simple but presents **serious performance issues** as data volume grows.

---

## ⚠️ Limitations of `icontains` or `ILIKE '%query%'`

### 1. **No Index Usage**

PostgreSQL cannot use a normal B-Tree index with `LIKE '%abc%'` or `icontains` because the wildcard `%` at the beginning makes it impossible to predict where to look.

```sql
SELECT * FROM plots WHERE plot_code ILIKE '%ABC%';
```

* This results in a **sequential scan** of the entire table.
* It checks every row’s value character-by-character.
* **Time complexity: O(n)** (linear scan across all rows).

### 2. **Slower Search as Data Grows**

With 100,000+ rows, this becomes:

* CPU-intensive
* Memory-consuming
* Poor user experience (high latency)

---

## ✅ Full-Text Search: The Solution

PostgreSQL’s Full-Text Search (FTS) is designed for **efficient textual matching** and **word-based querying**. It breaks down text into **tokens**, builds an **inverted index**, and allows fast searching using that index.

### Example

Suppose `plot_code = 'Institutional Area ABC-001'`

This is converted internally to tokens using `to_tsvector('english', plot_code)`:

```text
'institutional':1, 'area':2, 'abc':3, '001':4
```

Now, if we query using `plainto_tsquery('english', 'abc')`, the engine **matches the token directly** using the GIN (Generalized Inverted Index), resulting in:

* **Fast lookup**
* **No full table scan**
* **Highly scalable**

---

## 🔄 Comparison Table

| Feature                   | `icontains`/`ILIKE` | Full-Text Search                      |
| ------------------------- | ------------------- | ------------------------------------- |
| Index Usage               | ❌ No index used     | ✅ Uses GIN index                      |
| Performance on large data | ❌ Poor              | ✅ Fast and optimized                  |
| Matching capability       | ✅ Substring search  | ✅ Word/token-based, smarter           |
| Search for typos          | ❌ No                | ❌ No (FTS ≠ fuzzy search, but faster) |
| Ranking or relevance      | ❌ No                | ✅ Yes (if using `ts_rank`)            |

---

## 🧪 Performance Benchmarks (Illustrative)

| Dataset Size | `icontains` Avg Time | Full-Text Search Avg Time |
| ------------ | -------------------- | ------------------------- |
| 10,000 rows  | 300–500ms            | 20–50ms                   |
| 100,000 rows | 1.2–2.5s             | 40–70ms                   |

> *Actual numbers may vary based on infrastructure, indexing, and query tuning.*

---

## 🏗 How We Implemented It

### ✅ Create GIN Index

```sql
-- For plot_code
CREATE INDEX idx_plot_code_ft ON plots USING gin(to_tsvector('english', plot_code));

-- For plot_categ
CREATE INDEX idx_plot_categ_ft ON plots USING gin(to_tsvector('english', plot_categ));
```

### ✅ Use in Query

```sql
SELECT * FROM plots
WHERE to_tsvector('english', plot_code) @@ plainto_tsquery('english', 'ABC');
```

Or in Django ORM:

```python
from django.contrib.postgres.search import SearchVector, SearchQuery

Plot.objects.annotate(
    search=SearchVector('plot_code')
).filter(
    search=SearchQuery('abc')
)
```

---

## ✅ Final Benefits

* **Significant performance boost** in large-scale datasets.
* Better scalability and response time.
* PostgreSQL-native solution — no external search engine (like Elasticsearch) required.
* Clean, maintainable, and extensible search logic.

---

## 📌 Conclusion

We adopted **PostgreSQL Full-Text Search** to overcome the performance limitations of `icontains`-based searching. It ensures our application can handle large datasets efficiently, delivering a smoother and faster user experience.

> This change is justified not just for performance but also for future scalability, relevance-based results, and maintainability.


