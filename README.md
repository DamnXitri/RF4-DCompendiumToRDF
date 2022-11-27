# RF4-DCompendiumToRDF

This repository contains the code to convert the modified Rune Factory 4 - DataminingCompendium to RDF, using a modified version of the [Rune Factory 4 - Datamining Compendium](https://www.reddit.com/r/runefactory/comments/he3uu4/rf4_datamining_compendium_values_for_everything/) by u/OmnigamerSDA on Reddit.

## Used Software
- Tarql (https://github.com/tarql/tarql/releases)
- Apache Jena Fuseki (https://jena.apache.org/download/)

## Ejecucion de tarql

En el caso de querer generar nuevamente los archivos de output, deben utilizar los mapppings creados en la carpeta mappings y los archivos de input en la carpeta input. Para esto, con una terminal posicionada en la carpeta mappings, deben tener en cuenta lo siguiente:

Para utilizar tarql con los csv, utilizar el siguiente comando \
```<tarql.bat Route> <mapping>.sparql <source>.csv > <output>.ttl```

Por ejemplo, con nuestra disposicion de carpetas, para ejecutar el mapping de itemCategories utilizamos:\
```..\..\tarql-1.2\bin\tarql worldMap.sparql ..\input\worldMap.csv > ..\output\worldMap.ttl```

Tambien podiamos utilizar: \
```<tarql.bat Route> <mapping>.sparql > <output>.ttl``` \
Si en el mapping esta definido FROM, lo cual ocurre en todos los mappings :)

## Evitar tripletas duplicadas

En algunos casos, queremos evitar tripletas duplicadas, esto se evita con la flag `--dedup <size>`, por ejemplo: \
```..\..\tarql-1.2\bin\tarql --dedup 100000 worldMap.sparql > ..\output\worldMap.ttl``` \
El valor de `size` es el numero de lineas que se van a guardar en memoria antes de hacer el dedup. En general nuestros archivos no tienen
mas de 20k lineas y 100k no son problema para las memorias actuales :3

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
