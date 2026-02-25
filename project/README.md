Izaiah Morales
User:imorales28303@gmail.com
tampa/FL

# term-project
Python Inventory Management System Final Project
import json

class Product:
    def __init__(self, name, price, quantity):
        self.name = name
        self.price = price
        self.quantity = quantity

    def to_dict(self):
        return {
            "name": self.name,
            "price": self.price,
            "quantity": self.quantity
        }

class Inventory:
    def __init__(self):
        self.products = []

    def add_product(self, product):
        self.products.append(product)

    def display_products(self):
        if not self.products:
            print("Inventory is empty.")
        else:
            for product in self.products:
                print(f"Name: {product.name}, Price: {product.price}, Quantity: {product.quantity}")

# --------- FUNCTIONS (REQUIRED) ---------

def load_inventory(inventory):
    try:
        with open("inventory.json", "r") as file:
            data = json.load(file)
            for item in data:
                product = Product(item["name"], item["price"], item["quantity"])
                inventory.add_product(product)
    except FileNotFoundError:
        print("No previous inventory file found. Starting fresh.")

def save_inventory(inventory):
    with open("inventory.json", "w") as file:
        json.dump([product.to_dict() for product in inventory.products], file)

def menu(inventory):
    while True:
        print("\nInventory Menu")
        print("1. Add Product")
        print("2. View Products")
        print("3. Save and Exit")

        choice = input("Choose an option: ")

        if choice == "1":
            try:
                name = input("Enter product name: ")
                price = float(input("Enter price: "))
                quantity = int(input("Enter quantity: "))
                product = Product(name, price, quantity)
                inventory.add_product(product)
                print("Product added successfully.")
            except ValueError:
                print("Invalid input. Please enter correct data types.")

        elif choice == "2":
            inventory.display_products()

        elif choice == "3":
            save_inventory(inventory)
            print("Inventory saved. Goodbye!")
            break

        else:
            print("Invalid choice. Please try again.")

def main():
    inventory = Inventory()
    load_inventory(inventory)
    menu(inventory)

if __name__ == "__main__":
    main()
