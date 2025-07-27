# Day-17 | 15/7/25  
LeetCode 181. Employees Earning More Than Their Managers (Easy)

---

## 🎯 Problem in one line
Return the **names** of employees whose salary is **strictly higher** than their **direct manager’s** salary.

---

## ✅ SQL Solution (self-join)

```sql
SELECT e1.name AS Employee
FROM Employee e1
JOIN Employee e2
  ON e1.managerId = e2.id
WHERE e1.salary > e2.salary;
