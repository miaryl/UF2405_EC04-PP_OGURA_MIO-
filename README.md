# Tienda Django

Aplicación web desarrollada con Django que implementa un sistema completo de gestión de productos y categorías (CRUD), autenticación de usuarios y flujo de recuperación de contraseñas mediante correo electrónico.

---

## Características principales

- **Gestión de catálogo (CRUD):**
  - Categorías: Listado, creación, edición y eliminación.
  - Productos: Listado público, vista de detalle, creación, actualización y borrado.
- **Autenticación y control de acceso:**
  - Registro de nuevos usuarios con validación de correo electrónico único.
  - Inicio y cierre de sesión (`login` / `logout`).
  - Vistas protegidas mediante decorador `@login_required`.
- **Recuperación de contraseña:**
  - Envío de enlace temporal de restablecimiento mediante consola (`console.EmailBackend`).
  - Validación de tokens y generación segura de nuevas credenciales.

---

## Estructura del proyecto

```text
UF2406ED01/
├── manage.py
├── tienda/             # Configuración principal del proyecto
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── productos/          # App de catálogo y autenticación
│   ├── models.py       # Modelos Categoria y Producto
│   ├── forms.py        # Formularios de validación y registro
│   ├── views.py        # Lógica de vistas y control de acceso
│   └── urls.py         # Rutas de productos y categorías
└── templates/          # Plantillas HTML
    ├── base.html
    ├── home.html
    ├── productos/      # Plantillas de CRUD de productos y categorías
    └── registration/   # Plantillas del sistema de autenticación

```

## Requisitos previos

- Python 3.12+

- Entorno virtual (venv)

- Django 6.1.1

## Instalación y ejecución local

Clonar el repositorio:

```Bash
git clone https://github.com/miaryl/UF2405_EC04-PP_OGURA_MIO-.git
)
cd UF2405_EC04-PP_OGURA_MIO-
```

Crear y activar el entorno virtual:


### En Windows:

```Bash
python -m venv .venv
.venv\Scripts\activate
```

### En macOS/Linux:

```Bash
python3 -m venv .venv
source .venv/bin/activate
```

Instalar dependencias:

```Bash
pip install django

```

 o si existe requirements.txt:

` pip install -r requirements.txt`

Aplicar migraciones:

```Bash
python manage.py migrate
(Opcional) Crear un superusuario:
```

```Bash
python manage.py createsuperuser
```

Iniciar el servidor de desarrollo:

```Bash
python manage.py runserver
```

Abrir en el navegador:

Inicio: `http://127.0.0.1:8000/`

Panel de administración: `http://127.0.0.1:8000/admin/`

Pruebas del flujo de recuperación de contraseña
Acceder a `http://127.0.0.1:8000/accounts/password_reset/`

Introducir el correo electrónico de un usuario registrado.

Revisar la terminal donde se ejecuta el servidor: allí se imprimirá el correo con el enlace que contiene uidb64 y el token.

Copiar la URL completa generada y abrirla en el navegador para establecer la nueva contraseña.


---

## Rutas y Endpoints de la Aplicación

### 1. Navegación General y Autenticación de Usuarios

| Método | URL | Nombre de Ruta (`name`) | Descripción | Acceso |
| :--- | :--- | :--- | :--- | :--- |
| `GET` | `/` | `home` | Página principal de bienvenida | Público |
| `GET`, `POST` | `/register/` | `register` | Formulario de registro de nuevos usuarios | Público |
| `GET`, `POST` | `/accounts/login/` | `login` | Inicio de sesión de usuarios | Público |
| `POST` | `/accounts/logout/` | `logout` | Cierre de sesión de la cuenta actual | Autenticado |

---

### 2. Flujo de Recuperación de Contraseña

| Método | URL | Nombre de Ruta (`name`) | Descripción | Acceso |
| :--- | :--- | :--- | :--- | :--- |
| `GET`, `POST` | `/accounts/password_reset/` | `password_reset` | Formulario para solicitar enlace de recuperación | Público |
| `GET` | `/accounts/password_reset/done/` | `password_reset_done` | Confirmación de correo de recuperación enviado | Público |
| `GET`, `POST` | `/accounts/reset/<uidb64>/<token>/` | `password_reset_confirm` | Formulario para ingresar la nueva contraseña | Público (Token válido) |
| `GET` | `/accounts/reset/done/` | `password_reset_complete` | Notificación de cambio de contraseña exitoso | Público |

---

### 3. Gestión de Productos (CRUD)

| Método | URL | Nombre de Ruta (`name`) | Descripción | Acceso |
| :--- | :--- | :--- | :--- | :--- |
| `GET` | `/productos/` | `producto_lista` | Listado completo de productos disponibles | Público |
| `GET` | `/productos/<int:pk>/` | `producto_detalle` | Vista detallada de un producto específico | Público |
| `GET`, `POST` | `/productos/nuevo/` | `producto_crear` | Formulario para registrar un nuevo producto | `@login_required` |
| `GET`, `POST` | `/productos/<int:pk>/editar/` | `producto_editar` | Edición de los datos de un producto existente | `@login_required` |
| `POST` | `/productos/<int:pk>/eliminar/` | `producto_eliminar` | Eliminación de un producto del catálogo | `@login_required` |

---

### 4. Gestión de Categorías (CRUD)

| Método | URL | Nombre de Ruta (`name`) | Descripción | Acceso |
| :--- | :--- | :--- | :--- | :--- |
| `GET` | `/categorias/` | `categoria_lista` | Listado de todas las categorías | `@login_required` |
| `GET`, `POST` | `/categorias/nueva/` | `categoria_crear` | Formulario para crear una nueva categoría | `@login_required` |
| `GET`, `POST` | `/categorias/<int:pk>/editar/` | `categoria_editar` | Modificación de nombre y descripción | `@login_required` |
| `POST` | `/categorias/<int:pk>/eliminar/` | `categoria_eliminar` | Eliminación de una categoría | `@login_required` |

---

### 5. Administración

| Método | URL | Nombre de Ruta (`name`) | Descripción | Acceso |
| :--- | :--- | :--- | :--- | :--- |
| `GET`, `POST` | `/admin/` | `admin:index` | Panel oficial de administración de Django | `is_staff` / Superusuario |
