---
layout: post
title: "Binary Search Tree — Record Book of Interests"
subtitle: "Python implementation of a record book using a binary search tree"
date: 2025-01-13
author: "Celina Messner"
categories: [python, data-structures, algorithms]
tags: [python, data-structures, binary-search-tree, algorithms]
---

### 🌳 Record Book of Interests (BST)

The interest record book application is designed in Python to manage, store, and retrieve interests along with a respective resource. For the data structure of this application, a **binary search tree (BST)** was selected. The record book consists of an *interest* (as the key) and a *resource* (as the associated value). The tree consists of nodes of interests with a maximum of two children. The tree starts empty and the first entry acts as the root node (W3 Schools, 2025a).

Operations within scope of the application are adding interests (with a resource), searching for interests, deleting entries, displaying all interests with their resources, and a menu interface that guides the user through the functionality.

### Why a BST instead of sequential search?

Originally, sequential search was selected as the data structure for the application. Due to feedback that sequential search limits scalability and adds unnecessary overhead, binary search trees were chosen for development of the application. This allows for easier alphabetical sorting and faster searching of records (Canning, 2023).

Instead of checking every record one by one through sequential search, the binary search tree applies a *divide and conquer* approach. Each comparison eliminates half of the remaining tree, which is much faster than scanning the entire list sequentially (Glenn & Klassen, 2012).

### Application functionality

The record book supports the following operations:

- **Add** an interest and resource (menu option 1)
- **Delete** an interest after confirmation (menu option 2)
- **Search** for an interest and show its resource (menu option 3)
- **Display** all records in alphabetical order (menu option 4)
- **Exit** the program (menu option 5)

### Code Walkthrough

#### Node class

The `Node` class creates *interest* as the key and *resource* as the associated value. Because we chose a BST, each node has a `left` child (for interests alphabetically smaller) and a `right` child (for interests alphabetically larger).

```python
class Node:
    def __init__(self, interest, resource):
        self.interest = interest    # key
        self.resource = resource    # associated value
        self.left = None            # alphabetically smaller
        self.right = None           # alphabetically larger
```

#### BinarySearchTree class — insert

`insert()` adds a new record at the appropriate location based on alphabetical order. If the interest already exists, the resource is updated rather than duplicated.

```python
class BinarySearchTree:
    def __init__(self):
        self.root = None

    def insert(self, interest, resource):
        if self.root is None:
            self.root = Node(interest, resource)
            return

        current = self.root
        while True:
            if interest < current.interest:
                if current.left is None:
                    current.left = Node(interest, resource)
                    break
                current = current.left
            elif interest > current.interest:
                if current.right is None:
                    current.right = Node(interest, resource)
                    break
                current = current.right
            else:
                # Interest already exists — update the resource
                current.resource = resource
                break
```

#### Search

`search()` traverses the BST from the root, going left or right based on alphabetical comparison.

```python
    def search(self, interest):
        current = self.root
        while current is not None:
            if interest == current.interest:
                return current
            elif interest < current.interest:
                current = current.left
            else:
                current = current.right
        return None
```

#### Delete

Deletion removes a node while preserving the BST structure. If the node has two children, the *in-order successor* (smallest value in the right subtree) replaces it.

```python
    def delete(self, interest):
        self.root = self._delete_node(self.root, interest)

    def _delete_node(self, node, interest):
        if node is None:
            return node
        if interest < node.interest:
            node.left = self._delete_node(node.left, interest)
        elif interest > node.interest:
            node.right = self._delete_node(node.right, interest)
        else:
            # Node with one or no child
            if node.left is None:
                return node.right
            elif node.right is None:
                return node.left
            # Node with two children: get the in-order successor
            temp = self._min_value_node(node.right)
            node.interest = temp.interest
            node.resource = temp.resource
            node.right = self._delete_node(node.right, temp.interest)
        return node

    def _min_value_node(self, node):
        current = node
        while current.left is not None:
            current = current.left
        return current
```

#### Display (in-order traversal)

Because we want output in alphabetical order, **in-order traversal** is used: visit left subtree, then current node, then right subtree. For a BST, this yields values in ascending order (W3 Schools, 2025b).

```python
    def display(self):
        self._inorder_traversal(self.root)

    def _inorder_traversal(self, node):
        if node is not None:
            self._inorder_traversal(node.left)
            print(f"Interest: {node.interest}, Resource: {node.resource}")
            self._inorder_traversal(node.right)
```

#### Menu-driven interface

The menu runs in a loop, presenting the five options. A series of `if`/`elif`/`else` branches dispatches the user's choice; only option `5` breaks the loop.

```python
tree = BinarySearchTree()

def add_record():
    interest = input("Enter the interest: ")
    resource = input("Enter the resource: ")
    tree.insert(interest, resource)
    print("Record added successfully!")
    display_records()

def delete_record():
    interest = input("Enter interest you want to delete: ")
    result = tree.search(interest)
    if result is None:
        print("Deletion failed.")
    else:
        confirm = input(f"Are you sure you want to delete the record for '{interest}'? (yes/no): ")
        if confirm.lower() == 'yes':
            tree.delete(interest)
            print(f"Record for '{interest}' deleted.")
        else:
            print("Deletion cancelled.")
    display_records()

def search_record():
    if tree.root is None:
        print("No Interests in the Record.")
    else:
        interest = input("Enter the interest you want to search for: ")
        result = tree.search(interest)
        if result is None:
            print(f"'{interest}' does not exist.")
        else:
            print(f"Found record: Interest - {result.interest}, Resource - {result.resource}")

def display_records():
    print("\nCurrent Records:")
    tree.display()

def show_menu():
    while True:
        print("\nMenu:")
        print("1. Add a record")
        print("2. Delete a record")
        print("3. Search for a record")
        print("4. Display all records")
        print("5. Exit")
        choice = input("What would you like to do? ")
        if choice == '1':
            add_record()
        elif choice == '2':
            delete_record()
        elif choice == '3':
            search_record()
        elif choice == '4':
            display_records()
        elif choice == '5':
            print("Exiting the program.")
            break
        else:
            print("Invalid choice! Please enter a number between 1 and 5.")

show_menu()
```

### Testing

**Adding records** — inserted "AI", "Loneliness", "Sycophancy", "Bias", "Accessibility" in that order and verified the display showed them alphabetically.

**Deletion** — deleted an existing record ("AI") and confirmed it disappeared; attempted to delete a non-existent record ("Cooking") and verified that the system printed "Deletion failed".

**Search** — searched an empty tree (got "No Interests in the Record"), searched for "Bias" (got the resource), and searched for "Housework" (got "'Housework' does not exist").

All cases produced the expected output.

### References

Brookshear, J.G., Smith, D. and Brylow, D. (2012) *Computer Science: An Overview.*

Canning, J., Broder, A.J. and Lafore, R.W. (2023) *Data Structures & Algorithms in Python.* Addison-Wesley Professional.

W3 Schools (2025a) [DSA Binary Trees](https://www.w3schools.com/dsa/dsa_data_binarytrees.php).

W3 Schools (2025b) [DSA In-order Traversal](https://www.w3schools.com/dsa/dsa_algo_binarytrees_inorder.php).
