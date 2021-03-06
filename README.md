# Instalación de Geoserver

El objetivo de esta guía es instalar un GEOSERVER para producción y que se comporte lo más estable posible.

## Primeros pasos
Para obtener el archivo `.war`, ingresaremos a la siguiente dirección : 
   
   `http://geoserver.org/download/`  

#### Instalación del Geoserver
 Ahora procedemos a copiar el archivo `.war` al folder **webapps** dentro de **Tomcat** (tomcat/webapss)  
  ```
  sudo cp  geoserver.war root/tomcat/webapps/
  ```

#### Instalación nativa de JAI y JAI Image I/O
   **JAI**
   * Descargamos [JAI 1.1.3][1] para linux amd64
   * Hacemos los sigueinte pasos :
     ```
     export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_101  
     sudo cp ROOT/jai-1_1_3/lib/*.jar $JAVA_HOME/jre/lib/ext/
     sudo cp ROOT/jai-1_1_3/lib/*.so $JAVA_HOME/jre/lib/amd64/     
     ```
     
   **JAI-IMAGEIO**
   * Descargamos [JAI-IMAGEIO][2] para linux amd64
   * Hacemos los siguientes pasos :  
     ```
     sudo cp ROOT/jai_imageio-1_1/lib/*.jar $JAVA_HOME/jre/lib/ext/  
     sudo cp ROOT/jai-imageio-1_1/lib/*.so $JAVA_HOME/jre/lib/amd64/     
     ```
   
   Al final reiniciamos el servidor Tomcat
   ```
    sudo service tomcat stop
    sudo service tomcat start
   ```
   
  **NOTA :** GeoSolutions nos ofrece una guía para poder hacer la instalación. [link][3]
  
### Requerimientos Técnicos
   * Sistemas operativo Ubuntu
   * Servidor Tomcat  
   * GeoServer

### Configuración del GEOSERVER  

#### Web.xml
Editamos el archivo web.xml para agregar las configuraciones que listamos en seguida:
   * Cambiar el directorio 'data' que es en donde se almacena la información, lo hacemos por medio de la variable 
     *GEOSERVER_DATA_DIRECTORY*.
     ```
     <context-param>
       <param-name>GEOSERVER_DATA_DIR</param-name>
       <param-value>root/Geoserver/data</param-value>
     </context-param>
     ```
   * Habilitamos el formato de salida JSONP (text/javascript), por medio de la variable *ENABLE_JSONP*
     ```
     <context-param>
       <param-name>ENABLE_JSONP</param-name>
       <param-value>true</param-value>
     </context-param>
     ```

#### Por medio de la interfaz Web 
Con el GEOSERVER ya instalado sobre tomcat, por medio de un explorador web ingresamos la url `http://localhost:8080/geoserver/web` :  
   
   * Nos autenticamos por default con ***username*** : **admin** y **password** : **geoserver**  
   * Por seguridad creamos un nuevo usuario  en  
     *Security* -> *Users,Groups, and Roles* -> *Add new user* y asignamos los roles de ADMIN y GROUP_ADMIN.
   * Borramos las group-layers, layers, stores, styles y workspaces que vienen por default en el Geoserver en este orden.
   * Habilitamos el *GeoWebCache* :  
     *Tile Caching* -> *Caching Defaults* -> *Provided Services*  
     Y marcamos las casillas de *Enable direct integration with GeoServer WMS*, *Enable WMS-C Service* y *Enable TMS Service*
   * Cambiamos las opciones del **log** a **PRODUCTION**  
     En *Settings* -> *Global* -> *Internal Settings* -> *Logging Settings* y seleccionamos **PRODUCTION_LOGGING.properties**
   * Cambiamos el numero máximo de "features" devueltas por WFS a 1000
     *Services* -> *WFS* -> *Features* -> *Maximum number of features*

 ### Optimizaciones
 
   * Seleccionamos el conjunto de caracteres **ISO-8859-1** para acentos :  
     *Settings* -> *Global* -> *Service Response Settings* -> *Character Set* -> *ISO-8859-1*
   * Limitamos a **4** el número de decimales : *Settings* -> *Global* -> *Service Response Settings* -> *Number of decimals*  
   * Deshabilitamos los mensajes detallados: *Settings* -> *Global* -> *Service Response Settings*   
   * Omitimos las capas mal configuradas: *Settings* -> *Global* -> *Service Error Settings* -> *Skipping misconfigured layers*  
   * Cambiamos la localización del archivo **log** : *Settings* -> *Global* -> *Internal Settings* -> *Logging Settings* -> *Log location*  
   * Modificamos el tamaño del cache a **200** para el tipo de característica : 
        *Settings* -> *Global* -> *Internal Settings* -> *Catalog Settings* -> *Feature type cache size*  
   * Habilitamos *Tile Recycling* : *Settings* -> *Image Processing* -> *Memory Use* -> *Tile Recycling*  

 
## Construido con
* [Geoserver][4] GeoServer
* [Tomcat][5] Servidor de aplicaciones

## Versionando  
Usamos [SemVer][6] para versionar. Para las versiones disponibles, consulte [las etiquetas en este repositorio][7].

## Autores
* **Raúl Medina Peña** - [Github][8]

## Licencia
Este proyecto está licenciado bajo la licencia MIT; consulte el archivo [LICENSE](LICENSE) para obtener detalles.

## Agradecimientos  
* Agradecemos a [Olmo Zavala][9] por la creación de la primera versión, colaboración y su apoyo en este proyecto.

[1]: http://download.java.net/media/jai/builds/release/1_1_3/jai-1_1_3-lib-linux-amd64.tar.gz
[2]: http://download.java.net/media/jai-imageio/builds/release/1.1/jai_imageio-1_1-lib-linux-amd64.tar.gz
[3]: https://geoserver.geo-solutions.it/edu/en/install_run/jai_io_install.html
[4]: https://docs.geoserver.org/
[5]: http://tomcat.apache.org/
[6]: https://semver.org/lang/es/
[7]: https://github.com/grupoioa/geoserver_instalacion/tags
[8]: https://github.com/rmedina09
[9]: https://github.com/olmozavala

