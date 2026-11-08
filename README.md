# Dolores Hoy 🌎

Tablero en Flutter que consume 5 APIs gratuitas (Open-Meteo, Frankfurter, Nager.Date,
Wikipedia y NASA APOD) con buenas prácticas de seguridad. Hecho con TDD.

**Materia:** Desarrollo Móvil Integral · UTNG
**Profesor:** Anastacio Rodríguez García
**Alumna:** Paola Michelle González Arredondo

## Cómo abrir el proyecto

Este .zip trae el código (`lib/`, `test/`, `pubspec.yaml`). Las carpetas de plataforma
(android, ios...) se generan con Flutter:

```bash
cd dolores_hoy
flutter create .          # genera android/, ios/, etc. sin tocar lib/ ni test/
flutter pub get
cp secrets.json.example secrets.json   # y pon tu clave de api.nasa.gov (o deja DEMO_KEY)
flutter test              # corre todas las pruebas
flutter run --dart-define-from-file=secrets.json
```

> `secrets.json` está en `.gitignore`: nunca se sube al repositorio.

## Estructura

```
lib/
  dominio/       excepciones.dart, modelos.dart
  datos/         cliente_seguro.dart, config_claves.dart, servicios.dart
  presentacion/  tablero_viewmodel.dart, pantalla_tablero.dart
  main.dart
test/
  ayudas.dart, cliente_seguro_test.dart, servicios_test.dart,
  seguridad_test.dart, tablero_viewmodel_test.dart
```

## Medidas de seguridad aplicadas

- HTTPS obligatorio, timeout de 8 s y límite de 1 MB por respuesta (`ClienteSeguro`)
- Claves fuera del código (`--dart-define`) y fuera de git (`.gitignore`)
- Validación de entradas (tema de Wikipedia, coordenadas, año) y de salidas (`soloHttps`, `media_type`)
- Direcciones con `Uri.https` / `pathSegments` (anti path injection)
- Mensajes de error genéricos para errores inesperados; logs con `redactar()`
- Una API caída no tumba a las otras cuatro
