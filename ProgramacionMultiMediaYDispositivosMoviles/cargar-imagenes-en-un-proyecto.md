# Cargar imágenes en el proyecto

Como hemos visto, cargar y mostrar imágenes es sencillo gracias a `Image`, un componente incorporado que puedes personalizar. A continuación vamos a ver cómo cargar imágenes desde distintos orígenes en un proyecto de Jetpack Compose.

## Cargar imágenes desde los recursos (`drawable`)

El directorio `drawable` (dentro de la carpeta res) está diseñado para almacenar imágenes y recursos gráficos que la aplicación utiliza en su interfaz. Las imágenes en `drawable` se escalan automáticamente para adaptarse a la densidad de pantalla del dispositivo, y puedes tener múltiples carpetas `drawable` para cada densidad de pantalla:

* `drawable-mdpi` (160 dpi)
* `drawable-hdpi` (240 dpi)
* `drawable-xhdpi` (320 dpi)
* `drawable-xxhdpi` (480 dpi)
* `drawable-xxxhdpi` (640 dpi)

### Importar una imagen

Para importar una imagen a la carpeta `res/drawable` en Android Studio, sigue estos pasos:



{% stepper %}
{% step %}
### Selecciona el Archivo de Imagen

Prepara el archivo de imagen que quieres agregar a `drawable`. Asegúrate de que el archivo esté en uno de los formatos soportados por Android, como **PNG**, **JPG**, **WEBP** o **SVG**.
{% endstep %}

{% step %}
### Copia la Imagen en el Proyecto

1. **Abre Android Studio** y navega a la vista de **Project** (Proyecto) en la barra lateral izquierda.
2. Navega hasta `app > res > drawable`.
3. Haz clic derecho en la carpeta **drawable** y selecciona **Paste** (Pegar), o simplemente arrastra el archivo desde tu sistema de archivos y suéltalo en la carpeta `drawable`.
{% endstep %}

{% step %}
### Renombra el Archivo

Cuando pegas la imagen, Android Studio te pedirá confirmar el nombre. **Evita usar espacios, letras mayúsculas o caracteres especiales**. Los nombres válidos deben seguir las convenciones de Android, por lo que deben estar en **letras minúsculas y con guiones bajos** para separar palabras, como `mi_imagen.png`.
{% endstep %}

{% step %}
### Verifica la Imagen en el Proyecto

Una vez copiada, la imagen aparecerá en `res/drawable`. Ahora puedes referenciarla en tu código.
{% endstep %}

{% step %}
### Usar la Imagen en tu Código

Para cargar imágenes almacenadas en el directorio `res/drawable`, utiliza la función `painterResource`:

```kotlin
import androidx.compose.foundation.Image
import androidx.compose.runtime.Composable
import androidx.compose.ui.res.painterResource
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.Modifier
import androidx.compose.ui.layout.ContentScale
import androidx.compose.ui.graphics.ColorFilter
import com.example.tuapp.R

@Composable
fun ImagenDesdeDrawable() {
    Image(
        painter = painterResource(id = R.drawable.mi_imagen),
        contentDescription = "Descripción de la imagen",
        modifier = Modifier.size(128.dp),
        contentScale = ContentScale.Crop,
        colorFilter = ColorFilter.tint(Color.Gray) // Opcional, para aplicar un tinte
    )
}
```
{% endstep %}
{% endstepper %}

## Cargar imágenes desde la web (URL) con Coil

Para cargar imágenes desde una URL, te recomiendo usar **Coil** (Coil es una librería de carga de imágenes compatible con Jetpack Compose). Primero, añade la dependencia de Coil a tu archivo `build.gradle`:

```gradle
implementation("io.coil-kt:coil-compose:2.0.0") // o la versión más reciente
```

Luego, puedes utilizar el componente `AsyncImage` de Coil:

```kotlin
import androidx.compose.runtime.Composable
import coil.compose.AsyncImage
import androidx.compose.ui.Modifier

@Composable
fun ImagenDesdeURL() {
    AsyncImage(
        model = "https://ejemplo.com/mi_imagen.png",
        contentDescription = "Imagen desde la web",
        modifier = Modifier.size(128.dp),
        contentScale = ContentScale.Crop
    )
}
```
