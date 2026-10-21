# MutableState vs ViewModel

El uso de un **ViewModel** frente a un estado normal (`MutableState`) es necesario para abordar las limitaciones de las composables en Jetpack Compose y garantizar una gestión eficiente del estado en aplicaciones Android. Aquí tienes una comparación detallada que explica **por qué es esencial usar un ViewModel en lugar de estados simples** como `remember` o `mutableStateOf`:

## **Gestión de estados con MutableState (estado normal)**

Un estado normal en Jetpack Compose se refiere a usar herramientas como `remember` o `mutableStateOf` para gestionar valores reactivos dentro de una composable. Ejemplo básico:

```kotlin
@Composable
fun CounterScreen() {
    var count by remember { mutableStateOf(0) }

    Column {
        Text("Count: $count")
        Button(onClick = { count++ }) {
            Text("Increment")
        }
    }
}
```

Este enfoque es útil para manejar estados locales simples, pero tiene limitaciones importantes.

## **Limitaciones de la gestión de estados con MutableState**

1. **No persiste cambios de configuración:**
   * Cuando se rota la pantalla o se recrea la actividad, el estado gestionado con `remember` se reinicia, perdiendo el valor almacenado.
   * Ejemplo: Si el contador estaba en 5 y giras la pantalla, se reiniciará a 0.
2. **Falta de separación de responsabilidades:**
   * Manejar el estado directamente en la UI puede acoplar demasiado la lógica de negocio con la interfaz de usuario, haciendo el código más difícil de mantener.
3. **Dificultad para compartir datos:**
   * Un estado local no es fácil de compartir entre composables o pantallas. Si varias partes de la UI necesitan el mismo estado, sería necesario propagarlo manualmente, lo que aumenta la complejidad.
4. **No adecuado para lógica compleja:**
   * Operaciones más avanzadas como cargas asincrónicas, validaciones o combinaciones de estados son difíciles de manejar con estados simples en Compose.

## **¿Por qué ViewModel es necesario?**

El **ViewModel** aborda estas limitaciones al proporcionar un mecanismo robusto para gestionar estados de manera consistente, reactiva y escalable.

### **Ventajas del ViewModel frente a un estado normal**

1. **Persistencia del estado:**
   * El ViewModel está diseñado para sobrevivir a cambios de configuración como rotaciones de pantalla o recreaciones de actividades/fragments.
   * Ejemplo: Si el contador está en 5 y rotas la pantalla, el estado se mantiene.
2. **Separación de responsabilidades (MVVM):**
   * El ViewModel actúa como una capa intermedia entre la UI y la lógica de negocio. Esto desacopla la gestión del estado de la presentación, haciendo el código más modular y fácil de probar.
3. **Compatibilidad con Coroutines:**
   * El ViewModel tiene un `viewModelScope` que facilita ejecutar tareas asincrónicas como llamadas a APIs o operaciones de base de datos sin preocuparse por fugas de memoria.
4. **Reactividad nativa:**
   * Los datos gestionados en un ViewModel pueden exponerse a la UI usando herramientas reactivas como **StateFlow** o **LiveData**, lo que garantiza que la UI siempre refleje el estado actual.
5. **Reutilización de estado:**
   * Un ViewModel puede ser compartido entre diferentes pantallas o composables, permitiendo que varias partes de la UI accedan al mismo estado.
6. **Gestión eficiente del ciclo de vida:**
   * Android asegura que el ViewModel está vinculado al ciclo de vida del componente que lo usa (por ejemplo, una actividad o fragmento), pero no es destruido hasta que este realmente desaparece.

## **Cuándo usar ViewModel**

1. **Estados que deben sobrevivir a cambios de configuración:**
   * Ejemplo: Formularios largos, resultados de búsquedas, progreso de tareas en segundo plano.
2. **Lógica de negocio compartida entre composables o pantallas:**
   * Ejemplo: Carrito de compras compartido entre la lista de productos y la pantalla de checkout.
3. **Integración con APIs o bases de datos:**
   * Ejemplo: Obtener datos desde un servidor o guardar datos localmente.
4. **Manejo de estados complejos:**
   * Ejemplo: Combinar múltiples flujos de datos (loading, errores, contenido) en un solo estado reactivo.

## Cuándo usar MutableState

El uso de **MutableState** en lugar de un **ViewModel** es apropiado en escenarios específicos donde el estado es **local a la composición** y **no necesita persistir** más allá del ciclo de vida de la misma.

### **1. Estados locales simples**

Si el estado solo es relevante para un composable específico y no necesita ser compartido ni persistir tras un cambio de configuración.

Por ejemplo, un contador local dentro de un composable:

```kotlin
@Composable
fun Counter() {
    var count by remember { mutableStateOf(0) }

    Column {
        Text("Count: $count")
        Button(onClick = { count++ }) {
            Text("Increment")
        }
    }
}
```

**Por qué usar `MutableState`:**

* No hay lógica compleja ni necesidad de persistencia.
* El estado no necesita ser compartido con otras composables o pantallas.

### **2. Estados efímeros (no persistentes)**

Cuando el estado no necesita sobrevivir a cambios de configuración, como rotaciones de pantalla.

Por ejemplo, un diálogo temporal que almacena el estado de "visible" o "oculto":

```kotlin
@Composable
fun ShowDialogExample() {
    var showDialog by remember { mutableStateOf(false) }

    Column {
        Button(onClick = { showDialog = true }) {
            Text("Show Dialog")
        }
        if (showDialog) {
            AlertDialog(
                onDismissRequest = { showDialog = false },
                title = { Text("Example Dialog") },
                confirmButton = {
                    Button(onClick = { showDialog = false }) {
                        Text("OK")
                    }
                }
            )
        }
    }
}
```

**Por qué usar `MutableState`:**

* El estado (`showDialog`) es efímero y no necesita persistir.

### **3. Prototipos o composables aislados**

Si estás trabajando en un componente aislado o prototipo que no interactúa con otras partes de la aplicación.

Por ejemplo, un temporizador local dentro de una pantalla de prueba:

```kotlin
@Composable
fun Timer() {
    var seconds by remember { mutableStateOf(0) }

    LaunchedEffect(Unit) {
        while (true) {
            // Introduce un retraso de un segundo 
            kotlinx.coroutines.delay(1000)
            seconds++
        }
    }

    Text("Seconds elapsed: $seconds")
}
```

**Por qué usar `MutableState`:**

* El estado (`seconds`) es temporal y no forma parte de la lógica central de la aplicación.

### **4. Estados derivados o temporales**

Si el estado es un cálculo derivado de otros valores y no necesita persistir más allá de la composición.

Por ejemplo, mostrar si un número es par o impar:

```kotlin
kotlinCopy code@Composable
fun NumberChecker() {
    var number by remember { mutableStateOf(0) }
    val isEven = remember(number) { number % 2 == 0 }

    Column {
        Text("Number: $number")
        Text(if (isEven) "Even" else "Odd")
        Button(onClick = { number++ }) {
            Text("Increment")
        }
    }
}
```

**Por qué usar `MutableState`:**

* No necesitas manejar lógica compleja ni persistencia.

## **Comparativa MutableState y ViewModel**

| Característica                    | Estado Normal | ViewModel                       |
| --------------------------------- | ------------- | ------------------------------- |
| **Persistencia en rotación**      | No            | Sí                              |
| **Desacoplamiento**               | Bajo          | Alto                            |
| **Reactividad**                   | Limitada      | Completa con StateFlow/LiveData |
| **Compatibilidad con Coroutines** | Limitada      | Sí                              |
| **Escalabilidad**                 | Baja          | Alta                            |
