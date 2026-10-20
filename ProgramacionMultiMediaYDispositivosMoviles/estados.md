# Estado en Compose
## Administrar el estado

La gestión de estado en **Jetpack Compose** es un aspecto fundamental para crear interfaces reactivas y eficientes. Dado que **Jetpack Compose** está basado en la programación declarativa, el manejo del estado es crucial para actualizar la interfaz de usuario (UI) de forma adecuada. Vamos a hacer un tutorial en profundidad que cubra los conceptos clave, las herramientas proporcionadas por Compose, y algunos ejemplos prácticos.

## **Declarativo vs Imperativo**

Jetpack Compose sigue un paradigma de programación **declarativa**, a diferencia del paradigma **imperativo** que se encuentra en la programación tradicional de Android (por ejemplo, con XML y `findViewById`). En una UI declarativa, la interfaz se describe como una función del estado. Esto significa que cuando el estado cambia, la UI se actualiza automáticamente para reflejar esos cambios.

### **¿Qué es el estado en una UI declarativa?**

El estado es cualquier dato que afecta la representación de la interfaz de usuario. En Compose, este estado se puede manejar de manera que las vistas sean reactivas, es decir, que se actualicen automáticamente cada vez que el estado cambia.

```kotlin
@Composable
fun Contador() {
    // contador es de tipo MutableState<Int>
    var contador = remember { mutableStateOf(0) }
    
    Column {
        Text(text = "Contador: ${contador.value}")
        Button(onClick = { contador.value++ }) {
            Text("Incrementar")
        }
    }
}
```

En este ejemplo, cada vez que el botón se presiona, el valor de `contador` se actualiza y, debido a la naturaleza declarativa de Compose, el `Text` se recompone automáticamente.

## **¿Qué es una recomposición?**

Una **recomposición** es el proceso mediante el cual una función `@Composable` se vuelve a ejecutar cuando uno o más datos de estado de los que depende cambian. Esto ocurre debido al enfoque **declarativo** de Compose: la interfaz de usuario se vuelve a "recomponer" cuando detecta un cambio en el estado que influye en la representación visual.

En una aplicación **imperativa** tradicional, se tendría que modificar manualmente la interfaz de usuario cuando cambia un estado. Sin embargo, en Compose, no se modifican los elementos manualmente; en su lugar, el sistema vuelve a ejecutar las funciones `@Composable`  pertinentes, recreando la UI con los nuevos valores de estado.

#### **2. ¿Cuándo ocurre una recomposición?**

La recomposición ocurre cuando:

* **El estado que afecta un `@Composable`  cambia.** Por ejemplo, si un `Text` muestra el valor de un `mutableStateOf`, y ese valor cambia, la recomposición ocurre para que el `Text` muestre el nuevo valor.
* **Un efecto de estado cambia.** Al usar funciones como `LaunchedEffect` o `remember`, los efectos que dependen de algún valor pueden desencadenar recomposiciones.

## Funciones para la gestión del estado

### **`remember`**

La función `remember` es crucial para almacenar valores a lo largo del ciclo de vida de una función composable. Si no usas `remember`, el valor se reseteará cada vez que la función se vuelva a ejecutar (es decir, en cada recomposición).

```kotlin
@Composable
fun EjemploRemember() {
    val nombre = remember { mutableStateOf("Compose") }

    Text(text = "Hola, ${nombre.value}")
}
```

### **`mutableStateOf`**

`mutableStateOf` se usa para crear un estado mutable en Compose. Se comporta como un observable, lo que significa que cuando su valor cambia, los composables que dependen de ese valor se recomponen.

```kotlin
val nombre = remember { mutableStateOf("Mundo") }
nombre.value = "Compose"
```

### **`rememberSaveable`**

`rememberSaveable` es similar a `remember`, pero además guarda el estado a través de cambios de configuración, como la rotación de pantalla. Esto lo hace ideal para gestionar datos que no deseas que se pierdan en estos casos.

```kotlin
@Composable
fun EjemploRememberSaveable() {
    var contador by rememberSaveable { mutableStateOf(0) }

    Button(onClick = { contador++ }) {
        Text("Contador: $contador")
    }
}
```

### Estados derivados

`derivedStateOf` se usa para crear un nuevo estado que depende de otro, optimizando así el número de recomposiciones.

```kotlin
val scrollState = rememberScrollState()
val isScrolledToEnd = remember {
    derivedStateOf { scrollState.value == scrollState.maxValue }
}
```

### Azúcar sintático con by

Es posible utilizar el mecanismo de azúcar sintáctico by para evitar tener que estar llamando al `.value` de un estado:

```kotlin
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.setValue

@Composable
fun Contador() {
    // contador es de tipo MutableState<Int>
    var contador by remember { mutableStateOf(0) }
    
    Column {
        Text(text = "Contador: ${contador}")
        Button(onClick = { contador++ }) {
            Text("Incrementar")
        }
    }
}
```

Para que el código anterior funcione, es necesario añadir los dos import `getValue` y `setValue`
