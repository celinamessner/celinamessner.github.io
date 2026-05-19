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

**Introduction**
The design of robots interacting with humans in order to provide services requires a coherent execution of components ranging from speech recognition, to visual analysis to geographical coordination. Managing software and hardware components, scanners and sensors makes not only the development difficult but also requires foresight into scalability, which emphasizes the importance of structure planning (Kim et al., 2016).
The following reports outlines the design of a tea making humanoid robot, using unified modeling language design frameworks. Initially, the class diagram is outlined, followed by the state transition diagram. This report then describes the sequence diagram and finally concludes with the activity diagram.

**Class diagram**
The tea-making robot system has been designed to create an efficient, safe, and user-friendly automated beverage service solution. The design focuses on two main operations: order processing and tea preparation, implemented through an object-oriented architecture.
The system's core is built around a queue-based order management system, first-in-first-out (FIFO), as demonstrated in both the activity and class diagrams. The FIFO approach was selected due to proven resolution of queries with the fewest failures (Chien et al., 2012).
Research, however, has proven the FIFO approach only to be useful in non-collaborative robot processes. As soon as multiple robots would conduct collaborative execution, alternate strategies would be selected, making this design not fit for scaling (Humbert et al., 2015)
For a single robot execution, the queue implementation also allows for better resource management, as shown in the separation of the order_queue class from the main Robot class. 
<img width="1480" height="1141" alt="oop1-image3" src="https://github.com/user-attachments/assets/30511c4d-b6de-4604-b07c-f6d1ad319ba7" />
The class structure employs a separation of concerns, dividing functionality into distinct components. The Robot class serves as the central controller, handling user interactions and orchestrating the overall tea-making process. The Tea_Maker class encapsulates the physical tea preparation operations, while specialized components (water_boiler and tea_dispenser) manage specific hardware operations. The Robot, Tea_Maker, as well as the tea_dispenser class handle object manipulation, including picking and placing objects. The water_boiler class also requires environmental detection, to measure water temperature.

**State transition diagram**

<img width="769" height="1820" alt="oop1-image1" src="https://github.com/user-attachments/assets/571c64e7-ca17-41af-9c98-93cbdb277ebb" />
 
State management is a crucial aspect of the design, as illustrated in the state transition diagram. The system implements comprehensive status checking before any physical operation, preventing failure of an order such as dispensing without the tea specific’s temperature or attempting to serve when resources are unavailable. Recent studies, however, suggest that task level control leads to concurrent execution, as the one portrayed in the state transition diagram, and in turn can lead to high latency or inefficient execution (Toris, 2014). As an alternative, Task Definition Language, an extension of C++, is suggested for simple maintenance and easy error resolution (Reid & Apfelbaum, 1998).

**Sequence diagram**
 
<img width="2012" height="2048" alt="oop1-image2" src="https://github.com/user-attachments/assets/6120d88a-e84e-4cbd-b442-bd44a8cf4525" />

The sequence diagram reveals the system's ability to handle parallel operations efficiently. Water boiling and tea bag preparation can occur simultaneously, optimizing preparation time while maintaining safety through synchronized completion points.

**Activity diagram**

<img width="1740" height="2040" alt="oop1-image4" src="https://github.com/user-attachments/assets/b4938096-aa6e-4492-b032-3525fdbf5aff" />
  
The activity diagram demonstrates loops in the logic, to account for resource unavailability or temperature levels not reached. This makes sure, the system is ready, should there be an error in the order, and in the listening state.
The tea_dispenser and water_boiler components include specific attributes (temperature, quantity, tea_type) that enable precise control and monitoring of the tea-making process. These components implement functions like check_availability() and check_temperature() to ensure safe and quality operation, as shown in the class diagram.
The system maintains continuous monitoring of critical parameters like water temperature and tea availability, with clear status communication paths to both the main control system and the user interface.
The queue manages orders, while internal lists track available tea types and their properties. This structured approach to data management ensures consistent operation and enables system diagnostics and performance monitoring.
The overall design provides a foundation for implementation while maintaining flexibility for future enhancements. The robust state management system ensures safe and reliable operation in various conditions. 

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

Chien, S.-Y. et al. (2012) “Scheduling operator attention for multi-robot control,” in 2012 IEEE/RSJ International Conference on Intelligent Robots and Systems. IEEE, pp. 473–479.

Humbert, G. et al. (2015) “Comparative analysis of pick & place strategies for a multi-robot application,” in 2015 IEEE 20th conference on emerging technologies & factory automation (ETFA). IEEE, pp. 1–8.

Kim, Minseong et al. (2006) “UML-based service robot software development: a case study,” in Proceedings of the 28th international conference on software engineering, pp. 534–543.

Sharkawy, A.-N. (2021) Human-Robot Interaction: Applications.

Simmons, R. and Apfelbaum, D. (1998) “A task description language for robot control,” in Proceedings. 1998 IEEE/RSJ International Conference on Intelligent Robots and Systems. Innovations in Theory, Practice and Applications (Cat. No. 98CH36190). IEEE, pp. 1931–1937.

Toris, R., Kent, D. and Chernova, S. (2014) “The robot management system: A framework for conducting human-robot interaction studies through crowdsourcing,” Journal of Human-Robot Interaction, 3(2), pp. 25–49.

## Resources

👉 [View the tea_robot.py code on GitHub](https://github.com/celinamessner/tea_robot.py)
