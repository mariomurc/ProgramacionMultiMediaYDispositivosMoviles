# La clase Toast
La clase Toast permite mostrar mensajes breves en pantalla.

Un Toast:
- No bloquea la aplicación.
- No requiere interacción del usuario.
- Desaparece automáticamente tras unos segundos.

Se utiliza habitualmente para:
- Confirmar acciones.
- Mostrar errores sencillos.
- Informar al usuario de que una operación se ha completado.

Ejemplo:

```kotlin
Toast.makeText(this, "Operación realizada", Toast.LENGTH_SHORT ).show()
```

## Toast en Jetpack Compose
Cuando trabajamos con Compose necesitamos obtener el contexto de la aplicación:

```kotlin
val context = LocalContext.current
```
Ejemplo:
```kotlin
Button(
    onClick = { Toast.makeText(context, "Hola DAM", Toast.LENGTH_SHORT).show() }
){
    Text("Mostrar mensaje")
}
```

## Ejercicio 1
Solicitar un nombre y una edad mediante un TextField y mostrar: "Hola [nombre], tienes [edad] años por tanto eres mayor/menor de edad" usando un Toast.
