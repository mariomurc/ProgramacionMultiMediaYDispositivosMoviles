# Vista previa de componentes con @Preview
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

## Poner nombre a nuestras vistas previas
Cuando empezamos a tener muchas previews puede resultar complicado identificarlas.

Podemos utilizar el parámetro name:

```kotlin
@Preview(name = "Tarjeta pequeña", showBackground = true, widthDp = 200)
@Composable
fun TarjetaPequenaPreview() {
    Tarjeta()
}
```

El nombre nos permitirá reconocer con mayor facilidad qué situación estamos representando.

Por ejemplo, podríamos tener:

```kotlin
@Preview(name = "Texto corto",showBackground = true)
@Composable
fun MensajeCortoPreview() {
    Mensaje(texto = "Hola")
}
```
y otra preview:

```kotlin
Kotlin
@Preview(name = "Texto largo", showBackground = true)
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
    Usuario(nombre = "Alejandro)
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
fun TarjetaUsuario(nombre: String,ciudad: String) {
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

    Plain Text, Preview1 o Preview3

Es preferible utilizar:

    ProductoDisponiblePreview, ProductoAgotadoPreview o ProductoNombreLargoPreview

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
