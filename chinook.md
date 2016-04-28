//1. Provide a query showing Customers (just their full names, customer ID and country) who are not in the US.

SELECT FirstName, LastName, CustomerId, Country
FROM Customer
WHERE Country != "USA";

//2. Provide a query only showing the Customers from Brazil.

SELECT FirstName, LastName, CustomerId, Country
FROM Customer
WHERE Country = "Brazil";

//3. Provide a query showing the Invoices of customers who are from Brazil. The resultant table should show the customer's full name, Invoice ID, Date of the invoice and billing country.

SELECT FirstName, LastName, InvoiceId, InvoiceDate, BillingCountry
FROM Invoice
INNER JOIN Customer
WHERE Country = "Brazil";

//4. Provide a query showing only the Employees who are Sales Agents.

SELECT FirstName, LastName, Title
FROM Employee
WHERE Title = "Sales Support Agent";

//5. Provide a query showing a unique list of billing countries from the Invoice table.

SELECT DISTINCT BillingCountry
FROM Invoice;

//6. Provide a query that shows the invoices associated with each sales agent. The resultant table should include the Sales Agent's full name.

SELECT FirstName, LastName, InvoiceId
FROM Invoice
INNER JOIN Employee
WHERE Title = "Sales Support Agent";

//7. Provide a query that shows the Invoice Total, Customer name, Country and Sale Agent name for all invoices and customers.

SELECT Total, 
Customer.FirstName AS CustFirstName, 
Customer.LastName AS CustLastName, 
Employee.FirstName AS EmpFirstName, 
Employee.LastName AS EmpLastName, 
Customer.Country AS CustCountry, 
SupportRepId
FROM Invoice
INNER JOIN Customer ON Customer.CustomerId = Invoice.CustomerId
INNER JOIN Employee ON Employee.EmployeeId = Customer.SupportRepId;

//8. How many Invoices were there in 2009 and 2011? What are the respective total sales for each of those years?(include both the answers and the queries used to find the answers)

SELECT SUM(Invoice.Total) AS "2009 Sales Total"
FROM Invoice
WHERE Invoice.InvoiceDate LIKE '%2009%'

SELECT SUM(Invoice.Total) AS "2011 Sales Total"
FROM Invoice
WHERE Invoice.InvoiceDate LIKE '%2011%'

//9. Looking at the InvoiceLine table, provide a query that COUNTs the number of line items for Invoice ID 37.

SELECT COUNT(InvoiceId)
FROM InvoiceLine
WHERE InvoiceId = 37;

//10. Looking at the InvoiceLine table, provide a query that COUNTs the number of line items for each Invoice. HINT: GROUP BY

SELECT InvoiceId, COUNT(Quantity)
FROM InvoiceLine
GROUP BY InvoiceId

//11. Provide a query that includes the track name with each invoice line item.

SELECT InvoiceLineId, Track.Name
FROM Track
INNER JOIN InvoiceLine ON InvoiceLine.TrackId = Track.TrackId

//12. Provide a query that includes the purchased track name AND artist name with each invoice line item.

SELECT InvoiceLineId, Track.Name AS TrackName, Artist.Name AS ArtistName
FROM Track
INNER JOIN InvoiceLine ON InvoiceLine.TrackId = Track.TrackId
INNER JOIN Album ON Album.AlbumId = Track.AlbumId
INNER JOIN Artist ON Artist.ArtistId = Album.AlbumId

//13. Provide a query that shows the # of invoices per country. HINT: GROUP BY

SELECT Count(InvoiceId) AS NumberOfInvoices, BillingCountry
FROM Invoice
GROUP BY BillingCountry

//14. Provide a query that shows the total number of tracks in each playlist. The Playlist name should be included on the resulant table.

SELECT pl.Name, COUNT(pt.TrackId)
FROM Playlist pl
INNER JOIN PlaylistTrack pt  ON  pt.PlaylistId = pl.PlaylistId
GROUP BY pl.Name

//15. Provide a query that shows all the Tracks, but displays no IDs. The resultant table should include the Album name, Media type and Genre.

SELECT Album.Title AS AlbumTitle, Track.Name AS TrackName, MediaType.Name AS MediaType, Genre.Name AS Genre
FROM Track
INNER JOIN Album ON Album.AlbumId = Track.AlbumId
INNER JOIN MediaType ON MediaType.MediaTypeId = Track.MediaTypeId
INNER JOIN Genre ON Genre.GenreId = Track.GenreId

//16. Provide a query that shows all Invoices but includes the # of invoice line items.

SELECT Invoice.InvoiceId, Count(InvoiceLine.InvoiceLineId) AS NumberOfInvoiceLines
FROM Invoice
INNER JOIN InvoiceLine ON Invoice.InvoiceId = InvoiceLine.InvoiceId
GROUP BY Invoice.InvoiceId

//17. Provide a query that shows total sales made by each sales agent.

SELECT Employee.FirstName, Employee.LastName, SUM(Invoice.Total)
FROM Invoice
INNER JOIN Customer ON Customer.CustomerId = Invoice.CustomerId
INNER JOIN Employee ON Employee.EmployeeId = Customer.SupportRepId
GROUP BY Employee.EmployeeId

//18. Which sales agent made the most in sales in 2009? HINT: MAX

SELECT Employee.FirstName, Employee.LastName, SUM(Invoice.Total) AS TotalSales, Invoice.InvoiceDate
FROM Invoice
INNER JOIN Customer ON Customer.CustomerId = Invoice.CustomerId
INNER JOIN Employee ON Employee.EmployeeId = Customer.SupportRepId
WHERE InvoiceDate LIKE '%2009%'
ORDER BY TotalSales
LIMIT 1

//19. Which sales agent made the most in sales over all?

SELECT Employee.FirstName, Employee.LastName, SUM(Invoice.Total) AS TotalSales, Invoice.InvoiceDate
FROM Invoice
INNER JOIN Customer ON Customer.CustomerId = Invoice.CustomerId
INNER JOIN Employee ON Employee.EmployeeId = Customer.SupportRepId
ORDER BY TotalSales
LIMIT 1

//20. Provide a query that shows the # of customers assigned to each sales agent.

SELECT SUM(CustomerId) AS NumberOfCustomers, Employee.FirstName, Employee.LastName
FROM Customer
INNER JOIN Employee ON Employee.EmployeeId = Customer.SupportRepId
GROUP BY Employee.EmployeeId

//21. Provide a query that shows the total sales per country. Which country's customers spent the most?

SELECT SUM(Invoice.Total) AS TotalSales, Invoice.BillingCountry
FROM Invoice
GROUP BY Invoice.BillingCountry
ORDER BY TotalSales DESC

//22. Provide a query that shows the most purchased track of 2013.

SELECT SUM(InvoiceLine.Quantity) AS NumberOfTracksSold, Track.Name, Invoice.InvoiceDate AS DateSold
FROM InvoiceLine
INNER JOIN Track ON Track.TrackId = InvoiceLine.TrackId
INNER JOIN Invoice ON Invoice.InvoiceId = InvoiceLine.InvoiceId
WHERE Invoice.InvoiceDate LIKE '%2013%'
GROUP BY Track.Name
ORDER BY NumberOfTracksSold DESC

//23. Provide a query that shows the top 5 most purchased tracks over all.

SELECT SUM(InvoiceLine.Quantity) AS NumberOfTracksSold, Track.Name, Invoice.InvoiceDate AS DateSold
FROM InvoiceLine
INNER JOIN Track ON Track.TrackId = InvoiceLine.TrackId
INNER JOIN Invoice ON Invoice.InvoiceId = InvoiceLine.InvoiceId
GROUP BY Track.Name
ORDER BY NumberOfTracksSold DESC
LIMIT 5

//24. Provide a query that shows the top 3 best selling artists.

SELECT SUM(InvoiceLine.Quantity) AS NumberOfTracksSold, Track.Name, Invoice.InvoiceDate AS DateSold, Artist.Name AS ArtistName
FROM InvoiceLine
INNER JOIN Track ON Track.TrackId = InvoiceLine.TrackId
INNER JOIN Invoice ON Invoice.InvoiceId = InvoiceLine.InvoiceId
INNER JOIN Album ON Album.AlbumId = Track.AlbumId
INNER JOIN Artist ON Artist.ArtistId = Album.ArtistId
GROUP BY Track.Name
ORDER BY NumberOfTracksSold DESC
LIMIT 3

//25. Provide a query that shows the most purchased Media Type.

SELECT SUM(Invoice.Total) AS TotalSales, Track.Name, MediaType.Name AS MediaType
FROM InvoiceLine
INNER JOIN Track ON Track.TrackId = InvoiceLine.TrackId
INNER JOIN Invoice ON Invoice.InvoiceId = InvoiceLine.InvoiceId
INNER JOIN MediaType ON MediaType.MediaTypeId = Track.MediaTypeId
GROUP BY Track.Name
ORDER BY TotalSales DESC
LIMIT 1


















