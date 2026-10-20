# Modificadores
El **`Modifier`** en Jetpack Compose es un objeto flexible y poderoso que permite modificar el comportamiento, la disposición y el estilo de los elementos UI. Se puede usar para ajustar el tamaño, el padding, agregar clics, gestionar gestos y mucho más.

## Composición de Modifier

Es importante destacar que los modificadores pueden ser **encadenados** para lograr efectos combinados. Los modificadores se aplican en el orden en que se encadenan, por ejemplo:

```kotlin
Modifier
    .fillMaxWidth()
    .padding(16.dp)
    .background(Color.Cyan)
    .clickable { /* Acción cuando se haga clic */ }
    .border(2.dp, Color.Black)
```

Este modificador haría que el componente ocupe todo el ancho disponible, tenga un padding de 16 dp, un fondo cian, sea clickeable y tenga un borde negro.

El **orden de los modificadores** es crucial porque cada modificador se aplica de manera secuencial, y el resultado de un modificador puede influir en cómo se comportan los modificadores que vienen después.

