-- Creating tables for PH-EmployeeDB

-- Create new table for departments
CREATE TABLE departments (
	dept_no VARCHAR(4) NOT NULL,
    dept_name VARCHAR(40) NOT NULL,
    PRIMARY KEY (dept_no),
    UNIQUE (dept_name)
);

-- Create new table for employees
CREATE TABLE employees (
	emp_no INT NOT NULL, 
	birth_date DATE NOT NULL,
	first_name VARCHAR NOT NULL, 
	last_name VARCHAR NOT NULL, 
	gender VARCHAR NOT NULL,
	hire_date DATE NOT NULL, 
	PRIMARY KEY (emp_no)
);

-- Create new table for department manager
CREATE TABLE dept_manager (
	dept_no VARCHAR(4) NOT NULL,
    emp_no INT NOT NULL,
    from_date DATE NOT NULL,
    to_date DATE NOT NULL,
	FOREIGN KEY (emp_no) REFERENCES employees (emp_no) DEFERRABLE,
	FOREIGN KEY (dept_no) REFERENCES departments (dept_no)DEFERRABLE,
    PRIMARY KEY (emp_no, dept_no)
);

-- Create new table for department employees
CREATE TABLE dept_emp (
	dept_no VARCHAR(4) NOT NULL, 
	emp_no INT NOT NULL,
	from_date DATE NOT NULL,
	to_date DATE NOT NULL, 
	FOREIGN KEY (emp_no) REFERENCES employees (emp_no) DEFERRABLE,
	FOREIGN KEY (dept_no) REFERENCES departments (dept_no) DEFERRABLE,
	PRIMARY KEY (emp_no, dept_no)

-- Create new table for titles	
CREATE TABLE titles (
	emp_no INT NOT NULL,
	title VARCHAR NOT NULL, 
	from_date DATE NOT NULL, 
	to_date DATE NOT NULL, 
	FOREIGN KEY (emp_no) REFERENCES employees (emp_no)DEFERRABLE,
	PRIMARY KEY (emp_no, title, from_date)
);
SELECT * FROM titles;

-- Create new table for salaries	
CREATE TABLE salaries (
  	emp_no INT NOT NULL,
	salary INT NOT NULL,
	from_date DATE NOT NULL,
  	to_date DATE NOT NULL,
  	FOREIGN KEY (emp_no) REFERENCES employees (emp_no)DEFERRABLE,
  	PRIMARY KEY (emp_no, from_date)
);

-- Create new table for retirement_titles	
CREATE TABLE retirement_info(
	emp_no INT NOT NULL, 
	first_name VARCHAR NOT NULL, 
	last_name VARCHAR NOT NULL,
	FOREIGN KEY (emp_no) REFERENCES employees (emp_no)DEFERRABLE,
	PRIMARY KEY (emp_no, last_name)
);	

-- Create new table for retirement_titles
SELECT DISTINCT ON (e.emp_no)
e.emp_no,
e.first_name,
e.last_name,
ti.title,
ti.from_date,
ti.to_date
INTO retirement_titles
FROM titles as ti
INNER JOIN employees as e 
ON e.emp_no = ti.emp_no
WHERE e.birth_date between '1952-01-01' AND '1955-12-31'
ORDER BY e.emp_no;
	
SELECT * FROM retirement_titles;
	
-- Create new table for unique_titles	
SELECT DISTINCT ON (e.emp_no)
e.emp_no,
e.first_name,
e.last_name,
ti.title,
ti.from_date,
ti.to_date	
INTO unique_titles
FROM titles as ti
INNER JOIN employees as e 
ON e.emp_no = ti.emp_no
WHERE ti.to_date between '9999-01-01' AND '9999-01-01'
ORDER BY e.emp_no ASC, ti.to_date DESC;

SELECT * FROM unique_titles;		

-- Create new table for dept_info		
SELECT ce.emp_no,
ce.first_name,
ce.last_name,
d.dept_name
INTO dept_info
FROM current_emp as ce
INNER JOIN dept_emp AS de
ON (ce.emp_no = de.emp_no)
INNER JOIN departments AS d
ON (de.dept_no = d.dept_no);

SELECT * FROM dept_info;
	
-- Create new table for emp_info		
SELECT e.emp_no,
    e.first_name,
	e.last_name,
    e.gender,
    s.salary,
    de.to_date
INTO emp_info
FROM employees as e
INNER JOIN salaries as s
ON (e.emp_no = s.emp_no)
INNER JOIN dept_emp as de
ON (e.emp_no = de.emp_no)
WHERE (e.birth_date BETWEEN '1952-01-01' AND '1955-12-31')
     AND (e.hire_date BETWEEN '1985-01-01' AND '1988-12-31')
	 AND (de.to_date = '9999-01-01');
	 
SELECT * FROM employees;
	
-- Create new table for current_emp	
SELECT ri.emp_no,
    ri.first_name,
    ri.last_name,
	de.to_date
INTO current_emp
FROM retirement_info as ri
LEFT JOIN dept_emp as de
ON ri.emp_no = de.emp_no
WHERE de.to_date = ('9999-01-01');

SELECT * FROM current_emp
	
-- Create new table for manager_info	
SELECT  dm.dept_no,
        d.dept_name,
        dm.emp_no,
        ce.last_name,
        ce.first_name,
        dm.from_date,
        dm.to_date
INTO manager_info
FROM dept_manager AS dm
    INNER JOIN departments AS d
        ON (dm.dept_no = d.dept_no)
    INNER JOIN current_emp AS ce
        ON (dm.emp_no = ce.emp_no);

SELECT * FROM manager_info;	
	
-- Create new table for retiring_titles	
SELECT COUNT (emp_no),
title 
FROM unique_titles as ut
LEFT JOIN titles as ti
ON ut.emp_no = ti.emp_no
GROUP BY ti.title 
ORDER BY ut.unique_title;

-- Create new table for mentorship_eligibility	

SELECT DISTINCT ON (e.emp_no) 
	e.emp_no,
	e.first_name, 
	e.last_name, 
	e.birth_date,
	de.from_date,
	de.to_date, 
	ti.title
INTO mentorship_eligibility
FROM employees as e
INNER JOIN dept_emp as de
	ON (e.emp_no = de.emp_no)
INNER JOIN titles as ti
	ON (e.emp_no = ti.emp_no)
WHERE (birth_date BETWEEN '1965-01-01' AND '1965-12-31') 
AND (de.to_date ='9999-01-01')
ORDER BY e.emp_no;

SELECT * FROM mentorship_eligibility;	
	
	
-- Employee count by department number
SELECT COUNT(ce.emp_no), de.dept_no
FROM current_emp as ce
LEFT JOIN dept_emp as de
ON ce.emp_no = de.emp_no
GROUP BY de.dept_no
ORDER BY de.dept_no;

SELECT * FROM salaries;

SELECT * FROM salaries
ORDER BY to_date DESC;





	
	
	
