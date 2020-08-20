---
title: 牛客 sql
toc: true
recommend: 1
uniqueId: '2020-08-17 09:10:44/"牛客 sql".html'
date: 2020-08-17 17:10:44
thumbnail:
categories:
- leetcode
- 分类
tags:
keywords:
---

[TOC]

<!--more-->



[查找最晚入职员工的所有信息_牛客网](https://www.nowcoder.com/practice/218ae58dfdcd4af195fff264e062138f?tpId=82&&tqId=29753&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

```sqlite
select *
from employees
order by hire_date desc
limit 1  -- 或者 limit 0, 1

```



[查找入职员工时间排名倒数第三的员工所有信息_牛客网](https://www.nowcoder.com/practice/ec1ca44c62c14ceb990c3c40def1ec6c?tpId=82&tqId=29753&rp=1&ru=%2Fta%2Fsql&qru=%2Fta%2Fsql%2Fquestion-ranking)

```sqlite
select *
from employees
order by hire_date desc
limit 2, 1
```



[查找所有已经分配部门的员工的last_name和first_name以及dept_no_牛客网](https://www.nowcoder.com/practice/6d35b1cd593545ab985a68cd86f28671?tpId=82&&tqId=29756&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

三种等效的操作 , == join == inner join

```sqlite
SELECT e.last_name, e.first_name, d.dept_no
FROM employees AS e, dept_emp AS d
WHERE e.emp_no = d.emp_no;
```

```sqlite
SELECT e.last_name, e.first_name, d.dept_no
FROM employees AS e JOIN dept_emp AS d
ON e.emp_no=d.emp_no;
```

```sqlite
SELECT e.last_name, e.first_name, d.dept_no
FROM employees AS e INNER JOIN dept_emp AS d
ON e.emp_no=d.emp_no;
```

左连接：

left join 和 left outer join 是一样的，就是字面上有区别，执行结果是一样的

```sqlite
select last_name, first_name, dept_no
from employees as e left join dept_emp as d
on d.emp_no = e.emp_no
where d.dept_no not null;
```



[查找当前薪水详情以及部门编号dept_no_牛客网](https://www.nowcoder.com/practice/c63c5b54d86e4c6d880e4834bfd70c3b?tpId=82&&tqId=29755&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

```sqlite
select s.*,d.dept_no
from salaries as s inner join dept_manager as d
on s.emp_no=d.emp_no
where d.to_date='9999-01-01' and s.to_date='9999-01-01'
order by s.emp_no
```



[查找所有员工入职时候的薪水情况_牛客网](https://www.nowcoder.com/practice/23142e7a23e4480781a3b978b5e0f33a?tpId=82&&tqId=29758&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

```sqlite
-- group by  + having min(salary)
select emp_no, salary
from salaries
group by emp_no
having min(salary)
order by emp_no desc

-- e.hire_date=s.from_date
-- 注意，加上 s. e. 
select s.emp_no, s.salary
from employees as e inner join salaries as s
on e.emp_no=s.emp_no and e.hire_date=s.from_date
order by s.emp_no desc

```

[查找薪水变动超过15次的员工号emp_no以及其对应的变动次数t_牛客网](https://www.nowcoder.com/practice/6d4a4cff1d58495182f536c548fee1ae?tpId=82&&tqId=29759&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

```sqlite
select emp_no, count(salary) as t
from salaries
group by emp_no
having count(salary)>15

-- 补充：如果考虑严格意义上的涨幅，应该写：
select a.emp_no, count() t
from salaries a inner join salaries b
on a.emp_no=b.emp_no and a.to_date=b.from_date
where a.salary < b.salary
group by a.emp_no
having t>15
```

[找出所有员工当前薪水salary情况_牛客网](https://www.nowcoder.com/practice/ae51e6d057c94f6d891735a48d1c2397?tpId=82&&tqId=29760&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

```sqlite
select distinct salary
from salaries
where to_date='9999-01-01'
order by salary desc
```

[获取所有部门当前manager的当前薪水情况，给出dept_no, emp_no以及salary_牛客网](https://www.nowcoder.com/practice/4c8b4a10ca5b44189e411107e1d8bec1?tpId=82&&tqId=29761&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

```sqlite
select d.dept_no, d.emp_no, s.salary
from dept_manager as d inner join salaries as s
on d.emp_no=s.emp_no 
   and d.to_date='9999-01-01' 
   and s.to_date='9999-01-01'

```

[获取所有非manager的员工emp_no_牛客网](https://www.nowcoder.com/practice/32c53d06443346f4a2f2ca733c19660c?tpId=82&&tqId=29762&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

```sqlite
select emp_no
from employees
where emp_no not in (
    select emp_no
    from dept_manager
)
```

[获取所有员工当前的manager_牛客网](https://www.nowcoder.com/practice/e50d92b8673a440ebdf3a517b5b37d62?tpId=82&&tqId=29763&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

```sqlite
select e.emp_no, m.emp_no
from dept_emp as e inner join dept_manager as m
on e.dept_no=m.dept_no
where e.to_date='9999-01-01'
      and m.to_date='9999-01-01' 
      and e.emp_no<>m.emp_no
```

[获取每个部门中当前员工薪水最高的相关信息_牛客网](https://www.nowcoder.com/practice/4a052e3e1df5435880d4353eb18a91c6?tpId=82&&tqId=29764&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

用子查询，子查询内部可以在 where等地方 连接外部的表

```sqlite
select d.dept_no, d.emp_no, s.salary
from dept_emp as d inner join salaries as s
on d.emp_no=s.emp_no and d.to_date='9999-01-01' and s.to_date='9999-01-01'
where s.salary=(
  select max(s1.salary)
  from dept_emp as d1 inner join salaries as s1
  on d1.emp_no=s1.emp_no and d1.to_date='9999-01-01' and s1.to_date='9999-01-01'
  where d1.dept_no=d.dept_no
  group by d1.dept_no
)
order by d.dept_no asc
```

[从titles表获取按照title进行分组_牛客网](https://www.nowcoder.com/practice/72ca694734294dc78f513e147da7821e?tpId=82&&tqId=29765&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

```sqlite
select title, count() as t
from titles
group by title
having t>1
```

[从titles表获取按照title进行分组，注意对于重复的emp_no进行忽略。_牛客网](https://www.nowcoder.com/practice/c59b452f420c47f48d9c86d69efdff20?tpId=82&&tqId=29766&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

```sqlite
select title, count() as t
from (
    select distinct emp_no, title
    from titles
)
group by title
having t>1
```

补充：当DISTINCT应用到多个字段时，其应用范围是其后面的所有字段，而不是紧挨它的一个字段
注意：DISTINCT只能放在所有字段前面，所以上面的DISTINCT emp_no和title不可以交换

distinct 还能在 count 里面用

```sqlite
select title, count(distinct emp_no) as t
from titles
group by title
having t>1
```

[查找employees表_牛客网](https://www.nowcoder.com/practice/a32669eb1d1740e785f105fa22741d5c?tpId=82&&tqId=29767&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

```sqlite
select *
from employees
where last_name!='Mary'
and emp_no%2=1
order by hire_date desc
```



[统计出当前各个title类型对应的员工当前薪水对应的平均工资_牛客网](https://www.nowcoder.com/practice/c8652e9e5a354b879e2a244200f1eaae?tpId=82&&tqId=29768&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

```sqlite
select title, avg(salary)
from titles as t inner join salaries as s
on t.emp_no=s.emp_no
where t.to_date='9999-01-01' and s.to_date='9999-01-01'
group by title
```



[获取当前薪水第二多的员工的emp_no以及其对应的薪水salary_牛客网](https://www.nowcoder.com/practice/8d2c290cc4e24403b98ca82ce45d04db?tpId=82&&tqId=29769&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

```sqlite
select emp_no, salary
from salaries
where to_date='9999-01-01'
order by salary desc
limit 1, 1
```



[获取当前薪水第二多的员工的emp_no以及其对应的薪水salary_牛客网](https://www.nowcoder.com/practice/c1472daba75d4635b7f8540b837cc719?tpId=82&&tqId=29770&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)



```sqlite
-- 小于薪水最大的最大薪水
select e.emp_no, max(salary), last_name, first_name
from employees as e inner join salaries as s
on e.emp_no=s.emp_no
where to_date='9999-01-01' and salary<(
    select max(salary)
    from salaries
    where to_date='9999-01-01'
)
```



[查找所有员工的last_name和first_name以及对应的dept_name_牛客网](https://www.nowcoder.com/practice/5a7975fabe1146329cee4f670c27ad55?tpId=82&&tqId=29771&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

好像不能直接三个表 join ，比如 a left join b on xx join c on xxx ，这是错误的

```sqlite
select e.last_name, e.first_name, t.dept_name
from employees as e left join (
    select emp_no, dept_name
    from dept_emp as de inner join departments as d
    on d.dept_no=de.dept_no
) as t
on e.emp_no=t.emp_no
```



[查找员工编号emp_no为10001其自入职以来的薪水salary涨幅(总共涨了多少)growth_牛客网](https://www.nowcoder.com/practice/c727647886004942a89848e2b5130dc2?tpId=82&&tqId=29772&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

```sqlite

```











