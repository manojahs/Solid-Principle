# Solid-Principle
```
--------------------------------
1. Single Responsibility Principle (SRP)
2. Open/Closed Principle (OCP)
3. Liskov Substitution Principle (LSP)
4. Interface Segregation Principle (ISP)
5. Dependency Inversion Principle (DIP)

---------------------------------------------
1. Single Responsibility Principle (SRP)
--------------------------------------------
A class should have only one reason to change.
it Makes the code more maintainable and easier to understand.

// ❌ Bad Example: The class is handling both employee data and salary calculation
---------------------
public class Employee
{
    public string Name { get; set; }
    public double Salary { get; set; }

    public void CalculateSalary() { /* Calculation Logic */ }
    public void SaveToDatabase() { /* Database Logic */ }
}

// ✅ Good Example: Separate concerns into different classes
--------------------------
public class Employee
{
    public string Name { get; set; }
    public double Salary { get; set; }
}

public class SalaryCalculator
{
    public double CalculateSalary(Employee emp) { /* Calculation Logic */ return emp.Salary; }
}

public class EmployeeRepository
{
    public void SaveToDatabase(Employee emp) { /* Database Logic */ }
}
🔹 Benefit: Each class has a single responsibility, making it easier to test, modify, and reuse.


2. Open/Closed Principle (OCP)
---------------------------------------
 Open for extension, closed for modification.
 Adding new functionality should not require modifying existing code.

 // ❌ Bad Example: Every time we add a new payment , we need to modify this class.
 ----------------
class PaymentProcessor
{
    public void Process(string paymentType)
    {
        if (paymentType == "CreditCard")
            Console.WriteLine("Processing credit card payment...");
        else if (paymentType == "UPI")
            Console.WriteLine("Processing UPI payment...");
        // Tomorrow → Need PayPal? Must MODIFY this class ❌
    }
}


// ✅ Good Example: Using polymorphism (Open for extension, closed for modification)
------------------


// Step 1: Define abstraction
interface IPayment
{
    void Pay();
}

// Step 2: Implement different payment methods
class CreditCardPayment : IPayment
{
    public void Pay() => Console.WriteLine("Paid with Credit Card");
}

class UPIPayment : IPayment
{
    public void Pay() => Console.WriteLine("Paid with UPI");
}

// Step 3: Payment processor depends on abstraction
class PaymentProcessor
{
    public void Process(IPayment payment)
    {
        payment.Pay();
    }
}

// Usage
class Program
{
    static void Main()
    {
        var processor = new PaymentProcessor();
        processor.Process(new CreditCardPayment()); // Paid with Credit Card
        processor.Process(new UPIPayment());       // Paid with UPI
    }
}


3. Liskov Substitution Principle (LSP)
-------------------------------------
A subclass should be substitutable for its base class without breaking functionality.
It Prevents unexpected behaviors when using inheritance.
A better approach is to avoid inheritance and use interfaces instead.
 If a class B is a subclass of A, then objects of A can be replaced with objects of B without breaking the application.

Wrong Approach
-------------------------------
public class Bird
{
    public virtual void Fly()
    {
        Console.WriteLine("Bird is flying");
    }
}

public class Penguin : Bird
{
    public override void Fly()
    {
        throw new NotImplementedException("Penguins cannot fly!");
    }
}

Better Aproach
-----------------
public interface IFlyable
{
    void Fly();
}

public class Sparrow : IFlyable
{
    public void Fly()
    {
        Console.WriteLine("Sparrow is flying");
    }
}

public class Penguin
{
    public void Swim()
    {
        Console.WriteLine("Penguin is swimming");
    }
}

4. Interface Segregation Principle (ISP)
----------------------------------------------
A class should not be forced to implement interfaces it does not use.
// ❌ Bad Example: Printer class is forced to implement Scan(), even if it doesn’t support scanning
------------------------------------
public interface IMachine
{
    void Print();
    void Scan();
}

public class Printer : IMachine
{
    public void Print() { /* Print Logic */ }
    public void Scan() { throw new NotImplementedException(); } // Not applicable
}

// ✅ Good Example: Separate interfaces for specific behaviors
---------------------------
public interface IPrinter
{
    void Print();
}

public interface IScanner
{
    void Scan();
}

public class Printer : IPrinter
{
    public void Print() { /* Print Logic */ }
}

public class MultiFunctionPrinter : IPrinter, IScanner
{
    public void Print() { /* Print Logic */ }
    public void Scan() { /* Scan Logic */ }
}

5. Dependency Inversion Principle (DIP)
----------------------------------------
High-level modules should not depend on low-level modules. Both should depend on abstractions.

in the below example Notification class is using Email service which is dependent so that should not happen that why we need to use interface and call it seperately

// ❌ Bad Example: High-level class depends on a low-level class directly
-------------------------
public class EmailService
{
    public void SendEmail(string message) { /* Send Email */ }
}

public class Notification
{
    private EmailService _emailService = new EmailService();
    public void Notify(string message) { _emailService.SendEmail(message); }
}

// ✅ Good Example: Using an interface (abstraction)
-------------------------------------
public interface IMessageService
{
    void SendMessage(string message);
}

public class EmailService : IMessageService
{
    public void SendMessage(string message) { /* Send Email */ }
}

public class Notification
{
    private readonly IMessageService _messageService;

    public Notification(IMessageService messageService)
    {
        _messageService = messageService;
    }

    public void Notify(string message)
    {
        _messageService.SendMessage(message);
    }
}

```

