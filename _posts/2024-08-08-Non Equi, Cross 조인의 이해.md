---
title: Non Equi, Cross 조인의 이해
date: 2024-08-08 21:30:00 +09:00
published: true
toc_sticky: true
categories: [개발, DB]
tags: [Postgre, Spring Boot, Mybatis]
---

## Non Equi 조인

- Equi 조인과 다르게 between, >, >=, < 등의 연산자를 사용하는 조인

    ![non equi join](/assets/img/join-2/non_equi_join.png)

## Cross Join

- Cartesian Product 조인

- 조인 컬럼없이 두 테이블 간 가능한 모든 연결을 결합하는 조인 방식

    ![crossjoin](/assets/img/join-2/cross_join.png)

### 실습 문제 풀기

#### 실습 환경에 대한 설명

- 이전 포스트인 Join, join-2의 테이블 사용
- 추가로 **salgrade**라는 테이블 사용

    - [조인 기반 메커니즘](2024-07-30-JOIN%20기반%20메커니즘.md)
    - [조인 기반 메커니즘 - 2](2024-08-05-JOIN%20기반%20메커니즘_2.md)
    ![salgrade](/assets/img/join-2/join_quiz.png)

#### 실습 문제

- 직원정보와 급여등급 정보를 추출. 

    ```sql
    select emp.*, salgrade.grade as sal_grade, salgrade.losal , salgrade.hisal 
    from hr.emp as emp
        join hr.salgrade as salgrade on emp.sal between salgrade.losal and salgrade.hisal 
    ```

- 직원 급여의 이력정보를 나타내며, 해당 급여를 가졌던 시점에서의 부서번호도 함께 가져올것. 

    ```sql
    select sal_hist.*, dept_hist.*
    from hr.emp_salary_hist as sal_hist
        join hr.emp_dept_hist as dept_hist on dept_hist.empno = sal_hist.empno 
            and sal_hist.fromdate between dept_hist.fromdate and dept_hist.todate 
    ```

- cross 조인(예시)

    ```sql
    with
    temp_01 as (
        select 1 as rnum
        union
        select 2 as rnum
    )
    select dept.*, temp_01.*
    from hr.dept
        cross join temp_01
    ```