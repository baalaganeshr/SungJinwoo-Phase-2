# Day 16: Combine Two Tables (14/7/25)

## Problem Statement
Given two tables `Person` and `Address`, write a SQL query to report each person's first name, last name, city, and state. Include all persons from the Person table even if they don't have an address.

**Tables:**
```sql
Person (personId, lastName, firstName)
Address (addressId, personId, city, state)


SELECT 
    p.firstName,
    p.lastName,
    a.city,
    a.state
FROM 
    Person p
LEFT JOIN 
    Address a ON p.personId = a.personId
