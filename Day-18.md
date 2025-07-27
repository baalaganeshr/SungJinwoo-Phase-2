# Day-18 | 16/7/25  
LeetCode 182. Duplicate Emails (Easy)

---

## 🎯 Problem in one line
Return every **email address** that appears **more than once** in the `Person` table.

---

## ✅ SQL Solution (GROUP BY + HAVING)

```sql
SELECT email AS Email
FROM Person
GROUP BY email
HAVING COUNT(*) > 1;
