---
layout: post
title: "Protected Variables"
subtitle: "Exploring protected variables in an employee record system"
date: 2025-04-17
author: "Celina Messner"
categories: [python, oop]
tags: [oop, python, encapsulation, protected]
---

### 🔒 Protected Variables

This Python script demonstrates the use of protected variables in an employee management system. It uses `_leave_balance` (the leading underscore signals a protected attribute by convention) to restrict direct access and promote encapsulation, while still allowing controlled interaction through class methods like `book_leave()` and `display_details()`.

**Concepts demonstrated:** encapsulation, protected attributes (`_` convention), input validation, controlled state mutation.

#### Code

```python
class Employee:
    def __init__(self, name, location, total_leave_days):
        # unprotected variables
        self.name = name

        # protected variable
        self.location = location
        self._leave_balance = total_leave_days

    def display_details(self):
        print(f"Employee Name: {self.name}")
        print(f"location: {self.location}")
        print(f"Leave Days Remaining: {self._leave_balance}")

    def book_leave(self, days_requested):
        if days_requested <= 0:
            print("Error. Please enter a positive number of days.")
            return

        if days_requested <= self._leave_balance:
            self._leave_balance -= days_requested
            print(f"Leave approved for {days_requested} days.")
            print(f"Updated leave balance: {self._leave_balance}")
        else:
            print("Error. Not enough leave available.")

# Example usage
def main():
    # Create an employee instance
    employee1 = Employee("Anna Mayer", "Berlin", 20)

    # Display employee details
    employee1.display_details()

    # Book 5 days of leave
    print("\nRequesting 5 days of leave...")
    employee1.book_leave(5)

    # Attempt to book more leave than available
    print("\nRequesting 18 days of leave...")
    employee1.book_leave(18)

    # Accessing a protected variable directly
    print(f"\nLeave balance: {employee1._leave_balance} day(s)")

if __name__ == "__main__":
    main()
```

## Resources

👉 [View the protected_variable.py code on GitHub](https://github.com/celinamessner/protected_variables)
