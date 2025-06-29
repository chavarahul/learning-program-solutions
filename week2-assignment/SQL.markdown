# SQL Queries and Stored Procedures Guide

## SQL Queries

### Discount for Senior Customers
Reduce loan interest rate by 1% for customers over 60:
```sql
UPDATE Customers
SET LoanInterestRate = LoanInterestRate - 1
WHERE Age > 60;
```
![alt text](<WhatsApp Image 2025-06-29 at 22.57.50_bf87ea0c.jpg>)

### Promote to VIP Based on Balance
Set `IsVIP` to `TRUE` for customers with balance over 10,000:
```sql
UPDATE Customers
SET IsVIP = TRUE
WHERE Balance > 10000;
```
![alt text](<WhatsApp Image 2025-06-29 at 22.58.20_2c5671a6.jpg>)

### Loan Due Reminders (Next 30 Days)
Generate reminders for loans due in the next 30 days:
```sql
SELECT 
  CONCAT('Reminder: Loan ', LoanID, ' for Customer ', CustomerID,
         ' is due on ', DATE_FORMAT(DueDate, '%d-%b-%Y')) AS Reminder
FROM Loans
WHERE DueDate BETWEEN CURDATE() AND CURDATE() + INTERVAL 30 DAY;
```

![alt text](<WhatsApp Image 2025-06-29 at 22.58.54_d50baae0.jpg>)

## Stored Procedures

### ProcessMonthlyInterest
Add 1% interest to savings accounts:
```sql
DELIMITER $$

CREATE PROCEDURE ProcessMonthlyInterest()
BEGIN
  UPDATE Accounts
  SET Balance = Balance + (Balance * 0.01)
  WHERE AccountType = 'SAVINGS';
END$$

DELIMITER ;

CALL ProcessMonthlyInterest();
```



### UpdateEmployeeBonus
Increase salary based on bonus percentage for a department:
```sql
DELIMITER $$

CREATE PROCEDURE UpdateEmployeeBonus(
  IN dept_id INT,
  IN bonus_pct DECIMAL(5,2)
)
BEGIN
  UPDATE Employees
  SET Salary = Salary + (Salary * (bonus_pct / 100))
  WHERE DepartmentID = dept_id;
END$$

DELIMITER ;

CALL UpdateEmployeeBonus(101, 10);
```

![alt text](<WhatsApp Image 2025-06-29 at 23.02.42_22d3d4ed.jpg>)

### TransferFunds
Transfer funds between accounts with balance validation:
```sql
DELIMITER $$

CREATE PROCEDURE TransferFunds(
  IN from_acc INT,
  IN to_acc INT,
  IN amount DECIMAL(10,2)
)
BEGIN
  DECLARE from_balance DECIMAL(10,2);

  SELECT Balance INTO from_balance
  FROM Accounts
  WHERE AccountID = from_acc;

  IF from_balance >= amount THEN
    UPDATE Accounts
    SET Balance = Balance - amount
    WHERE AccountID = from_acc;

    UPDATE Accounts
    SET Balance = Balance + amount
    WHERE AccountID = to_acc;
  ELSE
    SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT = 'Insufficient balance';
  END IF;
END$$

DELIMITER ;

CALL TransferFunds(201, 203, 300);
```

![alt text](<WhatsApp Image 2025-06-29 at 23.03.21_33a01d07.jpg>)