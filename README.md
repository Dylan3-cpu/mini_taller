
# Mini Taller - Funciones en MySQL

Este proyecto contiene la creación y uso de **funciones definidas por el usuario (UDF)** en MySQL aplicadas a diferentes tablas. Cada función resuelve una necesidad específica del negocio, como calcular bonificaciones, edades, clasificación de precios, y formato de teléfonos.

---

## 🔹 1. Función `calculate_bonus`

Calcula la bonificación basada en el salario del empleado.

```sql
DELIMITER //

CREATE FUNCTION calculate_bonus(salary DECIMAL(10,2)) 
RETURNS DECIMAL(10,2)
DETERMINISTIC
BEGIN
    DECLARE bonus DECIMAL(10,2);

    IF salary < 2000 THEN
        SET bonus = salary * 0.10;
    ELSEIF salary <= 5000 THEN
        SET bonus = salary * 0.07;
    ELSE
        SET bonus = salary * 0.05;
    END IF;

    RETURN bonus;
END //

DELIMITER ;
```

**Tabla y prueba:**

```sql
CREATE TABLE employees (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50),
    salary DECIMAL(10,2)
);

INSERT INTO employees (name, salary) VALUES
('Checo Perez', 1500.00),
('Niki Lauda', 3000.00),
('Max Verstappend', 6000.00);

SELECT 
    name,
    salary,
    calculate_bonus(salary) AS bonus
FROM employees;
```

---

## 🔹 2. Función `calculate_age`

Calcula la edad actual en años a partir de una fecha de nacimiento.

```sql
DELIMITER //

CREATE FUNCTION calculate_age(birth_date DATE) 
RETURNS INT
DETERMINISTIC
BEGIN
    RETURN TIMESTAMPDIFF(YEAR, birth_date, CURDATE());
END //

DELIMITER ;
```

**Tabla y prueba:**

```sql
CREATE TABLE customers (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50),
    birth_date DATE
);

INSERT INTO customers (name, birth_date) VALUES
('Luis Pontifica', '1990-06-15'),
('Maria Rudencinda', '1985-09-20'),
('Pedro Picapiedra', '2000-03-10');

SELECT 
    name,
    birth_date,
    calculate_age(birth_date) AS age
FROM customers;
```

---

## 🔹 3. Función `format_phone`

Formatea un número de teléfono en el formato `(XXX) XXX-XXXX`.

```sql
DELIMITER //

CREATE FUNCTION format_phone(number VARCHAR(10)) 
RETURNS VARCHAR(14)
DETERMINISTIC
BEGIN
    DECLARE formatted_phone VARCHAR(14);

    SET formatted_phone = CONCAT(
        '(', SUBSTRING(number, 1, 3), ') ',
        SUBSTRING(number, 4, 3), '-', 
        SUBSTRING(number, 7, 4)
    );

    RETURN formatted_phone;
END //

DELIMITER ;
```

**Tabla y prueba:**

```sql
CREATE TABLE contacts (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50),
    phone VARCHAR(10)
);

INSERT INTO contacts (name, phone) VALUES
('Laura Torres', '3214567890'),
('Andres Mejillas', '3019876543'),
('Sofia Pereza', '3151234567');

SELECT 
    name,
    phone,
    format_phone(phone) AS formatted_phone
FROM contacts;
```

---

## 🔹 4. Función `classify_price`

Clasifica un precio como `'Low'`, `'Medium'` o `'High'`.

```sql
DELIMITER //

CREATE FUNCTION classify_price(price DECIMAL(10,2)) 
RETURNS VARCHAR(10)
DETERMINISTIC
BEGIN
    DECLARE category VARCHAR(10);

    IF price < 50 THEN
        SET category = 'Low';
    ELSEIF price <= 200 THEN
        SET category = 'Medium';
    ELSE
        SET category = 'High';
    END IF;

    RETURN category;
END //

DELIMITER ;
```

**Tabla y prueba:**

```sql
CREATE TABLE products (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50),
    price DECIMAL(10,2)
);

INSERT INTO products (name, price) VALUES
('USB 2000 GB', 25.00),
('Mechanical Keyboard Impact', 120.00),
('Gaming Laptop PC 360', 950.00);

SELECT 
    name,
    price,
    classify_price(price) AS category
FROM products;
```

---

## 📌 Autor

- Dylan Rodríguez  
- [Repositorio del proyecto](https://github.com/Dylan3-cpu/mini_taller)

---


