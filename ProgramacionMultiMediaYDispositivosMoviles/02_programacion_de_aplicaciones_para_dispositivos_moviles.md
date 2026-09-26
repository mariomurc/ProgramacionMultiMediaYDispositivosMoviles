## Unidad 2. Programación de Aplicaciones para Dispositivos Móviles_

**Módulo:** Programación Multimedia y Dispositivos Móviles\
**Ciclo:** 2.º DAM\
**Revisión y actualización:** septiembre de 2026

# 1. Primera clase en un proyecto Android Studio
Cuando creamos un proyecto Android vacío, Android Studio genera automáticamente una clase principal denominada **MainActivity**.

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```

En este código aparecen conceptos que ya conocemos de Kotlin:

- Definición de clases.
- Herencia.
- Sobreescritura de métodos.
- Llamadas a métodos de la superclase.

## 1.1. Explicación básica
### Clase Principal
```kotlin
class MainActivity : AppCompatActivity()
```

Define una Activity, es decir, una pantalla de nuestra aplicación mediante la clase MainActivity, que hereda de AppCompatActivity. AppCompatActivity es una clase base proporcionada por Android para actividades (pantallas) que desean ser compatibles con versiones anteriores del sistema operativo. En este caso, la MainActivity será la actividad principal de la aplicación.

### Método onCreate()
```kotlin
override fun onCreate(savedInstanceState: Bundle?)
```

Es el primer método que se ejecuta cuando la Activity es creada. Esta línea sobrescribe (override) el método onCreate de la clase base AppCompatActivity. onCreate es el primer método que se llama cuando se crea la actividad, por lo que aquí se configuran los componentes de la interfaz de usuario y se inicializan las operaciones necesarias para la actividad.

El parámetro savedInstanceState: Bundle? contiene datos sobre el estado anterior de la actividad, si es que fue destruida y creada nuevamente (por ejemplo, al rotar la pantalla).

### Llamada a la superclase
```kotlin
super.onCreate(savedInstanceState)
```

Esta línea llama al método onCreate de la clase base (AppCompatActivity) para asegurar que el ciclo de vida básico de la actividad se gestione correctamente antes de añadir más lógica personalizada.

## 1.2. Creación de interfaces: XML vs Compose
Tradicionalmente la interfaz se definía en un archivo XML:

```kotlin
setContentView(R.layout.activity_main)
```

Este método carga el diseño definido en:

    res/layout/activity_main.xml

### Ejemplo tradicional
XML

```xml
<TextView android:text="Hola Mundo"/>
```

Actualmente Google recomienda utilizar **Jetpack Compose**, un sistema moderno basado en Kotlin.

En Compose la interfaz se genera mediante funciones:

```kotlin
setContent {
    PantallaPrincipal()
}
@Composable
fun PantallaPrincipal() {
    Text("Hola Mundo")
}
```

Hay una idea clave en toda esta evolución:
#### La Activity sigue existiendo. Lo que cambia es la forma de construir la interfaz gráfica.

# 2. Ciclo de vida de una aplicación Android
Toda Activity pasa por una serie de estados durante su existencia. El ciclo de vida es independiente de que utilicemos XML o Jetpack Compose.

Las principales fases son:

- onCreate()
- onStart()
- onResume()
- onPause()
- onStop()
- onDestroy()

![Ciclo de vida de una Activity](img/image1.png)

## ¿Por qué es importante?
Permite:

- **Inicializar recursos.**

Cuando la Activity se crea por primera vez solemos cargar los elementos necesarios para que la aplicación funcione.

Por ejemplo:
    - Conectar con una base de datos.
    - Cargar una lista de alumnos.
    - Inicializar un RecyclerView.
    - Preparar una conexión a internet.
<!-- -->

- **Guardar estados.**

Cuando el usuario gira el móvil o cambia temporalmente a otra aplicación, la Activity puede destruirse y volver a crearse. Si no guardamos información, los datos introducidos podrían perderse.

Ejemplo: Un alumno está rellenando un formulario:

    Nombre: Mario
    Curso: DAM2

Gira el dispositivo. Los datos desaparecen, sin guardar el estado:

    Nombre:
    Curso: 

Ejemplo sencillo:

```kotlin
override fun onSaveInstanceState(outState: Bundle) {
    super.onSaveInstanceState(outState)
    outState.putString("nombre", txtNombre.text.toString()
    )
}
```

**Ejemplo real:**

Un usuario está escribiendo una incidencia en una app de mantenimiento y recibe una llamada telefónica.
Al volver a la aplicación quiere continuar exactamente donde estaba.

- **Recuperar información.**

No basta con guardar. También debemos recuperar esos datos cuando la Activity vuelva a crearse.

Ejemplo:
```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
  super.onCreate(savedInstanceState)
  val nombre = savedInstanceState?.getString("nombre")
}
```
**Ejemplo real:**

Un usuario está viendo: **Alumno nº 145**
y gira el dispositivo. La aplicación debería seguir mostrando el mismo alumno y no volver al principio de la lista.

- **Liberar memoria cuando la aplicación deja de utilizarse.**

Los dispositivos móviles tienen memoria limitada.
Cuando una Activity deja de utilizarse debemos liberar los recursos que ya no necesitamos.

Ejemplos:
    - Cerrar conexiones con la base de datos.
    - Detener música.
    - Parar la cámara.
    - Cancelar peticiones de red.

Ejemplo:

```kotlin
override fun onDestroy() {
    super.onDestroy()
    conexion.close()
}
```

**Ejemplo real:**
Imagina una aplicación de Spotify. Si el usuario cierra la Activity y la aplicación sigue consumiendo recursos innecesariamente:
    - gastará batería
    - consumirá memoria
    - ralentizará el dispositivo


### Transiciones de estados más habituales

* **onCreate()** → **onStart()** → **onResume()** (la actividad está visible e interactiva)
* Si el usuario cambia de pantalla o la actividad ya no está en primer plano: **onPause()** → **onStop()**
* Si el usuario vuelve a la actividad, se llama nuevamente a **onRestart()** → **onStart()** → **onResume()**.
* Finalmente, si la actividad se cierra o se destruye: **onDestroy()**.

# 3. Componentes visuales: XML vs Jetpack Compose
Las interfaces Android están formadas por componentes visuales.

Tradicionalmente estos componentes se denominan **Views** y se agrupan dentro de **ViewGroups**.

## Algunos ejemplos de Views
    - TextView            - EditText
    - Button              - ImageView
    - CheckBox            - RadioButton

## Algunos ejemplos de ViewGroups
    - LinearLayout
    - ConstraintLayout
    - FrameLayout

Todos los elementos visuales deberían encontrarse organizados dentro de algún contenedor.

# 3.1. Equivalencias XML y Compose
Jetpack Compose utiliza componentes equivalentes escritos directamente en Kotlin.

<table>
<colgroup>
<col style="width: 61%" />
<col style="width: 38%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>XML</strong></th>
<th style="text-align: center;"><strong>Compose</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">&lt;TextView android:text="Hola"/&gt;</td>
<td style="text-align: center;">Text(<br />
&nbsp&nbsp&nbsp&nbsptext = "Hola"<br />
)</td>
</tr>
<tr>
<td style="text-align: center;">&lt;EditText android:hint="Nombre" /&gt;</td>
<td style="text-align: center;">TextField(<br />
&nbsp&nbsp&nbsp&nbspvalue = nombre,<br />
&nbsp&nbsp&nbsp&nbsponValueChange = {<br />
&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbspnombre = it<br />
}<br />
)</td>
</tr>
<tr>
<td style="text-align: center;">&lt;Button android:text="Aceptar"/&gt;</td>
<td style="text-align: left;">Button(<br />
&nbsp&nbsp&nbsp&nbsponClick = { }<br />
) {<br />
&nbsp&nbsp&nbsp&nbspText("Aceptar")<br />
}</td>
</tr>
<tr>
<td style="text-align: center;">&lt;ImageView android:src="@drawable/logo" /&gt;</td>
<td style="text-align: center;">Image(<br />
&nbsp&nbsp&nbsp&nbsppainter = painterResource(R.drawable.logo),<br />
&nbsp&nbsp&nbsp&nbspcontentDescription = null<br />
)</td>
</tr>
<tr>
<td style="text-align: center;">&lt;LinearLayout android:orientation="vertical"&gt;</td>
<td style="text-align: center;"><p>Column {</p>
<p>}</p></td>
</tr>
<tr>
<td style="text-align: center;">&lt;LinearLayout android:orientation="horizontal"&gt;</td>
<td style="text-align: center;"><p>Row {</p>
<p>}</p></td>
</tr>
<tr>
<td style="text-align: center;">&lt;FrameLayout&gt;</td>
<td style="text-align: center;"><p>Box {</p>
<p>}</p></td>
</tr>
<tr>
<td style="text-align: center;"><p>&lt;RecyclerView /&gt;</p>
<p>[RecyclerView necesita:</p>
<p>RecyclerView - Adapter - ViewHolder - LayoutManager</p>
<p>]</p></td>
<td style="text-align: center;"><p>LazyColumn {</p>
<p>}</p>
<p>[LazyColumn necesita:</p>
<p>LazyColumn - items() - Composable</p>
<p>]</p></td>
</tr>
</tbody>
</table>

**Views (Vistas) y sus tipos**

Las vistas son los bloques de construcción de la interfaz de usuario en Android. Son componentes que permiten controlar la interacción del usuario con la aplicación. Estos son muy similares a los controles SWING de Java, como Labels, Buttons, TextFields, Checkboxes, etc. Los Views son organizados dentro de los Layouts para que el usuario comprenda los objetivos de la actividad.

Se pueden agrupar para formar interfaces de usuario más complejas. Un ViewGroup es un contenedor que determina cómo se muestran las vistas.

El ViewGroup es el padre y las vistas dentro de él son sus hijos. Algunos tipos de ViewGroups son:

![Tipos de ViewGroup](img/image2.png)

Todos los elementos de una pantalla, deberían ir agrupados dentro de un ViewGroup.

La representación jerárquica de un ViewGroup (LinearLayout) podría ser esta:

![Jerarquía de un LinearLayout](img/image3.png)

![Panel de vistas de Android Studio](img/image4.png)

En Android Studio encontramos todas las vistas y grupos de vistas para trabajar con XML con las que podemos interactuar en una ventana como la de la imagen derecha.

Hay multitud de variantes y opciones a la hora de crear vistas y adaptarlas a las necesidades de nuestra aplicación.

# 4. Primera aplicación funcional

Hasta ahora usando XML podríamos interactuar pulsando un botón con este código:

```kotlin
val boton = findViewById<Button>(R.id.btnSumar)
boton.setOnClickListener {
}
```

Este enfoque sigue presente en muchos proyectos existentes. Conviene conocerlo, pero ya no es la técnica recomendada para proyectos nuevos.

Un Composable es una función capaz de generar interfaz gráfica.

```kotlin
@Composable
fun Saludo() {
    Text("Hola DAM")
}
```

La anotación:

@Composable indica que la función dibuja elementos visuales.

# 4.3. Eventos en Compose
```kotlin
Button(
    onClick = {
    }
) {
    Text("Aceptar")
}
```

El parámetro onClick contiene la lógica que se ejecutará al pulsar el botón.

## Enlaces a Ejemplos: 
    00_Contador
    01_SumaDosNumeros (Version XML y Composable)

# 4.4. Estado en Compose
# Administrar el estado

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

# 4.5. Vista previa de componentes con @Preview
Cuando estamos creando una interfaz con Jetpack Compose, es habitual realizar pequeños cambios continuamente: modificar un texto, cambiar un color, ajustar un tamaño, añadir un componente, etc.

Podríamos ejecutar la aplicación en el emulador cada vez que hacemos uno de estos cambios, pero durante el diseño de la interfaz existe una alternativa mucho más cómoda: **las vistas previas de Jetpack Compose mediante la anotación@Preview**.

@Preview permite visualizar un componente @Composable directamente desde Android Studio sin necesidad de ejecutar toda la aplicación. Esto resulta especialmente útil durante el desarrollo y diseño de la interfaz.

## Nuestra primera vista previa
Supongamos que tenemos el siguiente componente:

```kotlin
@Composable
fun Saludo(nombre: String) {
    Text ( text = "Hola, $nombre" ) }
```

Para visualizarlo podemos crear otra función:

```kotlin
@Preview
@Composable
fun SaludoPreview() {
    Saludo(nombre = "Mario")}
```

Aquí aparecen dos anotaciones:

### @Composable
Indica que la función puede formar parte de una interfaz creada con Jetpack Compose.

### @Preview
Indica a Android Studio que queremos visualizar ese Composable en la ventana de diseño.

La documentación oficial recomienda precisamente crear una función @Composable anotada con @Preview que invoque al componente que queremos visualizar.

Por tanto, podemos diferenciar entre **nuestro componente real (@Composable)**, y @Preview, que utilizamos **para comprobar visualmente cómo queda ese componente**.

**Importante:** normalmente no añadiremos @Preview directamente a todos nuestros componentes. Crearemos funciones específicas de preview que llamen al componente que queremos visualizar.

## Mostrar un fondo en la vista previa
En algunos componentes puede resultar difícil distinguir sus límites si el fondo de la preview coincide con el color de nuestra interfaz.

Podemos solicitar a Android Studio que muestre un fondo utilizando, por ejemplo:

```kotlin
@Preview(showBackground = true)
@Composable
fun SaludoPreview() {
    Saludo(nombre = "Mario")
}
```

Ahora Android Studio mostrará un fondo detrás del componente. Esto será especialmente útil cuando empecemos a trabajar con diferentes colores, márgenes y contenedores.

## Modificar el tamaño de la vista previa
También podemos establecer el ancho y el alto disponibles mediante widthDp y heightDp. La documentación oficial de Compose permite definir manualmente estas dimensiones en lugar de utilizar el tamaño calculado automáticamente.

Por ejemplo:

```kotlin
@Preview( showBackground = true, widthDp = 300, heightDp = 200 )
@Composable
fun TarjetaPreview() {
    Tarjeta()
}
```

Esto puede ayudarnos a comprobar qué sucede cuando nuestro componente dispone de más o menos espacio.

```kotlin
@Preview( showBackground = true, widthDp = 150 )
@Composable
fun TarjetaPequenaPreview() {
    Tarjeta()
}
```

Frente a:

```kotlin
@Preview(
    showBackground = true,
    widthDp = 400
)
@Composable
fun TarjetaGrandePreview() {
    Tarjeta()
}
```

De esta manera podemos detectar problemas relacionados con el tamaño disponible sin necesidad de modificar continuamente el emulador.

## Poner nombre a nuestras vistas previas
Cuando empezamos a tener muchas previews puede resultar complicado identificarlas.

Podemos utilizar el parámetro name:

```kotlin
@Preview(
    name = "Tarjeta pequeña",
    showBackground = true,
    widthDp = 200
)
@Composable
fun TarjetaPequenaPreview() {
    Tarjeta()
}
```

El nombre nos permitirá reconocer con mayor facilidad qué situación estamos representando.

Por ejemplo, podríamos tener:

```kotlin
@Preview(
    name = "Texto corto",
    showBackground = true
)
@Composable
fun MensajeCortoPreview() {
    Mensaje(texto = "Hola")
}
```
y otra preview:

```kotlin
Kotlin
@Preview(
    name = "Texto largo",
    showBackground = true
)
@Composable
fun MensajeLargoPreview() {
    Mensaje(
        texto = "Este es un mensaje bastante más largo para comprobar cómo se adapta nuestro componente."
    )
}
```

Esto introduce una idea muy importante:

**Una preview no sirve únicamente para comprobar si algo queda bonito. También podemos utilizarla para comprobar cómo responde nuestro componente ante diferentes situaciones.**

## Varias vistas previas del mismo componente
Un mismo componente puede tener tantas funciones de preview como necesitemos.

Imaginemos:

```kotlin
@Composable
fun Usuario(nombre: String) {
    Text(text = nombre)
}
```

Podemos probar diferentes datos:

```kotlin
@Preview(showBackground = true)
@Composable
fun UsuarioPreview() {
    Usuario(nombre = "Ana")
}
@Preview(showBackground = true)
@Composable
fun UsuarioNombreLargoPreview() {
    Usuario(
        nombre = "Alejandro
    }
```

Incluso es posible aplicar @Preview varias veces para visualizar un mismo composable con diferentes propiedades. Android Studio ofrece modos de visualización para trabajar con varias previews simultáneamente.

Esta posibilidad se vuelve especialmente interesante según aumente la complejidad de nuestras interfaces.

Por ejemplo, más adelante podríamos comprobar estados como:

- Producto disponible.
- Producto agotado.
- Carrito vacío.
- Carrito con productos.
- Usuario conectado.
- Usuario sin identificar.
- Información cargando.
- Error al recuperar la información.

No necesitaremos aprender ahora cómo implementar todos esos estados. Lo importante es entender que **las previews pueden ayudarnos a visualizar diferentes situaciones de nuestra interfaz**.

## @Preview no sustituye al emulador
Es importante diferenciar ambas herramientas. Una preview está pensada principalmente para facilitar el **diseño y comprobación rápida de nuestra interfaz**. El emulador o dispositivo real nos permite ejecutar la aplicación y comprobar su comportamiento completo.

Por ejemplo, podemos utilizar @Preview para comprobar rápidamente:

- Cómo queda un botón
- La distribución de una tarjeta
- El aspecto de una pantalla
- Distintos tamaños
- Diferentes textos
- Distintas configuraciones visuales

Mientras que necesitaremos ejecutar la aplicación cuando queramos comprobar su funcionamiento real y su integración con el resto de elementos.

De hecho, una de las ventajas que Google destaca de @Preview es precisamente evitar depender constantemente del emulador mientras realizamos pequeños cambios sobre la interfaz.

## Diseñar componentes pensando en las previews
Existe una práctica muy recomendable que iremos aplicando durante el curso.

Un componente resulta más sencillo de visualizar y reutilizar cuando **los datos que necesita se reciben mediante parámetros**.

Por ejemplo, es preferible tener:

```kotlin
@Composable
fun TarjetaUsuario(
    nombre: String,
    ciudad: String
) {
    Column {
        Text(text = nombre)
        Text(text = ciudad)
    }
}
```

Ahora podemos crear fácilmente:

```kotlin
@Preview(showBackground = true)
@Composable
fun TarjetaUsuarioPreview() {
    TarjetaUsuario(
        nombre = "Mario",
        ciudad = "Madrid"
    )
}
```

Y otra situación:

```kotlin
@Preview(showBackground = true)
@Composable
fun TarjetaUsuarioNombreLargoPreview() {
    TarjetaUsuario(
        nombre = "Alejandro García Rodríguez",
        ciudad = "Madrid"
    )
}
```

Este pequeño ejemplo anticipa una idea que será muy importante más adelante: **separar el estado de la representación visual de nuestros componentes**.

La propia documentación de Compose recomienda estructurar las pantallas teniendo en cuenta el estado que reciben como entrada y los eventos que generan como salida para aprovechar mejor las previews. Volveremos sobre esto cuando estudiemos el **estado en Jetpack Compose**.

## Buenas prácticas con @Preview
Cuando trabajemos con previews intentaremos seguir estas recomendaciones:

### 1. Separar el componente de su preview
Mejor:

```kotlin
@Composable
fun BotonAceptar() {
    Button(onClick = {}) {
        Text("Aceptar")
    }
}
@Preview
@Composable
fun BotonAceptarPreview() {
    BotonAceptar()
}
```

Así no mezclamos la función real del componente con la utilizada para visualizarlo.

### 2. Utilizar nombres descriptivos
Si tenemos varias previews evitaremos nombres como:

    Plain Text, Preview1, Preview2 o Preview3

Es preferible utilizar:

    Plain Text, ProductoDisponiblePreview, ProductoAgotadoPreview o ProductoNombreLargoPreview

De esta manera sabemos inmediatamente qué situación estamos comprobando.

### 3. Probar situaciones diferentes
No debemos utilizar @Preview únicamente con el ejemplo perfecto.

También conviene probar:

- textos muy cortos
- textos largos
- componentes con poco espacio
- información ausente cuando nuestro diseño lo permita
- diferentes estados visuales.

Eso nos ayudará a encontrar problemas antes. 
