## Conversión Ejemplo Básico de Contador a MVVM

En MVVM (Model-View-ViewModel) el proceso sería:
1. View (Jetpack Compose): solo muestra datos y captura eventos.
2. ViewModel: contiene el estado y la lógica de negocio.
3. Model: los datos (en este caso ni siquiera necesitas uno porque solo hay un contador).

[LINK AL PROYECTO 02_Contador_MVVM](https://cloud.educa.madrid.org/s/Ddw6WRQNgf8KLEJ)

## Resultado visual de la arquitectura (ANTES)
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

## Resultado visual de la arquitectura (DESPUÉS)
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
## Regla práctica para Compose + MVVM
Cuando tengas código dentro de un @Composable, pregúntate:
1. ¿Solo pinta información? → Se queda en la View.
2. ¿Modifica datos o toma decisiones? → Va al ViewModel.
3. ¿Accede a base de datos, API REST o Room? → Repositorio/Model.



## Ejemplo de Login Screen

Otro ejemplo, podríamos modificar la ventana de Login del Ejercicio 6 para que use un `ViewModel`

Para entender el MVVM, la transición podría darse en varios pasos:

1. [03_LoginScreen](https://cloud.educa.madrid.org/s/Ddw6WRQNgf8KLEJ): Solución al ejercicio sin usar MVVM

2. [03_LoginScreen_MVVM](https://cloud.educa.madrid.org/s/Ddw6WRQNgf8KLEJ): Solución al ejercicio usando MVVM de forma sencilla y didáctica para entender que cambia entre el planteamiento anterior y este

3. [03_LoginScreen_MVVM_Mejorado](https://cloud.educa.madrid.org/s/Ddw6WRQNgf8KLEJ): Solución al ejercicio usando MVVM de forma "ortodoxa" como se resolvería en un proyecto real(teniendo en cuenta la simplicidad del problema)
   
En esta transición entre el ejemplo 1 y el 2 es importante entender la siguiente evolución:

| Nivel | Código | Explicación |
|--------|--------|-------------|
| **1** | `var usuario by remember { mutableStateOf("") }` | El estado vive dentro del Composable. Si la pantalla desaparece, el estado también. |
| **2** | `var usuario by mutableStateOf("")` | El estado vive dentro del ViewModel. Sobrevive a recomposiciones y separa UI de lógica. |
| **3** | `MutableStateFlow(UiState())` | El estado completo de la pantalla se representa mediante una única `data class`. El ViewModel es el único que puede modificarla y la UI la observa mediante `collectAsState()`. Facilita el mantenimiento, las pruebas y la integración con APIs, Room o Firebase. |

==COMO PRÁCTICA PARA SEGUIR ENTENDIENDO EL MVVM, ¿PODRÍAS RESOLVER LOS EJERCICIOS DEL 2, 3, 5, 7 Y 8 USANDO MVVM?==


# StateFlow: Regla para Recordar

## Conceptos clave

Cuando veas:

```kotlin
MutableStateFlow
```
Piensa: "Aquí guardo el estado de mi aplicación."

Cuando veas:

```kotlin
collectAsState()
```
Piensa: "La pantalla está escuchando ese estado."

Cuando el estado cambia mediante:

```kotlin
.value = ...
```
Jetpack Compose detecta el cambio y recompone automáticamente la UI.

## El flujo recomendado en MVVM
```
View
 │
 ▼
Evento del usuario
 │
 ▼
ViewModel
 │
 ▼
StateFlow actualiza estado
 │
 ▼
Compose observa cambios
 ↓
UI actualizada
```
