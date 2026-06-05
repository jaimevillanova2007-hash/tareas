Esta prueba es bastante grande, pero si la divides en módulos se vuelve mucho más manejable.
🗺️ Mapa General de la Aplicación
Plain text
src/
│
├── main.js
│
├── router/
│   └── router.js
│
├── services/
│   ├── authService.js
│   ├── reservationService.js
│   └── userService.js
│
├── guards/
│   ├── authGuard.js
│   └── roleGuard.js
│
├── views/
│   ├── Login.js
│   ├── Dashboard.js
│   ├── Reservations.js
│   ├── AdminReservations.js
│   ├── CreateReservation.js
│   └── NotFound.js
│
├── components/
│   ├── Navbar.js
│   ├── ReservationCard.js
│   └── Modal.js
│
├── utils/
│   ├── storage.js
│   └── helpers.js
│
└── assets/
1. Autenticación
¿Para qué sirve?
Permite que el usuario inicie sesión y valida si existe en la API.
Archivo
Plain text
services/authService.js
Funciones
JavaScript
export async function login(email, password) {}

export function logout() {}

export function getCurrentUser() {}
2. Persistencia de Sesión
¿Para qué sirve?
Guardar el usuario logueado para que no se cierre la sesión al refrescar.
Archivo
Plain text
utils/storage.js
Funciones
JavaScript
localStorage.setItem()

localStorage.getItem()

localStorage.removeItem()
3. Router SPA
¿Para qué sirve?
Permite cambiar de vistas sin recargar la página.
Archivo
Plain text
router/router.js
Funciones
JavaScript
window.addEventListener("hashchange")

window.location.hash = "#/dashboard"
Ejemplo de rutas
JavaScript
const routes = {
    "#/login": Login,
    "#/dashboard": Dashboard,
    "#/reservations": Reservations,
    "#/admin": AdminReservations
}
4. Auth Guard
¿Para qué sirve?
Impide que entren usuarios sin iniciar sesión.
Archivo
Plain text
guards/authGuard.js
Función
JavaScript
export function authGuard() {
    const user = JSON.parse(localStorage.getItem("user"))

    if (!user) {
        window.location.hash = "#/login"
        return false
    }

    return true
}
5. Role Guard
¿Para qué sirve?
Verifica si el usuario tiene permisos.
Archivo
Plain text
guards/roleGuard.js
Función
JavaScript
export function roleGuard(role) {
    const user = JSON.parse(localStorage.getItem("user"))

    return user.role === role
}
6. Dashboard
¿Para qué sirve?
Pantalla principal después del login.
Archivo
Plain text
views/Dashboard.js
Mostrar
Plain text
Bienvenido
Nombre
Rol
Botones de navegación
7. Gestión de Reservas
¿Para qué sirve?
CRUD completo de reservas.
Archivo
Plain text
services/reservationService.js
Funciones
JavaScript
getReservations()

createReservation()

updateReservation()

deleteReservation()
8. GET Reservas
¿Para qué sirve?
Obtiene reservas desde json-server.
JavaScript
export async function getReservations() {
    const response = await fetch(
        "http://localhost:3000/reservations"
    )

    return await response.json()
}
9. POST Reserva
¿Para qué sirve?
Crear una reserva nueva.
JavaScript
export async function createReservation(data) {
    await fetch(
        "http://localhost:3000/reservations",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    )
}
10. PUT/PATCH Reserva
¿Para qué sirve?
Actualizar una reserva.
JavaScript
export async function updateReservation(id,data){
    await fetch(
        `http://localhost:3000/reservations/${id}`,
        {
            method:"PATCH",
            headers:{
                "Content-Type":"application/json"
            },
            body:JSON.stringify(data)
        }
    )
}
11. DELETE Reserva
¿Para qué sirve?
Eliminar una reserva.
JavaScript
export async function deleteReservation(id){
    await fetch(
        `http://localhost:3000/reservations/${id}`,
        {
            method:"DELETE"
        }
    )
}
12. Regla de Negocio (No duplicados)
¿Para qué sirve?
Evita dos reservas para el mismo espacio y horario.
JavaScript
const exists = reservations.find(
    reservation =>
    reservation.spaceId === newReservation.spaceId &&
    reservation.date === newReservation.date &&
    reservation.startHour === newReservation.startHour
)

if(exists){
    alert("Horario ocupado")
}
13. Vista Usuario
¿Qué puede hacer?
Plain text
Crear reservas
Ver sus reservas
Editar pendientes
Cancelar reservas
Consulta filtrada:
JavaScript
const userReservations =
reservations.filter(
reservation =>
reservation.userId === currentUser.id
)
14. Vista Admin
¿Qué puede hacer?
Plain text
Ver todas las reservas
Crear
Editar
Eliminar
Aprobar
Rechazar
Ejemplo:
JavaScript
updateReservation(id,{
    status:"approved"
})
o
JavaScript
updateReservation(id,{
    status:"rejected"
})
15. Logout
¿Para qué sirve?
Cerrar sesión correctamente.
JavaScript
export function logout() {
    localStorage.removeItem("user")

    window.location.hash = "#/login"
}
16. Navbar Dinámico
¿Para qué sirve?
Mostrar opciones según el rol.
JavaScript
if(user.role === "admin"){
    // menú admin
}else{
    // menú user
}
17. Base de Datos (db.json)
JSON
{
  "users": [
    {
      "id": 1,
      "name": "Admin",
      "email": "admin@riwi.com",
      "password": "1234",
      "role": "admin"
    },
    {
      "id": 2,
      "name": "Juan",
      "email": "juan@riwi.com",
      "password": "1234",
      "role": "user"
    },
    {
      "id": 3,
      "name": "Maria",
      "email": "maria@riwi.com",
      "password": "1234",
      "role": "user"
    }
  ],

  "reservations": [],

  "spaces": [
    {
      "id": 1,
      "name": "Sala A",
      "type": "Meeting Room",
      "capacity": 10,
      "location": "Floor 1",
      "status": "available"
    }
  ]
}
Orden recomendado para desarrollar la prueba
Login ✅
Persistencia con localStorage ✅
Router SPA ✅
Auth Guard ✅
Role Guard ✅
Dashboard ✅
GET Reservas ✅
POST Reservas ✅
PATCH Reservas ✅
DELETE Reservas ✅
Vista User ✅
Vista Admin ✅
Validación de horarios duplicados ✅
Logout ✅
README en inglés ✅
Extras (Spaces, Dashboard, Toasts, Filtros) ⭐
Si sigues este orden, tendrás cubierto prácticamente el 100% de los requisitos obligatorios antes de empezar los puntos extra.