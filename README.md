-- Create the table to store city, year, month, and sales data
CREATE TABLE Table1 (
    City VARCHAR(50),
    Year INT,
    Month INT,
    Sales INT
);

-- Insert the provided sales data into the table
INSERT INTO Table1 (City, Year, Month, Sales) VALUES
('Delhi', 2020, 5, 4300),
('Delhi', 2020, 6, 2000),
('Delhi', 2020, 7, 2100),
('Delhi', 2020, 8, 2200),
('Delhi', 2020, 9, 1900),
('Delhi', 2020, 10, 200),
('Mumbai', 2020, 5, 4400),
('Mumbai', 2020, 6, 2800),
('Mumbai', 2020, 7, 6000),
('Mumbai', 2020, 8, 9300),
('Mumbai', 2020, 9, 4200),
('Mumbai', 2020, 10, 9700),
('Bangalore', 2020, 5, 1000),
('Bangalore', 2020, 6, 2300),
('Bangalore', 2020, 7, 6800),
('Bangalore', 2020, 8, 7000),
('Bangalore', 2020, 9, 2300),
('Bangalore', 2020, 10, 8400);
WITH SalesData AS (
    SELECT
        City,
        Year,
        Month,
        Sales,
        LAG(Sales) OVER (PARTITION BY City ORDER BY Year, Month) AS PreviousMonthSales,
        LEAD(Sales) OVER (PARTITION BY City ORDER BY Year, Month) AS NextMonthSales,
        SUM(Sales) OVER (PARTITION BY City ORDER BY Year, Month ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS YTDSales
    FROM
        Table1
)
SELECT
    City,
    Year,
    Month,
    Sales,
    COALESCE(PreviousMonthSales, 0) AS PreviousMonthSales,
    COALESCE(NextMonthSales, 0) AS NextMonthSales,
    YTDSales
FROM
    SalesData
ORDER BY
    City, Year, Month;
