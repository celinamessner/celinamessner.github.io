---
layout: post
title: "Polymorphism"
subtitle: "Demonstrating polymorphism by defining multiple classes with shared method names"
date: 2025-04-17
author: "Celina Messner"
categories: [python, polymorphism, oop]
tags: [oop, python, polymorphism]
---

### ✨ Polymorphism

This Python script demonstrates polymorphism by defining two classes — `Tea` and `Coffee` — that share the same method names (`info()` and `serving_recommendation()`). Because both objects expose the same interface, they can be iterated through and called identically, even though each class implements its own behaviour.

**Concepts demonstrated:** polymorphism via duck typing, shared interfaces across unrelated classes, iteration over heterogeneous objects.

#### Code

```python
class Tea:
    def __init__(self, bev_type, temperature, steeping_time):
        self.type = bev_type
        self.temperature = temperature
        self.steeping_time = steeping_time

    def info(self):
        print(f"This is the brewing information for {self.type}. {self.type} is best brewed at {self.temperature}. The ideal steeping time is {self.steeping_time} minutes.")

    def serving_recommendation(self):
        print("Tea is best served with honey as sweetener.")

class Coffee:
    def __init__(self, bev_type, temperature, steeping_time):
        self.type = bev_type
        self.temperature = temperature
        self.steeping_time = steeping_time

    def info(self):
        print(f"This is the brewing information for {self.type}. {self.type} is best brewed at {self.temperature}. The ideal steeping time is {self.steeping_time} minutes.")

    def serving_recommendation(self):
        print("Coffee is best served with sugar and milk.")

Tea = Tea("Green tea", 70, 2)
Coffee = Coffee("French press coffee", 90, 3)

for beverage in (Tea, Coffee):
    beverage.info()
    beverage.serving_recommendation()
```

## Resources

👉 [View the polymorphism.py code on GitHub](https://github.com/celinamessner/polymorhism)
