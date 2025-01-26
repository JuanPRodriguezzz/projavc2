# projavc2
```python
from datetime import date
import threading


class Product:
    def __init__(self, id: int, name: str, price: float, quantity: int, category: str,
                 entry_date: date, exit_date: date = None):
        self.id = id
        self.name = name
        self.price = price
        self.quantity = quantity
        self.category = category
        self.entry_date = entry_date
        self.exit_date = exit_date

    def register_entry(self, quantity: int):
        try:
            if quantity <= 0:
                raise ValueError("Quantity must be greater than 0.")
            self.quantity += quantity
            print(f"{quantity} units of {self.name} have been added. Total in inventory: {self.quantity}.")
        except ValueError as e:
            print(f"Error: {e}")

    def register_exit(self, quantity: int):
        try:
            if quantity <= 0:
                raise ValueError("Quantity must be greater than 0.")
            if quantity > self.quantity:
                raise ValueError("Not enough units in stock.")
            self.quantity -= quantity
            print(f"{quantity} units of {self.name} have been removed. Total in inventory: {self.quantity}.")
        except ValueError as e:
            print(f"Error: {e}")

    def __str__(self):
        return (f"ID: {self.id}, Name: {self.name}, Price: ${self.price:.2f}, "
                f"Quantity: {self.quantity}, Category: {self.category}, "
                f"Entry Date: {self.entry_date}, Exit Date: {self.exit_date or 'N/A'}")


class Inventory:
    def __init__(self):
        self.products = []

    def add_product(self, product: Product):
        self.products.append(product)
        print(f"Product {product.name} added to inventory.")

    def remove_product(self, id: int):
        product = self.search_product(id)
        if product:
            self.products.remove(product)
            print(f"Product {product.name} removed from inventory.")
        else:
            print(f"Product with ID {id} not found.")

    def list_inventory(self):
        print("\nCurrent Inventory:")
        if not self.products:
            print("The inventory is empty.")
        for product in self.products:
            print(product)

    def search_product(self, id: int):
        for product in self.products:
            if product.id == id:
                return product
        print(f"Product with ID {id} not found.")
        return None


class Report:
    def __init__(self, inventory: Inventory):
        self.inventory = inventory

    def generate_current_report(self):
        print("Generating Current Inventory Report...")
        threading.Thread(target=self.inventory.list_inventory).start()


# Example usage
if __name__ == "__main__":
    inventory = Inventory()

    product1 = Product(id=1, name="Laptop", price=1500.00, quantity=10, category="Electronics",
                       entry_date=date.today())
    product2 = Product(id=2, name="Smartphone", price=800.00, quantity=5, category="Electronics",
                       entry_date=date.today())

    inventory.add_product(product1)
    inventory.add_product(product2)

    report = Report(inventory)
    report.generate_current_report()
```

``` Script para crear entorno virtual
#!/bin/bash

# Crear entorno virtual
python3 -m venv venv

# Activar entorno virtual
source venv/bin/activate

# Instalar dependencias
pip install -r requirements.txt

echo "Entorno virtual configurado con éxito."}
```

# Guía del repositorio:

```
# Inventory Management System

## Descripción
Este proyecto implementa un sistema de gestión de inventarios utilizando programación orientada a objetos en Python.

## Características
- Gestión de productos: añadir, eliminar, registrar entradas y salidas.
- Reportes de inventario en tiempo real.
- Manejo básico de excepciones.

## Instalación
1. Clona este repositorio:
git clone <url-del-repositorio> cd project

arduino
Copy
Edit

2. Configura el entorno virtual:
bash setup_environment.sh


3. Ejecuta el programa:
python3 src/inventory_management.py


## Colaboradores
- [Tu Nombre](https://github.com/tuusuario)
- [Miembro 2](https://github.com/miembro2)
- [Miembro 3](https://github.com/miembro3)

## Diagrama de Clases
El diagrama de clases está disponible en `class_diagram.md`.
```
