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

# 4.1. Eventos en Compose
```kotlin
Button(
    onClick = {
    }
) {
    Text("Aceptar")
}
```

El parámetro onClick contiene la lógica que se ejecutará al pulsar el botón.

## Enlaces a Ejemplos 
   - [00_Contador](https://cloud.educa.madrid.org/s/Ddw6WRQNgf8KLEJ)
   - [01_SumaDosNumeros (Version XML y Composable)](https://cloud.educa.madrid.org/s/Ddw6WRQNgf8KLEJ)
