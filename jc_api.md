**Лекция: Работа с Web API на Kotlin в Jetpack Compose**

### Введение

Web API играет важную роль в современных мобильных приложениях, позволяя получать данные с серверов и взаимодействовать с удалёнными ресурсами. В языке программирования Kotlin для работы с Web API используется библиотека Retrofit, а для асинхронной обработки данных - Kotlin Coroutines. В этой лекции мы рассмотрим, как настроить запросы к API в Jetpack Compose.

### 1. Настройка проекта

Для работы с Web API нам потребуется добавить зависимости в `build.gradle.kts`:

```kotlin
dependencies {
    implementation("com.squareup.retrofit2:retrofit:2.9.0")
    implementation("com.squareup.retrofit2:converter-gson:2.5.0")
    implementation("androidx.lifecycle:lifecycle-viewmodel-compose:2.7.0")
}
```

### 2. Создание API-интерфейса

Определим интерфейс для работы с REST API, используя аннотации Retrofit:

```kotlin
import retrofit2.http.GET
import retrofit2.Call

interface ApiService {
    @GET("posts")
    suspend fun getPosts(): List<Post> //определяем функцию получения данных из апи

    @POST("posts")
    suspend fun createPost(@Body post: Post): Post //определяем функцию создания данных в апи

    @PUT("posts/{id}")
    suspend fun updatePost(@Path("id") id: Int, @Body post: Post): Post //определяем функцию обновления данных в апи

    @DELETE("posts/{id}")
    suspend fun deletePost(@Path("id") id: Int) //определяем функцию удаления данных в апи
}
```

### 3. Создание модели данных

Создадим модель данных для представления JSON-объектов:

```kotlin
data class Post(
    val userId: Int,
    val id: Int,
    val title: String,
    val body: String
)
```

### 4. Настройка Retrofit

Создадим объект для работы с API:

```kotlin
import retrofit2.Retrofit
import retrofit2.converter.gson.GsonConverterFactory

object RetrofitInstance {
    private const val BASE_URL = "https://jsonplaceholder.typicode.com/"

    val api: ApiService by lazy {
        Retrofit.Builder()
            .baseUrl(BASE_URL)                                    //базовый адрес
            .addConverterFactory(GsonConverterFactory.create())   //автоматическое преобразование JSON в объекты классов
            .build()
            .create(ApiService::class.java)
    }
}
```

### 5. Запрос данных в ViewModel

Jetpack Compose работает с `ViewModel` для хранения состояния. Определим `MainViewModel`, использующий `RetrofitInstance`:

```kotlin
import androidx.lifecycle.ViewModel
import androidx.lifecycle.viewModelScope
import kotlinx.coroutines.launch
import kotlinx.coroutines.flow.MutableStateFlow
import kotlinx.coroutines.flow.StateFlow

class MainViewModel : ViewModel() {
    private val _posts = MutableStateFlow<List<Post>>(emptyList())      //поток для хранения постов
    val posts: StateFlow<List<Post>> = _posts

    init {
        fetchPosts()
    }

    private fun fetchPosts() {
        viewModelScope.launch {
            try {
                _posts.value = RetrofitInstance.api.getPosts()          //асинхронно получаем данные
            } catch (e: Exception) {
                e.printStackTrace()
            }
        }
    }

    fun addPost(post: Post) {
        viewModelScope.launch {
            try {
                RetrofitInstance.api.createPost(post)          //асинхронно создадаем пост
                fetchPosts()
            } catch (e: Exception) {
                e.printStackTrace()
            }
        }
    }

    fun updatePost(id: Int, post: Post) {
        viewModelScope.launch {
            try {
                RetrofitInstance.api.updatePost(id, post)       //асинхронно обновим пост
                fetchPosts()
            } catch (e: Exception) {
                e.printStackTrace()
            }
        }
    }

    fun deletePost(id: Int) {
        viewModelScope.launch {
            try {
                RetrofitInstance.api.deletePost(id)          //асинхронно удалим пост
                fetchPosts()
            } catch (e: Exception) {
                e.printStackTrace()
            }
        }
    }
}
```

### 6. Отображение данных в Jetpack Compose

Используем `LazyColumn` для отображения списка постов:

```kotlin
import androidx.compose.foundation.layout.padding
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.foundation.lazy.items
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp
import androidx.lifecycle.viewmodel.compose.viewModel

@Composable
fun PostListScreen(viewModel: MainViewModel = viewModel()) {
    val posts by viewModel.posts.collectAsState()

    LazyColumn(modifier = Modifier.padding(16.dp)) {
        items(posts) { post ->
            Card(modifier = Modifier.padding(vertical = 8.dp)) {
                Text(text = post.title, modifier = Modifier.padding(16.dp))
            }
        }
    }
}
```

### 7. Интеграция в `MainActivity`

Определим `setContent` с `PostListScreen`:

```kotlin
import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Surface

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                Surface {
                    PostListScreen()
                }
            }
        }
    }
}
```

### 8. Разрешения

Для предоставления приложению доступа в интернет необходимо добавить в файл манифеста строку перед тегом `<application>`
```xml
<uses-permission android:name="android.permission.INTERNET" />
```


### Заключение

Протестируйте работу приложения с Api, попробуйте получить другие данные при помощи апи с сайта https://jsonplaceholder.typicode.com/

Для быстрого преобразование файлов JSON в классы Котлин можно использовать сайт https://json2kt.com/