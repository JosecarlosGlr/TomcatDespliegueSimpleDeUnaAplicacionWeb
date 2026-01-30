# Tomcat: Despliegue simple de una aplicacion web

## 1. Obtención y Despliegue del Artefacto
El objetivo es verificar el motor de despliegue automático de Tomcat. Para ello, se ha utilizado un archivo de aplicación web empaquetado (`.war`) llamado `examples`.

**Procedimiento realizado:**
1.  Se descargó el archivo `examples.war`.
2.  Se copió dicho archivo dentro del directorio de despliegue predeterminado: `/opt/tomcat/apache-tomcat-11.0.14/webapps`.

![Transferencia del archivo WAR](https://raw.githubusercontent.com/JosecarlosGlr/TomcatDespliegueSimpleDeUnaAplicacionWeb/refs/heads/main/1.png)

---

## 2. Análisis del Despliegue Automático

Al copiar el archivo en la carpeta `webapps`, Tomcat detecta el nuevo elemento y procede a su instalación sin necesidad de reiniciar el servicio.

**Evidencia en el sistema de archivos:**
En la siguiente captura se observa cómo, tras copiar el `.war`, Tomcat ha creado automáticamente una carpeta con el mismo nombre (`examples`), que contiene la aplicación descomprimida y lista para ejecutarse.

![Descompresión automática del WAR](https://raw.githubusercontent.com/JosecarlosGlr/TomcatDespliegueSimpleDeUnaAplicacionWeb/refs/heads/main/2.png)

### ⚙️ ¿Qué ocurre internamente en Tomcat?
El proceso técnico que realiza el servidor se denomina **Hot Deployment** y sigue estos pasos:

1.  **Polling (Sondeo):** Un hilo del servidor (configurado en `HostConfig`) monitoriza periódicamente la carpeta `webapps` buscando cambios.
2.  **Detección:** Al encontrar `examples.war`, verifica la configuración `autoDeploy="true"` y `unpackWARs="true"` en el archivo `server.xml`.
3.  **Expansión:** Descomprime el archivo `.war` en un directorio físico equivalente.
4.  **Creación del Contexto:** Lee el descriptor de despliegue (`WEB-INF/web.xml`), crea el contexto en memoria y arranca la aplicación.
5.  **Disponibilidad:** La aplicación queda accesible inmediatamente bajo la ruta de contexto `http://localhost:8080/examples`.

---

## 3. Verificación de Funcionamiento

Se accede a la aplicación a través del navegador para confirmar que el despliegue ha sido exitoso.

**Resultado:** La aplicación carga correctamente, mostrando la página de índice y los servlets de ejemplo.

![Acceso vía navegador](https://raw.githubusercontent.com/JosecarlosGlr/TomcatDespliegueSimpleDeUnaAplicacionWeb/refs/heads/main/3.png)

---

## 4. Prueba de Redespliegue (Cambio de Contexto)
Para verificar la dinamicidad del servidor, se modificó el nombre del archivo WAR original.

![Cambio de nombre del WAR](https://raw.githubusercontent.com/JosecarlosGlr/TomcatDespliegueSimpleDeUnaAplicacionWeb/refs/heads/main/4.png)

**Comportamiento observado:**
Al renombrar el archivo, Tomcat interpreta esto como el despliegue de una **nueva aplicación**. Si el archivo pasa a llamarse `prueba.war`, la aplicación será accesible inmediatamente bajo la URL `/prueba`, manteniendo la original `/examples` operativa o eliminándola según si se borró el archivo original.
