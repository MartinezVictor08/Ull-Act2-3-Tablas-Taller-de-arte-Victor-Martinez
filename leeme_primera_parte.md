# 🎨 Proyecto: Taller de Arte

**Lenguaje:** Python  
**Framework:** Django  
**Editor:** Visual Studio Code  

---

## 🧩 Primera Parte

### 🗂️ 1. Crear carpeta del proyecto
Nombre de la carpeta:
UIII_TallerdeArte_0272

markdown
Copiar código

### 💻 2. Abrir VS Code sobre la carpeta
1. Abre Visual Studio Code.  
2. Selecciona **Archivo → Abrir carpeta**.  
3. Elige `UIII_TallerdeArte_0272`.

### 🖥️ 3. Abrir terminal en VS Code
- Usa el atajo:  
  **Ctrl + ñ** o **Ver → Terminal**.

### 🐍 4. Crear entorno virtual `.venv`
En la terminal:
```bash
python -m venv .venv
🔑 5. Activar el entorno virtual
bash
Copiar código
.venv\Scripts\activate
⚙️ 6. Activar intérprete de Python
En VS Code, presiona Ctrl + Shift + P

Escribe: Python: Select Interpreter

Selecciona el entorno .venv.

📦 7. Instalar Django
bash
Copiar código
pip install django
🏗️ 8. Crear el proyecto sin duplicar carpeta
bash
Copiar código
django-admin startproject backend_TallerdeArte .
🚀 9. Ejecutar el servidor en el puerto 8272
bash
Copiar código
python manage.py runserver 8272
🌐 10. Copiar el enlace en el navegador
cpp
Copiar código
http://127.0.0.1:8272/
🎨 11. Crear aplicación
bash
Copiar código
python manage.py startapp app_TallerdeArte
🧱 12. Archivo models.py
python
Copiar código
from django.db import models

# ==========================================
# MODELO: Instructor
# ==========================================
class Instructor(models.Model):
    instructor_id = models.AutoField(primary_key=True)
    nombre = models.CharField(max_length=100)
    apellido = models.CharField(max_length=100)
    especialidad = models.CharField(max_length=100)
    telefono = models.CharField(max_length=20)
    correo = models.EmailField()
    experiencia_anios = models.PositiveIntegerField()
    sueldo = models.DecimalField(max_digits=8, decimal_places=2)

    def __str__(self):
        return f"{self.nombre} {self.apellido}"


# ==========================================
# MODELO: Material
# ==========================================
class Material(models.Model):
    material_id = models.AutoField(primary_key=True)
    nombre_material = models.CharField(max_length=100)
    tipo = models.CharField(max_length=50)
    marca = models.CharField(max_length=50)
    cantidad_stock = models.PositiveIntegerField()
    costo_unitario = models.DecimalField(max_digits=8, decimal_places=2)
    nombre_proveedor = models.CharField(max_length=100)
    descripcion = models.TextField()
    curso = models.ForeignKey('Curso', on_delete=models.CASCADE, related_name='materiales')

    def __str__(self):
        return self.nombre_material


# ==========================================
# MODELO: Curso
# ==========================================
class Curso(models.Model):
    curso_id = models.AutoField(primary_key=True)
    nombre_curso = models.CharField(max_length=100)
    descripcion = models.TextField()
    salon_num = models.IntegerField()
    duracion_semanas = models.PositiveIntegerField()
    costo = models.DecimalField(max_digits=8, decimal_places=2)
    fecha_inicio = models.DateField()
    cupo_maximo = models.PositiveIntegerField()
    instructor = models.ForeignKey(Instructor, on_delete=models.CASCADE, related_name='cursos')

    def __str__(self):
        return self.nombre_curso
🧩 12.5 Realizar migraciones
bash
Copiar código
python manage.py makemigrations
python manage.py migrate
👨‍🏫 13. Trabajar primero con el modelo Instructor
📄 14. En views.py crear las funciones:
inicio_TallerdeArte

agregar_instructor

actualizar_instructor

realizar_actualizacion_instructor

borrar_instructor

🧱 15–22. Estructura de carpetas y archivos HTML
css
Copiar código
app_TallerdeArte/
│
├── templates/
│   ├── base.html
│   ├── header.html
│   ├── navbar.html
│   ├── footer.html
│   ├── inicio.html
│   └── instructor/
│       ├── agregar_instructor.html
│       ├── ver_instructor.html
│       ├── actualizar_instructor.html
│       └── borrar_instructor.html
🎨 17–20. Contenido HTML
base.html → incluye Bootstrap (CSS + JS)

navbar.html → menú principal:

Sistema de Administración Taller de Arte

Inicio

Instructor → submenu: agregar, ver, actualizar, borrar

Curso → submenu

Material → submenu

(Usar íconos solo en las opciones principales)

footer.html → incluir:

css
Copiar código
© Creado por Victor Martinez, CBTIS 128 — [fecha del sistema]
(Fijo al final de la página)

inicio.html → información del sistema + imagen tomada de la red sobre taller de arte.

🌐 24. Archivo urls.py en app_TallerdeArte
Agregar las rutas correspondientes a las funciones CRUD del modelo Instructor.

⚙️ 25–26. Configuraciones
Registrar la app en settings.py

Enlazar urls.py del proyecto con el de la app.

🗃️ 27. Registrar modelos en admin.py
bash
Copiar código
python manage.py makemigrations
python manage.py migrate
⚠️ Por ahora, trabajar solo con el modelo Instructor.
Los modelos Curso y Material se dejan pendientes.

💅 28–31. Estilo y finalización
Usar colores suaves y modernos.

No validar entrada de datos.

Crear toda la estructura antes de comenzar.

Proyecto debe ser totalmente funcional.

Ejecutar servidor final:

bash
Copiar código
python manage.py runserver 8272
