---
layout: post
title: "Nested Dictionary"
subtitle: "Showcasing how nested dictionaries can be used within an object-oriented program"
date: 2025-04-17
author: "Celina Messner"
categories: [python, nested_dictionary, oop]
tags: [oop, python, nested_dictionary]
---

### 🚗 Nested Dictionary with Car Class

This Python script showcases how nested dictionaries can be used within an object-oriented program. A `Car` class is created to store and manage information about different cars using a nested dictionary structure. The program demonstrates how to interact with the dictionary using built-in methods like `.items()`, `.keys()`, and `.values()` to efficiently access and display car data.

**Concepts demonstrated:** nested dictionaries, encapsulating data inside a class, exposing dict views (`items`, `keys`, `values`) through instance methods.

#### Code

```python
class Car:
    def __init__(self):
        self.car_data = {
            "Audi": {"model": "A1", "year": 2022},
            "BMW": {"model": "X5", "year": 2024},
            "Mecedes": {"model": "A-Class", "year": 2016}
        }

    def show_items(self):
        return self.car_data.items()

    def show_keys(self):
        return self.car_data.keys()

    def show_values(self):
        return self.car_data.values()

car = Car()
print("Items:", list(car.show_items()))
print("Keys:", list(car.show_keys()))
print("Values:", list(car.show_values()))
```

## Resources

👉 [View the nested_dictionary.py code on GitHub](https://github.com/celinamessner/nested_dictionary)
