1.

using System;

class Program
{
    static void Main()
    {
        int score = 85;

        if (score >= 90)
        {
            Console.WriteLine("Excellent");
        }
        else if (score >= 70)
        {
            Console.WriteLine("Good");
        }
        else
        {
            Console.WriteLine("Needs Improvement");
        }
    }
}

2.

using System;

class BankAccount
{
    private double balance;
    public string OwnerName { get; set; }

    public BankAccount(string ownerName, double initialBalance)
    {
        OwnerName = ownerName;
        balance = initialBalance;
    }

    public void Deposit(double amount)
    {
        balance += amount;
    }

    public void Withdraw(double amount)
    {
        if (amount <= balance)
        {
            balance -= amount;
        }
        else
        {
            Console.WriteLine("Not enough balance!");
        }
    }

    public double GetBalance()
    {
        return balance;
    }
}

class Program
{
    static void Main()
    {
        BankAccount acc = new BankAccount("Giorgi", 1000);

        acc.Deposit(500);
        acc.Withdraw(300);

        Console.WriteLine("Final Balance: " + acc.GetBalance());
    }
}


3.

using System;

class Employee
{
    protected double salary;

    public Employee(double salary)
    {
        this.salary = salary;
    }

    public virtual double CalculateBonus()
    {
        return salary * 0.1;
    }
}

class Manager : Employee
{
    public Manager(double salary) : base(salary) { }

    public override double CalculateBonus()
    {
        return salary * 0.2;
    }
}

class Developer : Employee
{
    public Developer(double salary) : base(salary) { }

    public override double CalculateBonus()
    {
        return salary * 0.15;
    }
}

class Program
{
    static void Main()
    {
        Manager m = new Manager(5000);
        Developer d = new Developer(4000);

        Console.WriteLine("Manager Bonus: " + m.CalculateBonus());
        Console.WriteLine("Developer Bonus: " + d.CalculateBonus());
    }
}

4.

using System;

interface ILogger
{
    void Log(string message);
}

class ConsoleLogger : ILogger
{
    public void Log(string message)
    {
        Console.WriteLine(message);
    }
}

class FileLogger : ILogger
{
    public void Log(string message)
    {
        Console.WriteLine("Writing to file: " + message);
    }
}

class Program
{
    static void Main()
    {
        ILogger logger1 = new ConsoleLogger();
        ILogger logger2 = new FileLogger();

        logger1.Log("Hello Console");
        logger2.Log("Hello File");
    }
}

5.

using System;
using System.Collections.Generic;

class MyList<T> where T : IComparable<T>
{
    private List<T> items = new List<T>();

    public void Add(T item)
    {
        items.Add(item);
    }

    public T Max()
    {
        if (items.Count == 0)
            throw new InvalidOperationException("List is empty");

        T max = items[0];

        foreach (var item in items)
        {
            if (item.CompareTo(max) > 0)
            {
                max = item;
            }
        }

        return max;
    }
}

5.2

using System;

class Student : IComparable<Student>
{
    public string Name { get; set; }
    public int Score { get; set; }

    public Student(string name, int score)
    {
        Name = name;
        Score = score;
    }

    public int CompareTo(Student other)
    {
        return this.Score.CompareTo(other.Score);
    }

    public override string ToString()
    {
        return Name + " - " + Score;
    }
}

class Program
{
    static void Main()
    {
        MyList<Student> students = new MyList<Student>();

        students.Add(new Student("Ana", 85));
        students.Add(new Student("Giorgi", 92));
        students.Add(new Student("Nino", 78));

        Student topStudent = students.Max();

        Console.WriteLine("Top Student: " + topStudent);
    }
}
