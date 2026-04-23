¡Hola! Como desarrollador senior, he diseñado este plan de trabajo estructurado para que tus estudiantes no solo copien código, sino que entiendan la arquitectura moderna en Flutter. 

Utilizaremos una estética **"Dark Mode Premium"** con los colores que solicitaste y una estructura profesional.

---

## 1. Configuración del Entorno y Firebase
Antes de tocar el código, debemos preparar el terreno en la terminal y en la consola de Google.

* **Estructura de directorios:**
    1.  Crea la carpeta raíz: `mkdir xflutterRoldan0679`
    2.  Entra en ella: `cd xflutterRoldan0679`
    3.  Crea el proyecto Flutter: `flutter create crudlecturas`
* **Consola Firebase:**
    1.  Ve a [Firebase Console](https://console.firebase.google.com/).
    2.  Crea el proyecto **crudLecturas**.
    3.  Habilita **Cloud Firestore** en modo prueba.
    4.  Crea una colección llamada `categorias` y añade un documento de prueba con los campos: `categoria` (String), `nombre` (String), `stock` (Number).

---

## 2. Dependencias y `pubspec.yaml`
Para integrar Firebase y las librerías necesarias (incluyendo el soporte para el concepto de "Antigravity" o fluidez en la UI), modifica tu archivo `pubspec.yaml`:

```yaml
dependencies:
  flutter:
    sdk: flutter
  firebase_core: ^3.0.0      # Núcleo de Firebase
  cloud_firestore: ^5.0.0    # Base de datos NoSQL
  google_fonts: ^6.0.0       # Estética visual
```

**Instalación:** Ejecuta `flutter pub get` en tu terminal.

---

## 3. Metodología de Agentes y Flujo de Trabajo
Para que los estudiantes comprendan el desarrollo, dividiremos el proyecto en **Roles y Skills**:

| Agente | Rol | Skill (Habilidad) |
| :--- | :--- | :--- |
| **Arquitecto** | Definir Estructura | Organización de carpetas y Clean Code. |
| **Data Master** | Gestión Firestore | Conexión CRUD y mapeo de datos. |
| **Diseñador UI** | Antigravity Experience | Creación de widgets con el estilo Morado/Negro. |

### Estructura de Carpetas (Clean Architecture):
```text
lib/
├── config/
│   └── theme.dart        # Colores y estilos
├── models/
│   └── categoria_model.dart
├── services/
│   └── firebase_service.dart
└── screens/
    └── home_screen.dart   # UI del CRUD
```

---

## 4. Implementación del Código (Funcional)

### A. Definición de Colores (Theme)
En `lib/config/theme.dart`, definimos la paleta solicitada:

```dart
import 'package:flutter/material.dart';

class AppColors {
  static const Color fondoNegro = Color(0xFF000000);
  static const Color moradoClaro = Color(0xFFE1BEE7); // Fondo recuadro
  static const Color moradoObscuro = Color(0xFF4A148C); // Contorno
  static const Color sombraBlanca = Color(0xFFFFFFFF);
}
```

### B. Modelo de Datos
`lib/models/categoria_model.dart`
```dart
class Categoria {
  String id;
  String categoria;
  String nombre;
  int stock;

  Categoria({required this.id, required this.categoria, required this.nombre, required this.stock});

  Map<String, dynamic> toMap() => {
    "categoria": categoria,
    "nombre": nombre,
    "stock": stock,
  };
}
```

### C. Servicio CRUD (Firebase)
`lib/services/firebase_service.dart`
```dart
import 'cloud_firestore/cloud_firestore.dart';

FirebaseFirestore db = FirebaseFirestore.instance;

// LEER
Future<List> getCategorias() async {
  List categorias = [];
  QuerySnapshot querySnapshot = await db.collection('categorias').get();
  for (var doc in querySnapshot.docs) {
    final Map<String, dynamic> data = doc.data() as Map<String, dynamic>;
    categorias.add({...data, 'id': doc.id});
  }
  return categorias;
}

// CREAR
Future<void> addCategoria(String cat, String nom, int stock) async {
  await db.collection('categorias').add({'categoria': cat, 'nombre': nom, 'stock': stock});
}

// ACTUALIZAR
Future<void> updateCategoria(String id, String newNom) async {
  await db.collection('categorias').doc(id).update({'nombre': newNom});
}

// BORRAR
Future<void> deleteCategoria(String id) async {
  await db.collection('categorias').doc(id).delete();
}
```

---

## 5. UI con Estética Antigravity
Este widget genera los recuadros morados con el sombreado blanco sobre fondo negro.

`lib/screens/home_screen.dart`
```dart
import 'package:flutter/material.dart';
import '../config/theme.dart';
import '../services/firebase_service.dart';

class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: AppColors.fondoNegro,
      appBar: AppBar(title: Text("CRUD Lecturas"), backgroundColor: AppColors.moradoObscuro),
      body: FutureBuilder(
        future: getCategorias(),
        builder: (context, snapshot) {
          if (!snapshot.hasData) return Center(child: CircularProgressIndicator());
          return ListView.builder(
            itemCount: snapshot.data?.length,
            itemBuilder: (context, index) {
              return Container(
                margin: EdgeInsets.all(12),
                decoration: BoxDecoration(
                  color: AppColors.moradoClaro,
                  border: Border.all(color: AppColors.moradoObscuro, width: 2),
                  borderRadius: BorderRadius.circular(15),
                  boxShadow: [BoxShadow(color: AppColors.sombraBlanca, blurRadius: 10)],
                ),
                child: ListTile(
                  title: Text(snapshot.data![index]['nombre'], 
                    style: TextStyle(fontWeight: Bold, color: AppColors.moradoObscuro)),
                  subtitle: Text("Categoría: ${snapshot.data![index]['categoria']} | Stock: ${snapshot.data![index]['stock']}"),
                  trailing: IconButton(
                    icon: Icon(Icons.delete, color: Colors.red),
                    onPressed: () async {
                      await deleteCategoria(snapshot.data![index]['id']);
                      // Aquí se refrescaría el estado
                    },
                  ),
                ),
              );
            },
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        backgroundColor: AppColors.moradoObscuro,
        child: Icon(Icons.add, color: Colors.white),
        onPressed: () => addCategoria("Terror", "Drácula", 10), // Ejemplo rápido
      ),
    );
  }
}
```

---

## 6. Práctica Guiada: Concepto Antigravity
Para los estudiantes, **Antigravity** en este proyecto se refiere a la **sensación de flotabilidad** de los elementos.
1.  **El Vacío:** El fondo negro representa el espacio.
2.  **La Masa:** Los recuadros morados son los objetos.
3.  **La Fuerza (Sombra):** El `boxShadow` blanco simula que el objeto emite luz o flota sobre el fondo oscuro, rompiendo la bidimensionalidad.

**Instrucción para el alumno:** "Modifica el `blurRadius` de la sombra blanca en el código. Observa cómo, al aumentarlo, el libro parece alejarse del fondo negro, creando el efecto Antigravity."
