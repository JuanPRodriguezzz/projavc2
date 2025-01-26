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
## Script para crear entorno virtual
``` 
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

from datetime import date

class Product:
    """
    Representa un producto en el inventario.

    Attributes:
        id (int): Identificador único del producto.
        name (str): Nombre del producto.
        price (float): Precio unitario del producto.
        quantity (int): Cantidad disponible en inventario.
        category (str): Categoría a la que pertenece el producto.
        entry_date (date): Fecha en que ingresa el producto al inventario.
        exit_date (date, optional): Fecha en que sale el producto del inventario.
    """

    def __init__(
        self,
        id: int,
        name: str,
        price: float,
        quantity: int,
        category: str,
        entry_date: date,
        exit_date: date = None
    ):
        self.id = id
        self.name = name
        self.price = price
        self.quantity = quantity
        self.category = category
        self.entry_date = entry_date
        self.exit_date = exit_date

    def register_entry(self, quantity: int) -> None:
        """
        Aumenta la cantidad del producto en el inventario.
        """
        try:
            if not isinstance(quantity, int):
                raise TypeError("Quantity must be an integer.")
            if quantity <= 0:
                raise ValueError("Quantity must be greater than 0.")
            
            self.quantity += quantity
            print(f"{quantity} units of {self.name} have been ADDED.")
            print(f"Current stock: {self.quantity} units.\n")
        
        except (ValueError, TypeError) as e:
            print(f"Error in register_entry: {e}\n")

    def register_exit(self, quantity: int) -> None:
        """
        Disminuye la cantidad del producto en el inventario (salida).
        """
        try:
            if not isinstance(quantity, int):
                raise TypeError("Quantity must be an integer.")
            if quantity <= 0:
                raise ValueError("Quantity must be greater than 0.")
            if quantity > self.quantity:
                raise ValueError("Not enough units in stock.")
            
            self.quantity -= quantity
            print(f"{quantity} units of {self.name} have been REMOVED.")
            print(f"Current stock: {self.quantity} units.\n")
        
        except (ValueError, TypeError) as e:
            print(f"Error in register_exit: {e}\n")

    def __str__(self) -> str:
        exit_date_str = self.exit_date if self.exit_date else "N/A"
        return (
            f"ID: {self.id}, Name: {self.name}, Price: ${self.price:.2f}, "
            f"Quantity: {self.quantity}, Category: {self.category}, "
            f"Entry Date: {self.entry_date}, Exit Date: {exit_date_str}"
        )

docstrings
Se usan anotaciones de tipo (List[Product], Optional[Product]).

from typing import List, Optional
from .product import Product

class Inventory:
    """
    Maneja el listado de productos y operaciones sobre ellos.
    """

    def __init__(self):
        # Puedes optar por un diccionario: { product.id: product } para acceso más rápido.
        # Aquí usaremos una lista por simplicidad.
        self.products: List[Product] = []

    def add_product(self, product: Product) -> None:
        """
        Agrega un producto al inventario si no existe uno con el mismo ID.
        """
        if any(p.id == product.id for p in self.products):
            print(f"Error: A product with ID {product.id} already exists.")
            return
        
        self.products.append(product)
        print(f"Product {product.name} (ID: {product.id}) added to inventory.\n")

    def remove_product(self, product_id: int) -> None:
        """
        Elimina un producto del inventario por su ID.
        """
        product = self.search_product(product_id)
        if product:
            self.products.remove(product)
            print(f"Product {product.name} (ID: {product_id}) removed from inventory.\n")
        else:
            print(f"Error: Product with ID {product_id} not found.\n")

    def list_inventory(self) -> None:
        """
        Muestra todos los productos en el inventario.
        """
        print("=== Current Inventory ===")
        if not self.products:
            print("The inventory is empty.\n")
            return

        for product in self.products:
            print(product)
        print()

    def search_product(self, product_id: int) -> Optional[Product]:
        """
        Busca y retorna un producto por su ID. Retorna None si no existe.
        """
        for product in self.products:
            if product.id == product_id:
                return product
        return None

    def update_quantity(self, product_id: int, new_quantity: int) -> None:
        """
        Actualiza la cantidad de un producto existente.
        """
        try:
            product = self.search_product(product_id)
            if product is None:
                raise ValueError(f"Product with ID {product_id} not found.")

            if not isinstance(new_quantity, int):
                raise TypeError("Quantity must be an integer.")

            if new_quantity < 0:
                raise ValueError("Quantity cannot be negative.")
            
            product.quantity = new_quantity
            print(f"Successfully updated quantity of {product.name} to {product.quantity}.\n")

        except (ValueError, TypeError) as e:
            print(f"Error in update_quantity: {e}\n")





import threading
from .inventory import Inventory

class Report:
    """
    Maneja la generación de reportes del inventario.
    """

    def __init__(self, inventory: Inventory):
        self.inventory = inventory

    def generate_current_report(self) -> None:
        """
        Genera un reporte del inventario actual en un hilo separado.
        """
        print("Generating Current Inventory Report in background...\n")
        thread = threading.Thread(target=self._generate_report_logic)
        thread.start()

    def _generate_report_logic(self):
        """
        Método interno para generar el reporte sin bloquear el hilo principal.
        """
        # Aquí podrías añadir más lógica, como generar un PDF o un CSV.
        # Por ahora, solo listaremos el inventario.
        self.inventory.list_inventory()

Se encapsula la lógica de generación en _generate_report_logic para mantener limpio el método público.

from datetime import date
from .inventory import Inventory
from .product import Product
from .report import Report

def main():
    # Crear el inventario
    inventory = Inventory()

    # Crear algunos productos
    product1 = Product(
        id=1,
        name="Laptop",
        price=1500.00,
        quantity=10,
        category="Electronics",
        entry_date=date.today()
    )
    product2 = Product(
        id=2,
        name="Smartphone",
        price=800.00,
        quantity=5,
        category="Electronics",
        entry_date=date.today()
    )

    # Agregar productos al inventario
    inventory.add_product(product1)
    inventory.add_product(product2)
    # Intentar agregar un producto con un ID duplicado
    inventory.add_product(product1)  

    # Mostrar inventario
    inventory.list_inventory()

    # Actualizar la cantidad de un producto
    inventory.update_quantity(1, 15)
    inventory.update_quantity(3, 10)  # Producto inexistente

    # Registrar una entrada y salida en product1
    product1.register_entry(5)
    product1.register_exit(3)

    # Generar un reporte en segundo plano
    report = Report(inventory)
    report.generate_current_report()

if __name__ == "__main__":
    main()

Qué hace este main.py:
Crea el inventario y dos productos.
Agrega los productos, con manejo de duplicados.
Lista el inventario.
Actualiza la cantidad de un producto válido y uno inexistente.
Muestra el registro de entrada/salida en un producto.
Genera un reporte en segundo plano.




En tu repositorio, también puedes incluir capturas de pantalla, GIFs demostrando la ejecución, ejemplos de outputs, etc.

---

# Conclusión

Con esta estructura y mejoras:

- **Separas responsabilidades** en módulos (`product.py`, `inventory.py`, `report.py`, `main.py`).
- **Documentas** cada clase y método con **docstrings**.
- **Manejas errores** tanto de valor como de tipo en cada método.
- **Proporcionas** instrucciones claras en el `README.md`.
- **Propones** un diagrama de clases para explicar la arquitectura.

De esta forma, cumples con los requisitos de:
- Código original y organizado en **clases**.
- Manejo por consola (puedes mejorarlo con menús interactivos).
- Registros de entrada/salida, listado y búsqueda.
- Uso de fechas en los productos.
- Manejo de hilos para la generación de reportes.

Si quisieras añadir **carga masiva de registros** o **persistencia en archivos**, podrías crear métodos adicionales en `Inventory` para leer/escribir datos en formato CSV, JSON, o bases de datos. Igualmente, podrías crear un **módulo** separado llamado `persistence.py` que maneje esa lógica.

¡Listo! Con esto ya tienes una **base sólida** para tu sistema de gestión de inventario y un repositorio listo para compartir con tu equipo.


classDiagram
    class Product {
        +int id
        +str name
        +float price
        +int quantity
        +str category
        +date entry_date
        +date exit_date
        +register_entry(int)
        +register_exit(int)
    }

    class Inventory {
        -List~Product~ products
        +add_product(Product)
        +remove_product(int)
        +list_inventory()
        +search_product(int) Product
        +update_quantity(int, int)
    }

    class Report {
        -Inventory inventory
        +generate_current_report()
        +_generate_report_logic()
    }
    
    Inventory --> Product : "contiene lista de"
    Report --> Inventory : "genera reportes de"

