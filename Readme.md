
//**********************************************************************
 SIMPLE   Server  (Postgis BASED)
//**********************************************************************

Simple Server es un modulo creado para desarrollar y probar rapidamente el desarrollo de servicios web.

Para que un servicio web accesible, se debe definir una URL o punto de entrada que reciba la peticion con parametros. Cada URL esta asociada a una funcion de javascript que implementa la funcionalidad deseada.

En general, cada funcion contiene una consulta SQL, que puede o no recibir parametros, ejecuta la instruccion y devuelve resultados. Es esperado, dentro del contexto geoespacial, utilizar formatos estandares para intercambiar informacion, por lo que se suguiere devolver archivos GeoJSON.


Ejemplo de USO:

http://localhost:8000/wifi_layer/getCoverage?lat=20.0&lon=100.0

Lo que llama a  la funcion -->

    function getCoverage(ctx){
        ...
        let geoJSONResult = {
        "type": "FeatureCollection",
        "totalFeatures": 0,  //este valor se debe actualizar
        "features": [],//aqui es donde se colocan los rasgos
        "crs": {
            "type": "name",
            "properties": {
                "name": "urn:ogc:def:crs:EPSG::4326"//habra que modificarlo conforme a las respuestas
            }
        }
    };

        'SELECT st_asgeojson(geom) json, * from wifi_layer WHERE ... ST_INTERSECTS( geom, st_geomfromtext('Point( lat, lon)'))'
    
        resultados.forEach(function (resultado_i) {
            //creamos una plantilla para cada rasgo
        let feature = {
            "type": "Feature",
            "id": resultado_i.id,
            "geometry": JSON.parse(resultado_i.json),
            "geometry_name": "geom",
            "properties": {
                "id": resultado_i.id,  //<= muevanle aqui
                "cve_muni": resultado_i.cvemuni, //<= aqui,
                "nom_mun": resultado_i.nom_mun,//<= aqui
                "layer": resultado_i.layer,//<= y  aqui
                //si faltan mas cosas, agregarlas
                "source": resultado_i.source//<= y  aqui

            }
        };
        geoJSONResult.features.push(feature);//agregamos el feature al arreglo numFeatures


        return geoJSONResult
    }


y devuelve un archivo geoJSON.





Dependencias:

nodejs
npm install pg-promise
npm install local-web-server//  v3.0
koa

INSTALACION

npm install 

Lo que integra todas las dependiencias necesarias.


 



El backend de Postgis es un servicio web que recibe peticiones via http y las redirige a postgis

El resultado del servicio web se expresa via  JSON.


**************************************
EJECUCION
*************************************
el servidor se ejecuta via

    npx lws --stack  body-parser lws-static lws-cors simple_server.js   

o bien tras ejecutar

    npm test

o via los archivos

    run_server.bat
    o
    ./run_server.sh
    o
    sh run_server.sh


y se accede via el url, aunque potree puede correr en su propio puerto y maquina.

http://localhost:8000/getInfo (POST)
http://localhost:8000/getData  (GET)
http://localhost:8000/getResults  (POST)


http://localhost:8000/(GET)



**************************************
DATOS
*************************************




