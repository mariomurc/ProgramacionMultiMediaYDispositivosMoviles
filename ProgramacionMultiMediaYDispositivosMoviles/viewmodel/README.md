# ViewModel

El `ViewModel` es un componente esencial en la arquitectura de Android, particularmente en Jetpack Compose, ya que permite separar la lógica de negocio y los datos del ciclo de vida de la interfaz gráfica.

Los `ViewModel` están diseñados para:

* Almacenar y gestionar datos de estado relacionados con la UI.
* Sobrevivir a cambios de configuración como rotación de pantalla.
* Facilitar la conexión entre la UI y la lógica de negocio.

En Jetpack Compose, un `ViewModel` permite emitir estados observables hacia las composables de forma reactiva.

### **Configuración**

Incluye la siguiente dependencia en tu archivo `build.gradle`:

```groovy
implementation("androidx.lifecycle:lifecycle-viewmodel-compose:2.8.7")
```

### Arquitectura CLEAN

Si seguimos la arquitectura CLEAN, los `ViewModel` deben declararse en el paquete presentation.viewmodel

```
com.example.myapp
│
├── data
│   ├── model
│   ├── repository
│   ├── source
│   │   ├── local
│   │   └── remote
│   └── mapper
│
├── domain
│   ├── model
│   ├── repository
│   └── usecase
│
├── presentation
│   ├── ui
│   │   ├── components
│   │   ├── screens
│   │   │   ├── home
│   │   │   ├── profile
│   │   │   └── settings
│   ├── navigation
│   └── viewmodel
│
├── di
│
└── utils
```

## Crear un ViewModel

Un `ViewModel` se utiliza para manejar algún estado.  Todo `ViewModel` debe declarar las siguientes partes:

1. **Variable de estado mutable** (`MutableStateFlow`): define el estado que gestiona el `ViewModel`. Este estado es mutable y debe declararse privado para que los `@Composable` no puedan acceder al mismo directamente.
2. **Variable de estado inmutable** (`StateFlow`): es una versión de solo lectura del estado anterior. Se expone públicamente para que los @Composable puedan observar los cambios de estado sin tener capacidad para modificarlos. De esta forma se garantiza que el estado solo pueda alterarse desde el propio ViewModel, siguiendo el principio de encapsulación.
3. **Funciones de actualización o acciones del estado**: son las funciones encargadas de modificar el estado interno del ViewModel. Estas funciones representan las acciones que puede realizar el usuario o la propia aplicación (por ejemplo, actualizar un texto, cargar datos, marcar una tarea como completada o reiniciar el estado). Todas las modificaciones sobre el MutableStateFlow deben realizarse a través de estas funciones.

El `ViewModel` de cada ventana se declara como parámetro de su función `@Composable` y se instancia por defecto usando el método `viewModel()`.
