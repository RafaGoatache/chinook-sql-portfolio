# 🎵 Chinook SQL Analytics Portfolio

**Database / Base de Datos:** Chinook Sample Database (`chinook.db` / SQLite)  
**Tools Used / Herramientas:** SQL (SQLite)  
**Author / Autor:** Rafael Goatache  

---

## 📊 Business Cases & SQL Queries

### 🌐 Non-US Customers Analysis
**ES:** Presentar una tabla con los nombres completos y países, de todos los clientes que no provengan de EEUU.  
**EN:** *List full names and countries of all non-US customers.*

```sql
SELECT 
    FirstName || ' ' || LastName AS NombreCompleto, 
    Country 
FROM customers 
WHERE Country NOT IN ('USA');
```
### 🇧🇷 Brazilian Customers Analysis
**ES:** Presentar una tabla con los nombres completos y países, de todos los clientes que provengan de Brasil.  
**EN:** *List full names and countries of all customers from Brazil.*

```sql
SELECT 
    FirstName || ' ' || LastName AS NombreCompleto,
    Country
FROM customers 
WHERE Country = 'Brazil';
```
### 🇫🇷 French Customers Invoices Analysis
**ES:** Presentar una tabla con los nombres completos de los clientes, ID de factura, fecha de factura y país de facturación, de todos los clientes que provengan de Francia.  
**EN:** *List customer full names, invoice IDs, invoice dates, and billing countries for all clients located in France.*

```sql
SELECT 
    C.FirstName || ' ' || C.LastName AS NombreCompleto, 
    I.InvoiceId, 
    DATE(I.InvoiceDate) AS FechaFactura,
    I.BillingCountry
FROM customers C
JOIN invoices I 
    ON C.CustomerId = I.CustomerId 
WHERE C.Country = 'France';
```
### 💼 Employee Job Titles Analysis
**ES:** Presentar una tabla de todos los distintos roles que tienen los empleados.  
**EN:** *List all distinct employee job titles.*

```sql
SELECT DISTINCT 
    Title AS Roles 
FROM employees;
```
### 🎧 Sales Support Agents Analysis
**ES:** Presentar una tabla con los nombres completos de los empleados que son ‘Sales Support Agent’.  
**EN:** *List the full names of all employees holding the 'Sales Support Agent' role.*

```sql
SELECT 
    FirstName || ' ' || LastName AS NombreCompleto,
    Title 
FROM employees
WHERE Title = 'Sales Support Agent';
```
### 🌍 Invoice Volume by Country Analysis
**ES:** Presentar una tabla con el número de facturas por país, de forma descendente.  
**EN:** *List total invoice count by country in descending order.*

```sql
SELECT 
    BillingCountry AS Pais,
    COUNT(InvoiceId) AS NumeroFacturas 
FROM Invoices 
GROUP BY BillingCountry 
ORDER BY NumeroFacturas DESC;
```
### 🎵 Average Price by Genre Analysis
**ES:** Presentar una tabla con el precio promedio por género, de forma descendente por precio promedio.  
**EN:** *List average price by genre, sorted in descending order by average price.*

```sql
SELECT 
    G.Name AS Genero,
    ROUND(AVG(T.UnitPrice), 2) AS PrecioPromedio
FROM tracks T
JOIN genres G
    ON T.GenreId = G.GenreId 
GROUP BY G.Name, G.GenreId
ORDER BY PrecioPromedio DESC;
```
### 💼 Sales Support Agents Total Revenue Analysis
**ES:** Presentar una tabla con los empleados que tienen el rol de ‘Sales Support Agent’ y su facturación, ordenando de forma descendente por facturación.  
**EN:** *List Sales Support Agents and their total sales, ordered by sales in descending order.*

```sql
SELECT 
    E.EmployeeId,    
    E.FirstName || ' ' || E.LastName AS NombreCompleto,
    ROUND(SUM(I.Total), 2) AS FacturacionTotal	
FROM employees E	
JOIN customers C 
    ON E.EmployeeId = C.SupportRepId
JOIN invoices I 
    ON C.CustomerId = I.CustomerId	
WHERE E.Title = 'Sales Support Agent'
GROUP BY E.FirstName, E.LastName, E.EmployeeId
ORDER BY FacturacionTotal DESC;
```
### 📜 Track Count per Playlist Analysis
**ES:** Presentar una tabla con la cantidad de tracks en cada playlist.  
**EN:** *List the total number of tracks in each playlist.*

```sql
SELECT 
    P.PlaylistId,    
    P.Name AS Nombre,
    COUNT(PT.TrackId) AS CantidadTracks
FROM playlists P
JOIN playlist_track PT
    ON P.PlaylistId = PT.PlaylistId     
GROUP BY P.PlaylistId, P.Name
ORDER BY P.PlaylistId;
```
Markdown
### 📅 Daily Sales Volume (Jan 1-15, 2010)
**ES:** Presentar una tabla con la cantidad de ventas por día para los primeros 15 días de enero de 2010.  
**EN:** *List daily sales volume for the first 15 days of January 2010.*

```sql
SELECT
    DATE(InvoiceDate) AS Fecha,    
    COUNT(InvoiceId) AS CantidadVentas
FROM invoices
WHERE DATE(InvoiceDate) BETWEEN '2010-01-01' AND '2010-01-15'     
GROUP BY DATE(InvoiceDate) 
ORDER BY Fecha;
```
### 📈 Yearly Sales Comparison (2009 vs 2013)
**ES:** ¿Cuántas facturas hubo en 2009 y cuántas en 2013? ¿Cuánto fue la venta total por cada uno de esos años?  
**EN:** *Calculate total invoices and total revenue for 2009 and 2013.*

```sql
SELECT
    strftime('%Y', InvoiceDate) AS Fecha,    
    COUNT(InvoiceID) AS FacturaTotal,
    ROUND(SUM(Total), 2) AS VentaTotal
FROM invoices 
WHERE strftime('%Y', InvoiceDate) IN ('2009', '2013') 
GROUP BY strftime('%Y', InvoiceDate) 
ORDER BY Fecha;
```
### 🧾 Invoice Line Items Count (Invoice ID 37)
**ES:** ¿Cuántas líneas tuvo la factura con el ID 37?  
**EN:** *Calculate the total number of line items for invoice ID 37.*

```sql
SELECT 
    InvoiceId,
    COUNT(InvoiceLineId) AS TotalLineas
FROM invoice_items 
WHERE InvoiceId = '37' 
GROUP BY InvoiceId;
```
### 🧾 Total Line Items per Invoice Analysis
**ES:** ¿Cuántas líneas tuvo cada ID de factura?  
**EN:** *List the total number of line items per invoice ID.*

```sql
SELECT 
    InvoiceID AS ID,
    COUNT(InvoiceLineId) AS Lineas
FROM invoice_items 
GROUP BY InvoiceID 
ORDER BY ID;
```
### 📊 Invoices Exceeding Average Value Analysis
**ES:** Seleccionar los ID de facturas y sus totales, en tanto estén por encima del promedio de valores facturados.  
**EN:** *List invoice IDs and total amounts that exceed the overall average invoice value.*

```sql
SELECT 
    InvoiceId AS ID, 
    Total AS Totales
FROM invoices
WHERE Total > (SELECT AVG(Total) FROM invoices)
ORDER BY Totales DESC;
```
### 🏆 Top Spending Customers Analysis
**ES:** Presentar una tabla con los ID de clientes, sus nombres completos y el total que gastaron, ordenado de forma descendente por gasto.  
**EN:** *List customer IDs, full names, and total amount spent, ordered by total spend in descending order.*

```sql
SELECT 
    C.CustomerId,
    C.FirstName || ' ' || C.LastName AS NombreCompleto,
    ROUND(SUM(I.Total), 2) AS TotalGasto
FROM customers C
JOIN invoices I 
    ON C.CustomerId = I.CustomerId 
GROUP BY C.CustomerId, C.FirstName, C.LastName
ORDER BY TotalGasto DESC;
```
### 📊 Customer Purchasing Metrics & Average Tracks Analysis
**ES:** Calcular la cantidad de clientes con compra, la cantidad de tracks comprados, y con estos resultados, estimar el promedio de tracks comprados por cliente (con números enteros, sin decimales).  
**EN:** *Calculate the number of purchasing customers, total tracks purchased, and the estimated average tracks purchased per customer (rounded to the nearest integer).*

```sql
WITH RESUME AS (
    SELECT 
        COUNT(DISTINCT I.CustomerId) AS CantidadClientes,
        SUM(IT.Quantity) AS TracksComprados
    FROM invoices I
    JOIN invoice_items IT
        ON I.InvoiceId = IT.InvoiceId
)
SELECT
    CantidadClientes,
    TracksComprados, 
    ROUND((1.0 * TracksComprados / CantidadClientes), 0) AS PromedioTracksComprados 
FROM RESUME;
```
### 📅 Monthly Revenue Analysis (2009)
**ES:** Presentar una tabla con lo que se facturó cada mes durante 2009.  
**EN:** *List total monthly revenue for the year 2009.*

```sql
SELECT 
    Strftime('%Y-%m', InvoiceDate) AS Fecha, 
    ROUND(SUM(Total), 2) AS Facturacion
FROM invoices
WHERE Date(InvoiceDate) BETWEEN '2009-01-01' AND '2009-12-31'
GROUP BY Strftime('%Y-%m', InvoiceDate)
ORDER BY Fecha;
```
## 🛠️ Tools & Environment
* **Database:** Chinook Sample Database (SQLite)
* **Dialect:** SQL (SQLite)
* **Documentation:** Markdown (Bilingual ES/EN)

---

## 📬 Contact
* **GitHub:** https://github.com/RafaGoatache
* **LinkedIn:** www.linkedin.com/in/rafael-goatache-lara-administrador
