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

## Conversión Ejemplo Básico de Contador a MVVM

En MVVM (Model-View-ViewModel) el proceso sería:
1. View (Jetpack Compose): solo muestra datos y captura eventos.
2. ViewModel: contiene el estado y la lógica de negocio.
3. Model: los datos (en este caso ni siquiera necesitas uno porque solo hay un contador).

[LINK AL PROYECTO 02_Contador_MVVM](https://cloud.educa.madrid.org/s/Ddw6WRQNgf8KLEJ)

### Resultado visual de la arquitectura (ANTES)
```
Composable
│
├── Estado (contador)
├── Lógica (contador++)
└── Interfaz de usuario
```
Problemas
1. La UI conoce y gestiona el estado.
2. La lógica está mezclada con la presentación.
3. Es más difícil de probar.
4. Si el Composable desaparece de memoria, el estado puede perderse.

### Resultado visual de la arquitectura (DESPUÉS)
```
View (Compose)
│
├── Muestra contador
└── Captura click
      │
      ▼
ViewModel
│
├── contador
└── incrementar()
```    
### Regla práctica para Compose + MVVM
Cuando tengas código dentro de un @Composable, pregúntate:
1. ¿Solo pinta información? → Se queda en la View.
2. ¿Modifica datos o toma decisiones? → Va al ViewModel.
3. ¿Accede a base de datos, API REST o Room? → Repositorio/Model.



## Ejemplo de Login Screen

Otro ejemplo, podríamos modificar la ventana de Login del Ejercicio 6 para que use un `ViewModel`

Para entender el MVVM, la transición podría darse en varios pasos:

[1. 03_LoginScreen](https://cloud.educa.madrid.org/s/Ddw6WRQNgf8KLEJ): Solución al ejercicio sin usar MVVM
[2. 03_LoginScreen_MVVM](https://cloud.educa.madrid.org/s/Ddw6WRQNgf8KLEJ): Solución al ejercicio usando MVVM de forma sencilla y didáctica para entender que cambia entre el planteamiento anterior y este
CONTINÚA ....
<!-- 4. 03_LoginScreen_MVVM_Mejorado(): Solución al ejercicio usando MVVM de forma "ortodoxa" como se usaría en un proyecto real -->
