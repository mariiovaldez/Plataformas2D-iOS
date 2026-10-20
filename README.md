# Plataformas2D — Build iOS

Este repositorio **solo existe para compilar el juego a `.ipa` sin usar un Mac**.

El proyecto de Godot vive en el PC. Lo que se sube aquí es el proyecto de Xcode que
Godot 3.4 genera al exportar, y GitHub Actions lo compila en un runner de macOS.

## Flujo completo

```
PC (Windows)                     GitHub (nube)                  PC (Windows)              iPhone
--------------------------------------------------------------------------------------------
Godot 3.4  --export iOS-->  git push -->  xcodebuild  -->  descarga .ipa  -->  AltStore  -->  app
                                                       (Actions, macOS)    iTunes + Apple ID
```

## Cómo actualizar el juego

1. En Godot: `Project > Export > iOS > Export Project` (o ejecutar el export por consola).
2. Copiar la carpeta generada a este repositorio.
3. `git add -A && git commit -m "update" && git push`
4. Esperar a que termine la ejecución de Actions.
5. Descargar el `.ipa` desde la pestaña **Actions > Build iOS (.ipa) > Artifacts**.

## Cómo instalarlo en el iPhone

El `.ipa` sale **sin firmar**, así que hay que firmarlo con tu Apple ID en el momento
de instalarlo. Herramientas que hacen eso en Windows:

- **AltStore** — https://altstore.io  (requiere iTunes instalado)
- **Sideloadly** — https://sideloadly.io

Ambas necesitan: iTunes de escritorio (no el de Microsoft Store), el `.ipa`, el
celular por cable y tu Apple ID.

> **Cuenta Apple gratuita:** la app caduca a los **7 días**. Hay que volver a
> instalarla desde la herramienta de sideload. Con el programa Developer ($99/año)
> dura 1 año.

## Notas técnicas

- Godot 3.4 en Windows **sí** exporta el proyecto de Xcode; solo no arma el `.ipa`
  (eso es lo que hace el runner de macOS aquí).
- `IPHONEOS_DEPLOYMENT_TARGET` se subió de 10.0 a 14.0 porque los Xcode actuales ya
  no aceptan objetivos tan viejos.
- El build corre con `CODE_SIGNING_ALLOWED=NO` a propósito: la firma la hace
  AltStore/Sideloadly con tu Apple ID.
- Repo **público** = minutos de macOS ilimitados. Repo **privado** se cobra de los
  200 min/mes gratis (10x por minuto en macOS), suficiente para muy pocas corridas.
