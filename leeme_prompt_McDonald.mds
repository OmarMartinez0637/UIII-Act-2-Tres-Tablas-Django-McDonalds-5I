Aquí te dejo una estructura detallada y paso a paso para completar el proyecto de McDonald's usando Django:

---

### 1. **Procedimiento para crear la carpeta del proyecto:**

* Abre tu terminal (si usas VS Code, abre el terminal integrado).
* Navega al directorio donde quieres crear el proyecto.
* Ejecuta:

  ```bash
  mkdir UIII_McDonalds_0637
  cd UIII_McDonalds_0637
  ```

---

### 2. **Procedimiento para abrir VS Code sobre la carpeta UIII_McDonalds_0637:**

* Si ya tienes VS Code instalado, puedes abrir la carpeta con:

  ```bash
  code .
  ```
* Esto abrirá la carpeta en VS Code.

---

### 3. **Procedimiento para abrir terminal en VS Code:**

* En VS Code, ve al menú `Terminal` > `New Terminal`.

---

### 4. **Procedimiento para crear el entorno virtual ".venv" desde la terminal de VS Code:**

* Ejecuta el siguiente comando para crear un entorno virtual:

  ```bash
  python -m venv .venv
  ```
* Esto crea una carpeta oculta `.venv` en tu proyecto donde se guardarán las dependencias.

---

### 5. **Procedimiento para activar el entorno virtual:**

* En la terminal de VS Code, ejecuta el siguiente comando:

  * En **Windows**:

    ```bash
    .venv\Scripts\activate
    ```
  * En **Linux/macOS**:

    ```bash
    source .venv/bin/activate
    ```
* Esto activará el entorno virtual y cambiará el prompt de la terminal.

---

### 6. **Procedimiento para activar el intérprete de Python:**

* Asegúrate de que VS Code esté usando el intérprete de Python del entorno virtual.
* Haz clic en la barra de estado de Python en la parte inferior de VS Code y selecciona el intérprete de Python en `.venv`.

---

### 7. **Procedimiento para instalar Django:**

* Con el entorno virtual activado, instala Django:

  ```bash
  pip install django
  ```

---

### 8. **Procedimiento para crear el proyecto `backend_McDonalds` sin duplicar carpeta:**

* Ejecuta el siguiente comando para crear el proyecto Django:

  ```bash
  django-admin startproject backend_McDonalds .
  ```
* Nota que el punto `.` al final evita la creación de una subcarpeta.

---

### 9. **Procedimiento para ejecutar el servidor en el puerto 8036:**

* Para ejecutar el servidor en el puerto 8036, ejecuta:

  ```bash
  python manage.py runserver 8036
  ```
* Accede al servidor en el navegador usando: `http://127.0.0.1:8036/`.

---

### 10. **Procedimiento para copiar y pegar el link en el navegador:**

* Abre tu navegador y pega el link `http://127.0.0.1:8036/` para ver la página de inicio de Django.

---

### 11. **Procedimiento para crear la aplicación `app_McDonalds`:**

* Ejecuta este comando para crear una nueva app dentro del proyecto:

  ```bash
  python manage.py startapp app_McDonalds
  ```

---

### 12. **Modelo `models.py`:**

Ya tienes los modelos definidos en tu código, solo asegúrate de que el archivo `models.py` en `app_McDonalds` contenga lo siguiente:

```python
from django.db import models

# MODELO: CLIENTE
class Cliente(models.Model):
    id_cliente = models.AutoField(primary_key=True)
    nombre = models.CharField(max_length=50)
    apellido = models.CharField(max_length=50)
    email = models.EmailField(unique=True)
    telefono = models.CharField(max_length=15)
    direccion = models.TextField()
    fecha_registro = models.DateField(auto_now_add=True)
    fecha_nacimiento = models.DateField()
    
    def __str__(self):
        return f"{self.nombre} {self.apellido}"

# MODELO: PEDIDO
class Pedido(models.Model):
    id_pedido = models.AutoField(primary_key=True)
    id_cliente = models.ForeignKey(Cliente, on_delete=models.CASCADE, related_name="pedidos")
    id_empleado = models.IntegerField()  # ID de empleado, relacionado con la tabla Empleados
    fecha_pedido = models.DateField(auto_now_add=True)
    total = models.DecimalField(max_digits=10, decimal_places=2)
    estado = models.CharField(max_length=30, choices=[('Pendiente', 'Pendiente'), ('Enviado', 'Enviado'), ('Entregado', 'Entregado')])
    direccion = models.TextField()
    forma_pago = models.CharField(max_length=20, choices=[('Efectivo', 'Efectivo'), ('Tarjeta', 'Tarjeta')])
    
    def __str__(self):
        return f"Pedido #{self.id_pedido} - {self.id_cliente.nombre} {self.id_cliente.apellido}"

# MODELO: EMPLEADO
class Empleado(models.Model):
    id_empleado = models.AutoField(primary_key=True)
    nombre = models.CharField(max_length=50)
    apellido = models.CharField(max_length=50)
    puesto = models.CharField(max_length=50)
    fecha_ingreso = models.DateField(auto_now_add=True)
    salario = models.DecimalField(max_digits=10, decimal_places=2)
    telefono = models.CharField(max_length=15)
    horario = models.CharField(max_length=20)  # Horario de trabajo (Ejemplo: Mañana, Tarde)
    
    def __str__(self):
        return f"{self.nombre} {self.apellido} - {self.puesto}"
```

---

### 13. **Procedimiento para realizar las migraciones:**

* Ejecuta el siguiente comando para crear las migraciones:

  ```bash
  python manage.py makemigrations
  ```
* Luego, aplica las migraciones a la base de datos con:

  ```bash
  python manage.py migrate
  ```

---

### 14. **Procedimiento para trabajar con los modelos:**

Puedes trabajar con el modelo `Cliente` en el panel administrativo de Django o desde la shell de Django.

---

### 15. **Procedimiento para crear las funciones en `views.py`:**

Abre el archivo `views.py` en `app_McDonalds` y agrega las funciones que mencionas:

```python
from django.shortcuts import render, get_object_or_404, redirect
from .models import Cliente
from .forms import ClienteForm

def inicio_mcdonalds(request):
    return render(request, 'inicio.html')

def agregar_cliente(request):
    if request.method == "POST":
        nombre = request.POST['nombre']
        apellido = request.POST['apellido']
        email = request.POST['email']
        telefono = request.POST['telefono']
        direccion = request.POST['direccion']
        fecha_nacimiento = request.POST['fecha_nacimiento']
        Cliente.objects.create(
            nombre=nombre,
            apellido=apellido,
            email=email,
            telefono=telefono,
            direccion=direccion,
            fecha_nacimiento=fecha_nacimiento,
        )
        return redirect('inicio_mcdonalds')
    return render(request, 'agregar_cliente.html')

def actualizar_cliente(request, id_cliente):
    cliente = get_object_or_404(Cliente, id_cliente=id_cliente)
    if request.method == "POST":
        cliente.nombre = request.POST['nombre']
        cliente.apellido = request.POST['apellido']
        cliente.email = request.POST['email']
        cliente.telefono = request.POST['telefono']
        cliente.direccion = request.POST['direccion']
        cliente.fecha_nacimiento = request.POST['fecha_nacimiento']
        cliente.save()
        return redirect('inicio_mcdonalds')
    return render(request, 'actualizar_cliente.html', {'cliente': cliente})

def borrar_cliente(request, id_cliente):
    cliente = get_object_or_404(Cliente, id_cliente=id_cliente)
    cliente.delete()
    return redirect('inicio_mcdonalds')
```

---

### 16. **Crear la carpeta `templates` y los archivos HTML:**

Dentro de `app_McDonalds`, crea las siguientes carpetas y archivos:

* `templates/`

  * `base.html`
  * `header.html`
  * `navbar.html`
  * `footer.html`
  * `inicio.html`
  * `clientes/`

    * `agregar_cliente.html`
    * `ver_clientes.html`
    * `actualizar_cliente.html`
    * `borrar_cliente.html`

### 17. **Configura `urls.py` de `app_McDonalds`:**

Crea el archivo `urls.py` dentro de `app_McDonalds` y agrega las rutas para las vistas:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio_mcdonalds, name='inicio_mcdonalds'),
    path('agregar_cliente/', views.agregar_cliente, name='agregar_cliente'),
    path('actualizar_cliente/<int:id_cliente>/', views.actualizar_cliente, name='actualizar_cliente'),
    path('borrar_cliente/<int:id_cliente>/', views.borrar_cliente, name='borrar_cliente'),
]
```

---

### 18. **Configura `urls.py` del proyecto:**

En `backend_McDonalds/urls.py`, enlaza con las urls de `app_McDonalds`:

```python
```


from django.contrib import admin
from django.urls import path, include

urlpatterns = [
path('admin/', admin.site.urls),
path('', include('app_McDonalds.urls')),  # Enlaza las urls de la app
]

````

---

### 19. **Registrar modelos en `admin.py`:**
   En `app_McDonalds/admin.py`, registra los modelos para el panel de administración de Django:

```python
from django.contrib import admin
from .models import Cliente, Pedido, Empleado

admin.site.register(Cliente)
admin.site.register(Pedido)
admin.site.register(Empleado)
````

---

### 20. **Procedimiento para ejecutar el servidor en el puerto 8036:**

* Ejecuta:

  ```bash
  python manage.py runserver 8036
  ```

---

¡Listo! Ahora tienes tu proyecto McDonald's funcional y organizado. ¡Espero que te sea útil!
