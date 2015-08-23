# SQL: Silvan

## OPG 0.1 Opret et SQL script der kan oprette Silvan databasen.
```sql
CREATE DATABASE [Silvan]
```

## OPG 0.2 Opret et SQL script der kan oprette tabellerne i Silvan databasen.
```sql
USE [Silvan]

CREATE TABLE [Customer](
  CustNo     [int] Primary Key NOT NULL,
  CustName   [nvarchar](30) NOT NULL,
  State      [varchar](30) NOT NULL,
  Phone      [varchar](15) NOT NULL
)

CREATE TABLE [Item](
  ItemNo     [int] PRIMARY KEY NOT NULL,
  ItemName   [nvarchar](50) NOT NULL,
  ItemPrice  [float] NOT NULL,
  QtyOnHand  [int] NOT NULL
)

CREATE TABLE [Invoice](
  InvNo      [int] Primary Key NOT NULL,
  InvDate    [date] NOT NULL,
  CustNo     [int] NOT NULL,

  CONSTRAINT [FK_Invoice_Customer] FOREIGN KEY (CustNo)
    REFERENCES [Customer] (CustNo)
    ON DELETE CASCADE
    ON UPDATE CASCADE
)

CREATE TABLE [InvItem](
  InvNo      [int] NOT NULL,
  ItemNo     [int] NOT NULL,
  Qty        [int] NOT NULL,

  CONSTRAINT [FK_InvItem_Invoice] FOREIGN KEY (InvNo)
    REFERENCES [Invoice] (InvNo)
    ON DELETE CASCADE
    ON UPDATE CASCADE,

  CONSTRAINT FK_InvItem_Item FOREIGN KEY (ItemNo)
    REFERENCES Item (ItemNo)
    ON DELETE CASCADE
    ON UPDATE CASCADE,

  CONSTRAINT [PK_Item] PRIMARY KEY (InvNo, ItemNo)
)
```

## OPG 0.3 Opret et ER- diagram i Visio over Silvan databasen.

## OPG 0.4 Beskriv hvad der sker med Silvan databasen når man følger mapningsreglerne.

## OPG 0.4 Beskriv hvad der sker med Silvan databasen hvis den skal opfylde 3. noalform.

## OPG 0.5 Opret et RDS i Visio over Silvan databasen.

## OPG 0.6 Opret ovenstående database og tabeller i MSSQL med ovenstående test data.
```sql
USE [Silvan]

INSERT INTO dbo.Customer (CustNo, CustName, State, Phone) VALUES (211, 'Garcia', 'NJ', '732-555-1000')
INSERT INTO dbo.Customer (CustNo, CustName, State, Phone) VALUES (212, 'Parikh', 'NY', '212-555-2000')
INSERT INTO dbo.Customer (CustNo, CustName, State, Phone) VALUES (225, 'Elsenhauer', 'NJ', '973-555-3333')
INSERT INTO dbo.Customer (CustNo, CustName, State, Phone) VALUES (239, 'Bayer', 'FL', '407-555-7777')

INSERT INTO dbo.Invoice (InvNo, InvDate, CustNo) VALUES (1001, '09-05-2000', 212)
INSERT INTO dbo.Invoice (InvNo, InvDate, CustNo) VALUES (1002, '09-09-2000', 225)
INSERT INTO dbo.Invoice (InvNo, InvDate, CustNo) VALUES (1003, '09-17-2000', 239)
INSERT INTO dbo.Invoice (InvNo, InvDate, CustNo) VALUES (1004, '09-18-2000', 211)
INSERT INTO dbo.Invoice (InvNo, InvDate, CustNo) VALUES (1005, '09-21-2000', 212)

INSERT INTO dbo.Item (ItemNo, ItemName, ItemPrice, QtyOnHand) VALUES (1, 'screw', 2.25, 50)
INSERT INTO dbo.Item (ItemNo, ItemName, ItemPrice, QtyOnHand) VALUES (2, 'Nut', 5.00, 110)
INSERT INTO dbo.Item (ItemNo, ItemName, ItemPrice, QtyOnHand) VALUES (3, 'Bolt', 3.99, 75)
INSERT INTO dbo.Item (ItemNo, ItemName, ItemPrice, QtyOnHand) VALUES (4, 'Hammer', 9.99, 125)
INSERT INTO dbo.Item (ItemNo, ItemName, ItemPrice, QtyOnHand) VALUES (5, 'Washer', 1.99, 100)
INSERT INTO dbo.Item (ItemNo, ItemName, ItemPrice, QtyOnHand) VALUES (6, 'Nail', 0.99, 300)

INSERT INTO dbo.InvItem (InvNo, ItemNo, Qty) VALUES (1001, 1, 5)
INSERT INTO dbo.InvItem (InvNo, ItemNo, Qty) VALUES (1001, 3, 5)
INSERT INTO dbo.InvItem (InvNo, ItemNo, Qty) VALUES (1001, 5, 9)
INSERT INTO dbo.InvItem (InvNo, ItemNo, Qty) VALUES (1002, 1, 2)
INSERT INTO dbo.InvItem (InvNo, ItemNo, Qty) VALUES (1002, 2, 3)
INSERT INTO dbo.InvItem (InvNo, ItemNo, Qty) VALUES (1003, 1, 7)
INSERT INTO dbo.InvItem (InvNo, ItemNo, Qty) VALUES (1003, 2, 1)
INSERT INTO dbo.InvItem (InvNo, ItemNo, Qty) VALUES (1004, 4, 5)
INSERT INTO dbo.InvItem (InvNo, ItemNo, Qty) VALUES (1005, 4, 10)
```

##  OPG 1 - DISPLAY ALL CUSTOMER INFORMATION
```sql
SELECT * FROM customer
```

##  OPG 2 - DISPLAY ALL ITEM NAMES AND THEIR RESPECTIVE UNIT PRICE
```sql
Select Item.ItemName, Item.ItemPrice FROM Item
```

##  OPG 3 - DISPLAY UNIQUE INVOICE NUMBERS FROM THE INVITEM TABLE
```sql
Select invitem.InvNo FROM invitem Group by InvNo
```

##  OPG 4 - DISPLAY ITEM INFORMATION WITH APPROPRIATE COLUMN ALIASES
```sql
SELECT Item.ItemNo as 'ItemNumber', Item.ItemName as 'Name of item', Item.ItemPrice as 'Unit price' FROM item
```

##  OPG 5 - DISPLAY ITEM NAME AND PRICE IN SENTENCE FORM USING CONCATENATION
```sql
SELECT CONCAT ( Item.ItemName, ' koster ', Item.ItemPrice, ' kr' ) AS Information FROM item
```

##  OPG 6 - FIND TOTAL VALUE OF EACH ITEM BASED ON QUANTITY ON HAND
```sql
SELECT Item.ItemName, Item.QtyOnHand*Item.ItemPrice as 'Total value' From Item
```

##  OPG 7 - FIND CUSTOMERS WHO ARE FROM THE STATE OF FLORIDA
```sql
SELECT * FROM customer where state = 'FL'
```

##  OPG 8 - DISPLAY ITEMS WITH UNIT PRICE OF AT LEAST
```sql
select ItemName,ItemPrice from item where item.ItemPrice >= 5
```

##  OPG 9 - WHICH ITEMS ARE BETWEEN  AND 7
```sql
Select * from item where ItemPrice >= 2 and ItemPrice <= 57
```

##  OPG 10 - WHICH CUSTOMERS ARE FROM THE TRISTATE AREA OF NJ, NY, AND CT
```sql
SELECT * from customer WHERE customer.State IN ('NJ', 'NY', 'CT')
```

##  OPG 11 - FIND ALL CUSTOMERS WHOSE NAMES START WITH THE LETTER E
```sql
SELECT * from customer where customer.CustName like 'E%'
```

##  OPG 12 - FIND ITEMS WITH E W IN THEIR NAME
```sql
SELECT * FROM item where item.ItemName like '%e%' or item.ItemName like '%w%'
```

##  OPG 13 - SORT ALL CUSTOMERS ALPHABETICALLY
```sql
SELECT customer.CustName from customer order by customer.CustName asc
```

##  OPG 14 - SORT ALL ITEMS IN DESCENDING ORDER BY THEIR PRICE
```sql
select item.ItemPrice from item order by item.ItemPrice desc
```

##  OPG 15 - SORT ALL CUSTOMERS BY THEIR STATE AND ALSO ALPHABETICALLY BY NAME
```sql
SELECT customer.State, customer.CustName from customer order by customer.CustName asc
```

##  OPG 16 - DISPLAY ALL THE CUSTOMERS FROM NEW JERSEY ALPHABETICALLY
```sql
SELECT * from customer where customer.State = 'NJ' order by customer.CustName
```

##  OPG 17 - DISPLAY ALL ITEM PRICES ROUNDED TO THE NEAREST DOLLAR
```sql
SELECT round(item.ItemPrice, 0) FROM item
```

##  OPG 18 - THE PAYMENT IS DUE IN TWO MONTHS FROM THE INVOICE DATE. FIND THE PAYMENT DUE DATE
```sql
SELECT InvNo, InvDate, DATEADD(month,2,InvDate) as 'Payment Due' from invoice
```

##  OPG 19 - DISPLAY INVOICE DATES IN 'SEPTEMBER 05, 2000' FORMAT
```sql
SELECT InvDate, FORMAT ( InvDate, 'D', 'en-gb' ) AS 'Great Britain English Result' from invoice
```

##  OPG 20 - FIND THE TOTAL, AVERAGE, HIGHEST, AND LOWEST UNIT PRICE IN ITEM
```sql
SELECT SUM(ItemPrice) AS 'Total', AVG(ItemPrice) AS 'Average', MAX(ItemPrice) AS 'Highest', MIN(ItemPrice) AS 'Lowest' from Item
```

##  OPG 21 - HOW MANY DIFFERENT ITEMS ARE AVAILABLE FOR CUSTOMERST
```sql
SELECT COUNT(*) AS 'Different Items' from item
```

##  OPG 22 - COUNT THE NUMBER OF ITEMS ORDERED IN EACH INVOICE
```sql
SELECT InvNo, COUNT(*) AS 'Items ordered' from invitem group by InvNo
```

##  OPG 23 - FIND INVOICES IN WHICH THREE OR MORE ITEMS ARE ORDERED
```sql
SELECT InvNo, COUNT(*) AS 'Items ordered' from invitem group by InvNo having COUNT(*) >= 3
```

##  OPG 24 - FIND ALL POSSIBLE COMBINATIONS OF CUSTOMERS AND ITEMS (CARTESIAN PRODUCT)
```sql
SELECT * FROM Customer cross join item
```

##  OPG 25 - DISPLAY ALL ITEM QUANTITIES AND ITEM PRICES FOR INVOICES
```sql
select * from invitem inner join item on invitem.ItemNo = item.ItemNo
```

##  OPG 26 - FIND THE TOTAL PRICE AMOUNT FOR EACH INVOICE
```sql
SELECT invitem.InvNo, SUM(invitem.Qty * item.itemPrice) from invitem inner join item on invitem.ItemNo = item.ItemNo group by invitem.InvNo
```

##  OPG 27 - USE AN OUTER JOIN TO DISPLAY ITEMS ORDERED AS WELL AS NOT ORDERED SO FAR
```sql
select item.ItemNo, invitem.InvNo from item
	left outer join invitem on invitem.itemno = item.itemno
	order by invitem.InvNo
```

##  OPG 28 - DISPLAY INVOICES. CUSTOMER NAMES, AND ITEM NAMES TOGETHER (MULTIPLE JOINS) (THE JOIN OF FOUR TABLES MIGHT NOT RETURN ANY ROWS)
```sql
select invitem.InvNo, customer.CustName, item.ItemName, invitem.Qty from invoice
	inner join customer on customer.CustNo = invoice.CustNo
	inner join invitem	on invitem.InvNo   = invoice.InvNo
	inner join item		on item.ItemNo     = invitem.ItemNo
```

##  OPG 29 - FIND INVOICES WITH HAMMER AS AN ITEM
```sql
select invoice.InvNo, item.ItemName, invitem.Qty from item inner join invitem on item.ItemNo = invitem.ItemNo inner join invoice on invitem.InvNo = invoice.InvNo where item.ItemName = 'Hammer'
```

##  OPG 30 - FIND INVOICES WITH HAMMER AS AN ITEM BY USING A SUB-QUERY INSTEAD OF A JOIN
```sql
select item.itemname from item where item.itemno in ( select invitem.itemno from invitem where invitem.InvNo = 1001 )
```

##  OPG 31 - DISPLAY THE NAMES OF ITEMS ORDERED IN INVOICE NUMBER 1001 (USE A SUB-QUERY)
```sql
select Item.ItemName, item.ItemPrice from item where item.ItemPrice < (select item.ItemPrice from item where item.itemname = 'Nut')
```

##  OPG 32 - FIND ITEMS THAT ARE CHEAPER THAN NUT
```sql

```
##  OPG 33 - CREATE A TABLE FOR ALL THE NEW JERSEY CUSTOMERS BASED ON THE EXISTING CUSTOMER TABLE
```sql

```
##  OPG 34 - COPY ALL NEW YORK CUSTOMERS TO THE TABLE WITH NEW JERSEY CUSTOMERS
```sql

```
##  OPG 35 RENAME THE NJ CUSTOMER TABLE TO NYNJ CUSTOMER
```sql

```
##  OPG 36 - FIND CUSTOMERS WHO ARE IN CUSTEMER BUT NOT FROM NY OR NJ (USE SET OPERATOR MINUS)
```sql

```
##  OPG 37 - DELETE ROWS FROM THE CUSTOMER TABLE THAT ARE ALSO IN THE NYNJ_CUSTOMER TABLE
```sql

```
##  OPG 38 - FIND THE ITEMS WITH THE TOP THREE PRICES
```sql

```
##  OPG 39 - FIND TWO ITEMS WITH THE LOWEST QUANTITY ON HAND
```sql

```
##  OPG 40 - CREATE A SIMPLE VIEW WITH ITEM NAMES AND ITEM PRICES ONLY
```sql

```
##  OPG 40 - CREATE A VIEW THAT WILL DISPLAY INVOICE NUMBER AND CUSTOMER NAMES FOR al CUSTOMERS
```sql

```
##  OPG 41 - CREATE AN INDEX FILE TO SPEED UP A SEARCH BASED ON CUSTOMER'S NAME
```sql

```
##  OPG 42 - LOCK CUSTOMER BAYER'S RECORD TO UPDATE THE STATE AND THE PHONE NUMBER
```sql

```
##  OPG 43 - GIVE EVERYBODY SELECT AND INSERT RIGHTS ON YOUR ITEM TABLE
```sql

```
##  OPG 44 - REVOKE THE INSERT OPTION ON THE ITEM TABLE FROM USER EVERYBODY
```sql

```
