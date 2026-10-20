# Evolución de las listas en Android
Una de las formas que tiene Android para mostrar colecciones de datos es el uso de las estructuras:

- ListView (XML)
- RecyclerView (XML)
- LazyColumn. (Compose)

## ListView y ArrayAdapter
### Estado actual
LEGACY

Es importante conocerlo porque aparecen en aplicaciones antiguas. Sin embargo, actualmente apenas se utiliza en proyectos nuevos.

### ¿Qué es un ArrayAdapter?
Un adaptador actúa como puente entre:
- Los datos.
- El componente visual.

Ejemplo:

```kotlin
val ciudades = arrayOf( "Madrid", "Toledo", "Sevilla" )
val adapter = ArrayAdapter(this, android.R.layout.simple_list_item_1, ciudades )
```

## RecyclerView
### Estado actual
USADO

RecyclerView sigue apareciendo en multitud de aplicaciones profesionales.

### ¿Qué es RecyclerView?
RecyclerView mejora el rendimiento reutilizando vistas. En lugar de crear continuamente nuevos elementos:

- Los recicla.
- Los reutiliza.
- Consume menos memoria.

### Elementos principales
#### RecyclerView
Zona donde se muestran los datos.

#### Adapter
Conecta datos e interfaz.

#### ViewHolder
Representa una fila individual.

#### Dataset
Colección de información.

## LazyColumn (Jetpack Compose)
Es la alternativa moderna a RecyclerView y permite mostrar colecciones de datos utilizando Compose.

Ejemplo:

```kotlin
LazyColumn {
    items(alumnos) { alumno ->
        Text(alumno.nombre)
    }
}
```

### Ventajas
- Menos código.
- No necesita Adapter.
- No necesita ViewHolder.
- Más fácil de mantener.

### Equivalencia conceptual
RecyclerView necesita:

- Adapter
- ViewHolder
- Item XML

LazyColumn necesita:

- Una lista.
- Un composable.
