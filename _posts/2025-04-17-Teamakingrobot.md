---
layout: post
title: "System Design — Tea Making Robot"
subtitle: "Object-oriented system implementation for a humanoid tea-making robot"
date: 2025-04-14
author: "Celina Messner"
categories: [python, uml, oop, system-design]
tags: [oop, python, uml, system-design, robotics]
---

### 🤖 System Implementation

The tea making robot application is a Python-based project designed to simulate the process of taking tea orders, preparing, and dispensing tea. The application uses Object-Oriented Programming (OOP) principles to represent various components such as the water boiler, tea dispenser, and order queue. In OOP, classes allow a clear modular set-up, which structures the program with clear, modular, and reusable code components that interact to perform the desired task of making tea (Klump, 2001). The classes encapsulate the logic of different components (e.g., the water boiler, tea dispenser, order queue) into distinct objects.

### Application functionality

The tea making robot supports the following operations:

**Taking orders**

- The robot receives tea orders.
- The robot checks if the tea is available and adds it to the order queue.

**Processing orders**

- The robot heats the water to the ideal temperature based on the type of tea.
- It dispenses the tea and updates the stock after dispensing.

**Exiting the program**

- The user can exit the program by typing `exit`.

### Code Walkthrough

#### Tea Stock & Temperatures

Tea stock and temperatures are stored as dictionaries, which get updated as the system runs. The available quantity of each type of tea is recorded in `tea_stock`. The ideal temperature for each type of tea is defined in `tea_temperatures`.

```python
# Ideal temperatures for each type of tea
tea_temperatures = {
    "Mint": 90,
    "Green": 80,
    "Black": 95,
    "Chamomile": 93
}

# Stock for each type of tea
tea_stock = {
    "Mint": 5,
    "Green": 2,
    "Black": 8,
    "Chamomile": 0
}
```

#### WaterBoiler class

The `WaterBoiler` class is responsible for heating water to the desired temperature for each tea type. It uses the `heat_to()` method to set the water to the correct temperature, based on the tea order.

```python
class WaterBoiler:
    def __init__(self):
        self.temperature = 25       # starting temp

    def heat_to(self, target_temp):
        self.temperature = target_temp
        print(f"Heating water to {self.temperature}°...")
```

#### TeaDispenser class

The `TeaDispenser` class is responsible for checking whether the desired tea type is in stock and dispensing it. The `check_availability()` method checks whether there are any servings left in stock for the requested tea type, while the `dispense()` method updates the stock after dispensing a tea.

```python
class TeaDispenser:
    def __init__(self):
        self.available = tea_stock        # start with initial stock

    def check_availability(self, tea_type):
        return self.available.get(tea_type, 0) > 0

    def dispense(self, tea_type):
        if self.check_availability(tea_type):
            print(f"Dispensing {tea_type} tea...")
            self.available[tea_type] -= 1
            print(f"Stock left: {self.available[tea_type]}")
        else:
            print(f"{tea_type} tea is not available!")
```

#### OrderQueue class

The `OrderQueue` class manages tea orders in FIFO (First In, First Out) order. It adds a new tea order to the queue and retrieves and removes the first order from the queue to be processed. If the queue is empty, it returns `None`. It also reports whether there are any orders pending.

```python
class OrderQueue:
    def __init__(self):
        self.queue = []

    def add_order(self, order):
        print(f"New order received: {order}")
        self.queue.append(order)

    def get_next_order(self):
        if self.queue:
            return self.queue.pop(0)        # FIFO
        return None

    def has_orders(self):
        return len(self.queue) > 0
```

#### Tea_Maker class

The `Tea_Maker` class combines the water boiler and the tea dispenser. The `prepare_tea()` method first checks if the requested tea is in stock. If it is, the robot heats the water to the correct temperature and dispenses the tea.

```python
class Tea_Maker:
    def __init__(self, boiler, dispenser):
        self.boiler = boiler
        self.dispenser = dispenser

    def prepare_tea(self, tea_type):
        print(f"Preparing {tea_type} tea...")
        if not self.dispenser.check_availability(tea_type):
            print("Error: Tea not available!")
            return False

        desired_temp = tea_temperatures[tea_type]
        print(f"Heating water to the right temperature for {tea_type}...")
        self.boiler.heat_to(desired_temp)

        self.dispenser.dispense(tea_type)
        print(f"Tea prepared successfully at {desired_temp}°C!")
        return True
```

#### Robot class

The `Robot` class is the controller for the tea-making process. It manages the order queue, takes orders, and processes them by interacting with the `Tea_Maker`, `TeaDispenser`, and `WaterBoiler`.

```python
class Robot:
    def __init__(self):
        self.queue = OrderQueue()
        self.boiler = WaterBoiler()
        self.dispenser = TeaDispenser()
        self.tea_maker = Tea_Maker(self.boiler, self.dispenser)

    def take_order(self, tea_type):
        if tea_type in tea_temperatures:
            self.queue.add_order(tea_type)
        else:
            print("Sorry, we don't have that tea.")

    def process_orders(self):
        while self.queue.has_orders():
            next_order = self.queue.get_next_order()
            print(f"\nProcessing order: {next_order}")
            success = self.tea_maker.prepare_tea(next_order)
            if not success:
                print("Order failed.\n")
            else:
                print("Order complete.\n")
            print("Ready for the next order!")
```

#### Main loop for program interaction

The `main()` function prompts the user to enter the type of tea they want to order (e.g. *Green*, *Black*). Once the order is added, the system informs the user that the order has been successfully placed, and processes any pending orders in the queue.

```python
def main():
    robot = Robot()
    print("Welcome to the Tea Making Robot!")
    print("Type in your tea order (e.g. 'Green', 'Black').")
    print("Type 'exit' to stop ordering.")

    while True:
        tea_order = input("Enter your tea order: ").strip()
        if tea_order.lower() == "exit":
            print("Exiting the Tea Making robot.")
            break
        else:
            robot.take_order(tea_order)
        robot.process_orders()

main()
```

### Testing

The following test cases were executed in order:

1. Order black tea — boil water to the right temperature.
2. Order black tea again — validate that the stock decreased.
3. Order chamomile tea — expect an "out of stock" error, because stock is 0.

The expected outcome is that all orders should execute correctly, or the flow should break gracefully where there is no stock.

#### Results

All orders were successfully executed, and the order that had no stock was handled with the expected error path.

### Conclusion

This tea making robot is a simplified demonstration of task execution for a humanoid robot taking tea orders, managing stock, and orchestrating objects to prepare tea.

### References

Klump, R., 2001. *Understanding object-oriented programming concepts.* In 2001 IEEE Power Engineering Society Summer Meeting Conference Proceedings (Vol. 2, pp. 1070–1074).

## Resources

👉 [View the tea_robot.py code on GitHub](https://github.com/celinamessner/tea_robot.py)
