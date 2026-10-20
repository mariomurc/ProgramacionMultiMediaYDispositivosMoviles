# Primera clase en un proyecto Android Studio
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

## Explicación básica
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

## Creación de interfaces: XML vs Compose
Tradicionalmente la interfaz se definía en un archivo XML:

```kotlin
setContentView(R.layout.activity_main)
```
Este método carga el diseño definido en:

    res/layout/activity_main.xml

Históricamente, el desarrollo de interfaces en Android se ha realizado mediante archivos XML (Extensible Markup Language). En este enfoque, se definen las vistas y la jerarquía de la interfaz en archivos separados del código de la lógica de la aplicación (en archivos .xml dentro de la carpeta res/layout). Los componentes visuales como botones, textos, imágenes y listas se crean usando etiquetas XML que describen las propiedades visuales y su disposición.:

### Ejemplo tradicional
XML

```xml
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hola, Mundo!" />

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Presionar" />
</LinearLayout>
```
Las principales características son:
* Las vistas se describen mediante XML, pero la lógica de interacción se maneja en código Java o Kotlin.
* Jerarquía de vistas: Las vistas (Views) se anidan en una estructura jerárquica, donde cada componente ocupa un espacio en memoria.
* UI Imperativa: Aunque la estructura de la interfaz es declarativa (XML), su manipulación en tiempo de ejecución es más imperativa. Es decir, el desarrollador debe actualizar las vistas manualmente a través de código cuando cambien los datos.
* Fragmentación del código: El diseño y la lógica se manejan en archivos separados (XML y código Java/Kotlin).

Actualmente Google recomienda utilizar **Jetpack Compose**, un sistema moderno basado en Kotlin. Jetpack Compose es un moderno kit de herramientas declarativo de Android que permite crear interfaces de usuario completamente en código, sin necesidad de XML. Lanzado oficialmente por Google en 2020, Jetpack Compose está escrito en Kotlin y simplifica la creación de interfaces gracias a un enfoque declarativo, inspirado por frameworks como React o SwiftUI.

En Compose la interfaz se genera mediante funciones:

```kotlin
setContent {
    PantallaPrincipal()
}
@Composable
fun PantallaPrincipal() {
    Column {
        Text(text = "Hola, Mundo!")
        Button(onClick = { /* Acción al presionar */ }) {
            Text(text = "Presionar")
        }
    }
}
```

Las principales características son:

* Declarativo completo: La interfaz se construye mediante código declarativo. Se describe el estado de la UI y Jetpack Compose se encarga de renderizar los cambios cuando el estado subyacente cambia.
* UI Reactiva: La interfaz está vinculada al estado de los datos. Si el estado cambia, la UI se actualiza automáticamente, sin necesidad de manipular manualmente las vistas.
* Menos fragmentación: Todo, desde el diseño de la UI hasta la lógica, puede estar en el mismo archivo, lo que facilita el manejo del código.
* Optimización del rendimiento: Jetpack Compose utiliza menos jerarquías de vistas que el enfoque basado en XML, lo que puede mejorar el rendimiento en términos de memoria y procesamiento.
* Compatibilidad con vistas heredadas: Aunque es un enfoque moderno, Compose es compatible con vistas tradicionales de Android, lo que permite mezclar ambas técnicas si es necesario.
