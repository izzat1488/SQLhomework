-- Создание таблицы Staff и наполнение данными
CREATE TABLE Staff (
    StaffID INT PRIMARY KEY,
    FullName VARCHAR(100),
    Department VARCHAR(50),
    Salary DECIMAL(10,2)
);

INSERT INTO Staff (StaffID, FullName, Department, Salary)
VALUES
(1, 'Alice Johnson', 'HR', 50000),
(2, 'Bob Smith', 'IT', 65000),
(3, 'Charlie Brown', 'Finance', 70000),
(4, 'David Wilson', 'IT', 68000),
(5, 'Eve Adams', 'HR', 55000);

-- Создание представления для сотрудников IT
CREATE VIEW Vw_IT AS
SELECT * FROM Staff WHERE Department = 'IT';

SELECT * FROM Vw_IT;

-- Создание таблицы Sales и наполнение данными
CREATE TABLE Sales (
    SaleID INT PRIMARY KEY,
    ProductName VARCHAR(100),
    QuantitySold INT,
    SaleDate DATE
);

INSERT INTO Sales (SaleID, ProductName, QuantitySold, SaleDate)
VALUES
(1, 'Laptop', 5, '2024-03-01'),
(2, 'Mouse', 10, '2024-03-02'),
(3, 'Keyboard', 8, '2024-03-02'),
(4, 'Monitor', 3, '2024-03-03'),
(5, 'Laptop', 2, '2024-03-04');

SELECT * FROM Sales;

-- Создание временной таблицы и вставка данных за 2024-03-01
CREATE TABLE #Temp_sales (
    SaleID INT PRIMARY KEY,
    ProductName VARCHAR(100),
    QuantitySold INT,
    SaleDate DATE
);

INSERT INTO #Temp_sales
SELECT * FROM Sales WHERE SaleDate = '2024-03-01';

SELECT * FROM #Temp_sales;

-- Создание таблицы Orders и наполнение данными
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    CustomerID INT,
    OrderDate DATE,
    TotalAmount DECIMAL(10,2)
);

INSERT INTO Orders (OrderID, CustomerID, OrderDate, TotalAmount)
VALUES
(1, 101, '2024-03-01', 150),
(2, 101, '2024-03-05', 120),
(3, 101, '2024-03-10', 600),
(4, 101, '2024-03-15', 650), 
(5, 101, '2024-03-20', 700),
(6, 102, '2024-03-02', 200),
(7, 102, '2024-03-06', 250),
(8, 102, '2024-03-11', 800),
(9, 103, '2024-03-05', 90),
(10, 103, '2024-03-10', 100),
(11, 103, '2024-03-15', 110);

SELECT * FROM Orders;

-- Создание временной таблицы и отбор заказов с суммой выше средней
CREATE TABLE #temp_orders (
    CustomerID INT,
    TotalAmount DECIMAL(10,2)
);

INSERT INTO #temp_orders
SELECT O.CustomerID, O.TotalAmount
FROM Orders O
JOIN (SELECT CustomerID, AVG(TotalAmount) AS AvgTotal FROM Orders GROUP BY CustomerID) AS AvgData
ON O.CustomerID = AvgData.CustomerID AND O.TotalAmount > AvgData.AvgTotal;

SELECT * FROM #temp_orders;

-- Работа с таблицей Numbers
CREATE TABLE Numbers (
    NumberID INT PRIMARY KEY,
    Value INT
);

INSERT INTO Numbers (NumberID, Value)
VALUES
(1, 5),
(2, 10),
(3, 15),
(4, 25), -- Пропущено 20
(5, 30),
(6, 35),
(7, 40);

SELECT * FROM Numbers;

-- Поиск пропущенного числа
DECLARE @MissingValue INT;
SELECT @MissingValue = MIN(N1.Value + 5)
FROM Numbers N1
LEFT JOIN Numbers N2 ON N1.Value + 5 = N2.Value
WHERE N2.Value IS NULL;

SELECT @MissingValue AS MissedNumber;

-- Добавление новых значений в Numbers
INSERT INTO Numbers VALUES (8, 7), (9, 14), (10, 21), (11, 35), (12, 42), (13, 49);

-- Очистка таблицы Numbers
TRUNCATE TABLE Numbers;

-- Работа с таблицей Clients
CREATE TABLE Clients (
    ClientID INT PRIMARY KEY,
    ClientName VARCHAR(100)
);

INSERT INTO Clients (ClientID, ClientName)
VALUES
(101, 'John Doe'),
(102, 'Jane Smith'),
(103, 'Michael Brown');

-- Создание временной таблицы и вставка данных
CREATE TABLE #ClientDataTemp (
    ClientID INT PRIMARY KEY,
    ClientName VARCHAR(100)
);

INSERT INTO #ClientDataTemp (ClientID, ClientName)
VALUES
(101, 'John Doe'),
(104, 'Samuel Green'),
(105, 'Laura White');

SELECT * FROM Clients;
SELECT * FROM #ClientDataTemp;

-- Синхронизация данных между Clients и #ClientDataTemp
MERGE INTO Clients AS Target
USING #ClientDataTemp AS Source
ON Target.ClientID = Source.ClientID
WHEN MATCHED THEN 
    UPDATE SET Target.ClientName = Source.ClientName
WHEN NOT MATCHED BY TARGET THEN 
    INSERT (ClientID, ClientName) VALUES (Source.ClientID, Source.ClientName)
WHEN NOT MATCHED BY SOURCE THEN 
    DELETE;
