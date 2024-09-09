
# The Company
[**HackerRank Link**](https://www.hackerrank.com/challenges/the-company?isFullScreen=true)

---

## Question Explanation:
The task is to retrieve details about companies, including the number of lead managers, senior managers, managers, and employees in each company. The results should be grouped by company and ordered by `company_code`.

---

## Sample Tables:
- `company`: Contains `company_code` and `founder`, representing each company and its founder.
- `lead_manager`: Contains `lead_manager_code` and `company_code`, linking lead managers to companies.
- `senior_manager`: Contains `senior_manager_code` and `lead_manager_code`, linking senior managers to lead managers.
- `manager`: Contains `manager_code` and `senior_manager_code`, linking managers to senior managers.
- `employee`: Contains `employee_code` and `manager_code`, linking employees to managers.

---

## Answer:
Here’s the SQL query that solves the problem:

```sql
select c.company_code, c.founder, count(distinct lm.lead_manager_code), 
       count(distinct sm.senior_manager_code), 
       count(distinct m.manager_code), 
       count(distinct e.employee_code)
from company c
join lead_manager lm
on c.company_code = lm.company_code
join senior_manager sm
on lm.lead_manager_code = sm.lead_manager_code
join manager m
on sm.senior_manager_code = m.senior_manager_code
join employee e
on m.manager_code = e.manager_code
group by c.company_code, c.founder
order by c.company_code;
```

---

## Answer Explanation:
1. The query joins multiple tables: `company`, `lead_manager`, `senior_manager`, `manager`, and `employee`, to retrieve the hierarchical structure within each company.
2. For each company (`company_code`), it counts distinct lead managers, senior managers, managers, and employees.
3. The results are grouped by `company_code` and `founder`, and the output is ordered by `company_code`.
