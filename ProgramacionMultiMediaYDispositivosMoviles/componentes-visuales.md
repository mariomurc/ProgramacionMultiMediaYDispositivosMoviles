# Componentes visuales: XML vs Jetpack Compose
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

# Equivalencias XML y Compose
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
