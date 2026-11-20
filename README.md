# Novedades de ROB RUN para Pacto Gamer

Flujo de GitHub Actions que comprueba Steam y el Telegram público oficial de ROB RUN cada hora. Cuando encuentra una novedad, prepara una ficha en español y una tarjeta PNG vertical de 1080 × 1350 con el emblema de Pacto Gamer. Envía ambas en un solo mensaje de Discord y guarda el identificador de recepción en `state.json`. La fecha del aviso se muestra en hora de Nueva York (ET). La fuente aparece en la imagen. La consulta automática cubre Steam y Telegram.

## Activación

1. Crea un repositorio **privado** de GitHub y sube el contenido de esta carpeta a su rama principal. Incluye la carpeta oculta `.github`. Puedes hacerlo desde GitHub Desktop copiando los archivos a la carpeta del repositorio y usando **Commit** y **Push origin**.
2. En el repositorio, entra en **Settings → Secrets and variables → Actions → Secrets**. Crea `DISCORD_WEBHOOK_URL` con la URL de un webhook del canal elegido y `OPENAI_API_KEY` con una clave de la API de OpenAI. La API se configura por separado de ChatGPT. Si la URL anterior se compartió por chat, crea un webhook nuevo y elimina el anterior. Guarda el reemplazo directamente en GitHub Secrets.
3. En **Actions → Novedades de ROB RUN → Run workflow**, ejecuta **inspect** para verificar el canal del webhook. Comprueba el ID que figura en el registro. Esta comprobación solo lee los datos del webhook.
4. Ejecuta **preview** para generar una ficha real con un aviso reciente. Descarga `aviso-pacto-gamer` desde **Artifacts** para revisar la imagen y el texto. Esta opción no escribe en Discord ni cambia el registro.
5. En **Settings → Secrets and variables → Actions → Variables**, crea `ROBRUN_PUBLICAR` con valor `si`. Ejecuta **live** una vez para iniciar. La primera ejecución en vivo registra los avisos que ya existen. Desde la siguiente revisión, los anuncios nuevos podrán publicarse. Las revisiones se programan para el minuto 36 de cada hora. GitHub puede retrasar una ejecución.

La rama principal debe permitir al flujo guardar `state.json` con el permiso `contents: write`. La variable `ROBRUN_PUBLICAR` se puede quitar para pausar los envíos. La tarea existente de ChatGPT puede seguir sirviendo como monitor aparte.

## Si una entrega queda sin confirmar

Busca el aviso en Discord y abre `state.json`. Si está publicado, cambia su `status` de `sending` a `sent` y guarda el ID del mensaje en `discord_message_id`. Si comprobaste que el aviso no llegó, elimina su entrada de `posts`; el flujo volverá a intentarlo si el aviso sigue en las fuentes. Esto evita duplicar un envío cuando Discord recibe el mensaje pero se pierde la confirmación.

- [Webhooks de Discord](https://support.discord.com/hc/en-us/articles/228383668-Intro-to-Webhooks)
- [Secretos en GitHub Actions](https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-secrets)
- [Configurar la API de OpenAI](https://developers.openai.com/api/docs/quickstart)
