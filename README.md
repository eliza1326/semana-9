# semana-9
Uso de inventario en un local de venta de quimicos, actualizaciones, creacion de clientes, creacion de productos y eliminacion
import json

class Inventario:
    def __init__(self):
        self.productos = {"Materias Primas": [], "Productos Terminados": [], "Perfumes Réplica AAA": []}

    def añadir_producto(self, categoria, nombre, cantidad, precio):
        producto = {"nombre": nombre, "cantidad": cantidad, "precio": precio}
        self.productos[categoria].append(producto)
        print(f"Producto {nombre} añadido a {categoria}.")

    def actualizar_producto(self, categoria, nombre, cantidad, precio):
        for producto in self.productos[categoria]:
            if producto["nombre"] == nombre:
                producto["cantidad"] = cantidad
                producto["precio"] = precio
                print(f"Producto {nombre} actualizado.")
                return
        print(f"Producto {nombre} no encontrado en {categoria}.")

    def consultar_inventario(self, categoria=None):
        if categoria:
            print(f"Inventario de {categoria}:")
            for producto in self.productos[categoria]:
                print(f"{producto['nombre']} - Cantidad: {producto['cantidad']} - Precio: ${producto['precio']}")
        else:
            for cat in self.productos:
                print(f"Inventario de {cat}:")
                for producto in self.productos[cat]:
                    print(f"{producto['nombre']} - Cantidad: {producto['cantidad']} - Precio: ${producto['precio']}")
    
    def eliminar_producto(self, categoria, nombre):
        for producto in self.productos[categoria]:
            if producto["nombre"] == nombre:
                self.productos[categoria].remove(producto)
                print(f"Producto {nombre} eliminado de {categoria}.")
                return
        print(f"Producto {nombre} no encontrado en {categoria}.")

    def guardar_datos(self, archivo):
        with open(archivo, 'w') as f:
            json.dump(self.productos, f)
        print(f"Datos guardados en {archivo}.")

    def cargar_datos(self, archivo):
        try:
            with open(archivo, 'r') as f:
                self.productos = json.load(f)
            print(f"Datos cargados desde {archivo}.")
        except FileNotFoundError:
            print(f"No se encontró el archivo {archivo}. Comenzando con inventario vacío.")

def menu():
    inventario = Inventario()
    inventario.cargar_datos('inventario.json')

    while True:
        print("\n--- Menú de Inventario ---")
        print("1. Añadir producto")
        print("2. Actualizar producto")
        print("3. Consultar inventario")
        print("4. Eliminar producto")
        print("5. Guardar datos")
        print("6. Salir")
        
        opcion = input("Seleccione una opción: ")

        if opcion == "1":
            categoria = input("Ingrese la categoría (Materias Primas, Productos Terminados, Perfumes Réplica AAA): ")
            nombre = input("Ingrese el nombre del producto: ")
            cantidad = int(input("Ingrese la cantidad: "))
            precio = float(input("Ingrese el precio: "))
            inventario.añadir_producto(categoria, nombre, cantidad, precio)
        
        elif opcion == "2":
            categoria = input("Ingrese la categoría (Materias Primas, Productos Terminados, Perfumes Réplica AAA): ")
            nombre = input("Ingrese el nombre del producto: ")
            cantidad = int(input("Ingrese la nueva cantidad: "))
            precio = float(input("Ingrese el nuevo precio: "))
            inventario.actualizar_producto(categoria, nombre, cantidad, precio)
        
        elif opcion == "3":
            categoria = input("Ingrese la categoría (Materias Primas, Productos Terminados, Perfumes Réplica AAA) o 'Todas' para ver todo: ")
            if categoria.lower() == "todas":
                inventario.consultar_inventario()
            else:
                inventario.consultar_inventario(categoria)
        
        elif opcion == "4":
            categoria = input("Ingrese la categoría (Materias Primas, Productos Terminados, Perfumes Réplica AAA): ")
            nombre = input("Ingrese el nombre del producto: ")
            inventario.eliminar_producto(categoria, nombre)
        
        elif opcion == "5":
            inventario.guardar_datos('inventario.json')
        
        elif opcion == "6":
            inventario.guardar_datos('inventario.json')
            print("Saliendo del programa.")
            break

        else:
            print("Opción no válida, por favor intente de nuevo.")

if __name__ == "__main__":
    menu()
