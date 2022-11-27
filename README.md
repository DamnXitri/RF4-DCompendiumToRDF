# RF4-DCompendiumToRDF

## Ejecucion de tarql

En el caso de querer generar nuevamente los archivos de output, 

Para utilizar tarql con los csv, utilizar el siguiente comando \
```<tarql.bat Route> <mapping>.sparql <source>.csv > <output>.ttl```

Por ejemplo, para ejecutar el mapping de itemCategories \
```..\tarql-1.2\bin\tarql.bat itemCategories.sparql itemCategories.csv > itemCategories.ttl```

Tambien se puede usar: \
```..\tarql-1.2\bin\tarql.bat <mapping>.sparql > <output>.ttl``` \
Si en el mapping esta definido FROM (en todos c : )

## Evitar tripletas duplicadas

En algunos casos, queremos evitar tripletas duplicadas, esto se evita con la flag `--dedup <size>`, por ejemplo: \
```..\tarql-1.2\bin\tarql.bat --dedup 100000 worldMap.sparql > worldMap.ttl``` \
El valor de `size` es el numero de lineas que se van a guardar en memoria antes de hacer el dedup. En general nuestros archivos no tienen
mas de 20k lineas y 100k no son problema para las memorias actuales :3 \

## Ejecucion de Sparql Endpoint

Utilizamos apache jena :O \
Para correr el servidor, deben posicionarse en la carpeta `apache-jena-fuseki-4.6.1` y ejecutar el siguiente comando: \
```.\fuseki-server --port <portNumber (por defecto 3030)>``` \
Luego, en el navegador, ir a `http://localhost:<portNumber>/`, crear la database y cargar los archivos ttl.

## Hacer consultas

Utilizamos
```PREFIX ex: <http://ex.org/a#>```
para definir nuestras propiedades.

Un ejemplo de consulta es:
```sparql
PREFIX ex: <http://ex.org/a#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT *
WHERE {
  ?URI a ex:MapLocation.
  ?URI ex:mainArea ?MainArea.
  OPTIONAL {
    ?URI ex:floor ?Floor.
    ?URI ex:subArea ?SubArea.
  }
  
  ?URI ex:has ?Has.
  
  OPTIONAL {
    ?Has a ?HasType .
    ?Has rdfs:label ?Label.
  }

}
LIMIT 150
```

Donde se obtienen las MapLocations, su mainArea, floor, subArea, y las cosas que se encuentran en la zona.
