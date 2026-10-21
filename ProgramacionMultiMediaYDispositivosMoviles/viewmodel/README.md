# ViewModel

El `ViewModel` es un componente esencial en la arquitectura de Android, particularmente en Jetpack Compose, ya que permite separar la lógica de negocio y los datos del ciclo de vida de la interfaz gráfica.

Los `ViewModel` están diseñados para:

* Almacenar y gestionar datos de estado relacionados con la UI.
* Sobrevivir a cambios de configuración como rotación de pantalla.
* Facilitar la conexión entre la UI y la lógica de negocio.

En Jetpack Compose, un `ViewModel` permite emitir estados observables hacia las composables de forma reactiva.

### **Configuración**

Incluye la siguiente dependencia en tu archivo `build.gradle`:

```groovy
implementation("androidx.lifecycle:lifecycle-viewmodel-compose:2.8.7")
```

### Arquitectura CLEAN

Si seguimos la arquitectura CLEAN, los `ViewModel` deben declararse en el paquete presentation.viewmodel

```
com.example.myapp
│
├── data
│   ├── model
│   ├── repository
│   ├── source
│   │   ├── local
│   │   └── remote
│   └── mapper
│
├── domain
│   ├── model
│   ├── repository
│   └── usecase
│
├── presentation
│   ├── ui
│   │   ├── components
│   │   ├── screens
│   │   │   ├── home
│   │   │   ├── profile
│   │   │   └── settings
│   ├── navigation
│   └── viewmodel
│
├── di
│
└── utils
```

## Crear un ViewModel

Un `ViewModel` se utiliza para manejar algún estado.  Todo `ViewModel` debe declarar las siguientes partes:

1. **Variable de estado mutable** (`MutableStateFlow`): define el estado que gestiona el `ViewModel`. Este estado es mutable y debe declararse privado para que los `@Composable` no puedan acceder al mismo directamente.
2. **Variable de estado inmutable** (`StateFlow`): Es una derivación inmutable del estado anterior, este es público ya que&#x20;
3. Funciones de acciones del estado

El `ViewModel` de cada ventana se declara como parámetro de su función `@Composable` y se instancia por defecto usando el método `viewModel()`.

## Ejemplo de Login Screen

Por ejemplo, podríamos modificar la ventana de Login para que use un `ViewModel`

### LoginScreenViewModel

```kotlin
package org.ies.navigation.presentation.viewmodel

import androidx.lifecycle.ViewModel
import kotlinx.coroutines.flow.MutableStateFlow
import kotlinx.coroutines.flow.StateFlow

class LoginScreenViewModel : ViewModel() {
    // var username by remember{ mutableStateOf("") }
    private val _username = MutableStateFlow("")
    val username: StateFlow<String> = _username

    private val _password = MutableStateFlow("")
    val password: StateFlow<String> = _password


    fun setUsername(username: String) {
        _username.value = username
    }

    fun setPassword(password: String) {
        _password.value = password
    }

    fun clear() {
        _username.value = ""
        _password.value = ""
    }

}
```

### LoginScreen

```kotlin
package org.ies.navigation.presentation.ui.screens

import androidx.compose.foundation.layout.Arrangement
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.Spacer
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.height
import androidx.compose.foundation.layout.padding
import androidx.compose.material3.Button
import androidx.compose.material3.Scaffold
import androidx.compose.material3.Text
import androidx.compose.material3.TextField
import androidx.compose.runtime.Composable
import androidx.compose.runtime.collectAsState
import androidx.compose.runtime.derivedStateOf
import androidx.compose.runtime.getValue
import androidx.compose.runtime.remember
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import androidx.lifecycle.viewmodel.compose.viewModel
import org.ies.navigation.presentation.viewmodel.LoginScreenViewModel

@Composable
fun LoginScreen(loginScreenViewModel: LoginScreenViewModel = viewModel()) {
    val username by loginScreenViewModel.username.collectAsState()
    val password by loginScreenViewModel.password.collectAsState()
    val loginEnabled by remember {
        derivedStateOf {
            username.isNotBlank() && password.isNotBlank()
        }
    }
    Scaffold { innerPadding ->
        Column(
            verticalArrangement = Arrangement.Center,
            horizontalAlignment = Alignment.CenterHorizontally,
            modifier = Modifier
                .padding(innerPadding)
                .fillMaxSize()
        ) {
            TextField(
                modifier = Modifier
                    .fillMaxWidth(),
                value = username,
                onValueChange = { loginScreenViewModel.setUsername(it) },
                label = {
                    Text("Username")
                }
            )
            Spacer(modifier = Modifier.height(16.dp))

            TextField(
                modifier = Modifier
                    .fillMaxWidth(),
                value = password,
                onValueChange = { loginScreenViewModel.setPassword(it) },
                label = {
                    Text("Password")
                }
            )
            Spacer(modifier = Modifier.height(16.dp))
            Button(
                enabled = loginEnabled,
                onClick = {
                    loginScreenViewModel.clear()
                }
            ) {
                Text("Login")
            }
        }

    }
}

@Preview(showBackground = true)
@Composable
fun LoginScreenPreview() {
    LoginScreen()
}
```
