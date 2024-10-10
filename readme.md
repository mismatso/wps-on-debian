# WPS Office en GNU/Linux Debian

## ¿Cómo instalar WPS en GNU/Linux Debian?

Para descargar el instalador de *WPS Office* en *GNU/Linux Debian*, puede ejecutar el siguiente comando o visitar el sitio oficial [aquí](https://www.wps.com/download/) y descargar el paquete en formato _Deb Package_.

```bash
wget https://wdl1.pcfg.cache.wpscdn.com/wpsdl/wpsoffice/download/linux/11723/wps-office_11.1.0.11723.XA_amd64.deb
```

A continuación, instale WPS utilizando el paquete `.deb` descargado:

```bash
sudo dpkg -i wps-office_11.1.0.11723.XA_amd64.deb
```

Si durante la instalación encuentra problemas de dependencias, puede solucionarlos ejecutando el siguiente comando.

```bash
sudo apt --fix-broken install
```

## Corrector ortográfico en español

Para habilitar la corrección ortográfica en español en WPS, es necesario contar con un diccionario compatible con _Hunspell_. Para ello, puede utilizar los paquetes ya disponibles en Debian.

```bash
sudo apt-get install hunspell hunspell-es
```

Ahora, cree un directorio donde WPS pueda encontrar el diccionario en español.

```bash
sudo mkdir -p /opt/kingsoft/wps-office/office6/dicts/spellcheck/es_ES
```

A continuación, genere un archivo de configuración para el corrector ortográfico en español.

```bash
echo -e "[Dictionary]\nDisplayName=Español (España)\nDisplayName[en_US]=Spanish (Spain)" | sudo tee /opt/kingsoft/wps-office/office6/dicts/spellcheck/es_ES/dict.conf
```

Para que WPS utilice los diccionarios de _Hunspell_, cree enlaces simbólicos hacia los archivos correspondientes.

```bash
sudo ln -s /usr/share/hunspell/es_ES.aff /opt/kingsoft/wps-office/office6/dicts/spellcheck/es_ES/main.aff
sudo ln -s /usr/share/hunspell/es_ES.dic /opt/kingsoft/wps-office/office6/dicts/spellcheck/es_ES/main.dic
```

Reinicie WPS para habilitar el corrector ortográfico en español.

![Habilitar corrector ortográfico en español](/images/enable-spa-spell-checker.png)

## Corregir error al exportar a PDF

WPS requiere la librería _libtiff.so.5_; sin embargo, en Debian 12 (Bookworm) solo está disponible la versión 6 de esta librería. Esta incompatibilidad provoca el error _WPS Writer encountered an error while trying exporting to PDF_ al intentar exportar un archivo a PDF.

![Error al exportar a PDF](/images/error-exporting-to-pdf.png)

Para corregirlo, instale la librería _libtiff6_ y luego cree un enlace simbólico que haga referencia a la versión anterior:

```bash
sudo apt-get install libtiff6
```

Después de instalar _libtiff6_, cree el siguiente enlace simbólico para que WPS utilice esta versión en lugar de la anterior:

```bash
sudo ln -s /usr/lib/x86_64-linux-gnu/libtiff.so.6 /usr/lib/x86_64-linux-gnu/libtiff.so.5
```

## «Self-Promotion»

Si lo desea, puede visitar mi canal de YouTube [MizaqScreencasts](https://www.youtube.com/MizaqScreencasts) y seguirme en [Twitter](https://twitter.com/mismatso).

## Licencia ![by-nc-sa](https://licensebuttons.net/l/by-nc-sa/4.0/80x15.png)

Este manual está licenciado bajo una [Licencia Creative Commons Atribución/Reconocimiento-NoComercial-CompartirIgual 4.0 Internacional](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.es). Esto significa que usted es libre de:

- **Compartir** — copiar y redistribuir el material en cualquier medio o formato.
- **Adaptar** — remezclar, transformar y construir a partir del material, pero **no con fines comerciales**.

Bajo las siguientes condiciones:

- **Atribución** — debe otorgar el crédito adecuado, proporcionar un enlace a la licencia e indicar si se han realizado cambios. Puede hacerlo en cualquier forma razonable, pero no de manera que sugiera que usted o su uso tienen el apoyo de la licenciante.
- **NoComercial** — no puede utilizar el material con fines comerciales.
- **CompartirIgual** — si remezcla, transforma o crea a partir del material, debe distribuir su contribución bajo la la misma licencia del original.

---

[WPS Office en GNU/Linux Debian](https://github.com/mismatso/wps-on-debian) © 2024 by [Misael Matamoros](https://t.me/mismatso) está licenciado bajo [CC BY-NC-SA 4](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.es).