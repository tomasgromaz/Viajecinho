# Viajecinho

App del viaje a Río de Janeiro: gastos compartidos, plan por día, rosco y trivia, y frasario en portugués con audio.

## Subirla a GitHub Pages

1. Subí `index.html` a un repositorio (en la raíz).
2. En el repositorio: Settings > Pages > Deploy from a branch > `main` / `(root)`.
3. Abrí la dirección que te da GitHub Pages.

Sin hacer nada más, la app funciona y guarda los datos en el propio celular.

## Sincronizar entre celulares con Firebase (gratis)

Es opcional. Usa Firestore, en el plan Spark: no hace falta tarjeta. Los nombres de los menús pueden variar un poco.

1. Entrá a https://console.firebase.google.com con tu cuenta de Google y creá un proyecto (por ejemplo `viajecinho`). Podés desactivar Google Analytics.
2. En el menú Compilación (Build) elegí Firestore Database > Crear base de datos. Si te pregunta la edición, elegí Standard. Ubicación: `southamerica-east1` (São Paulo). Modo producción.
3. Pestaña Reglas: reemplazá todo por lo siguiente y tocá Publicar.

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /trips/{code}/{document=**} {
      allow read, write: if code.size() >= 10;
    }
  }
}
```

4. Configuración del proyecto (engranaje) > Tus apps > ícono web `</>`. Registrá la app (sin Firebase Hosting) y copiá el objeto `firebaseConfig`.
5. En `index.html` buscá `const FIREBASE_CONFIG = null;` y reemplazá `null` por ese objeto, con las llaves `{ }`. Subí el archivo de nuevo a GitHub.
6. Abrí la app > Ajustes > Sincronizar con el grupo > Crear grupo. Se copia un enlace: mandáselo por WhatsApp a Gero y Nacho. Al abrirlo quedan conectados al mismo grupo.

Si ya habías cargado datos en tu celular antes de crear el grupo, se suben solos al crearlo.

## Cosas para saber

- La configuración web de Firebase no es secreta. La protección es el código del grupo, que va dentro del enlace: quien tenga el enlace puede ver y editar los datos del viaje. No lo publiques.
- Los datos quedan en Firestore, en `trips/<código>`.
- Cuota gratis de Firestore: 1 GiB, 50.000 lecturas y 20.000 escrituras por día. Si se supera una cuota, ese producto se bloquea hasta el mes siguiente. No vincules una cuenta de facturación: eso pasa el proyecto al plan pago.
- Para sincronizar hace falta conexión. Lo que se carga sin conexión se guarda en el celular y se sube cuando vuelve.
- Mientras no esté conectado, cada celular guarda sus propios datos. En Ajustes hay un respaldo para copiarlos y pasarlos a otro celular.
- La cotización de Ajustes es la general. En cada gasto pagado con Pix o tarjeta se puede cargar cuántos pesos se descontaron, y ese gasto usa el valor exacto.
