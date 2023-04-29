# Ejemplo de Map Reduce

Este ejemplo asume que ya tiene la BBDD MongoDB iniciada en Docker y accesible por Consola.

## Ejemplo

Obtener cuantaas películas se hicieron por lenguaje utilizando Map Reduce.

Resultado:
```
[
  { _id: 'Spanish', count: 135 },
  { _id: 'Chinese', count: 135 },
  { _id: 'Japanese', count: 134 },
  { _id: 'German', count: 134 },
  { _id: 'French', count: 134 },
  { _id: 'English', count: 119 },
  { _id: 'Russian', count: 107 },
  { _id: 'Italian', count: 102 }
]
```

### Solución
1. Se ingresa a la BBDD:

```
docker exec -it mcd-mongo-mysql_mongo_1 bash
```
2. Ya dentro del contenedor se ejecuta `mongosh -u root -p example`

3. Se ingresa a Sakila: `use sakila`.

4. Revisamos la estructura de la BBDD:

```javascript
db.film.find().limit(10)
```

5. Hacemos el map reduce.

```javascript
// Función map
var mapFunction = function() {
  emit(this.language_id, 1);
};

// Función reduce
var reduceFunction = function(language_id, counts) {
  return Array.sum(counts);
};

// Ejecutar mapReduce
var mapReduceResults = db.film.mapReduce(
  mapFunction,
  reduceFunction,
  {
    out: { inline: 1 }
  }
)

function updateResultsWithLanguageName(results) {
  return results.map(function(result) {
    var language = db.language.findOne({ _id: result._id });
    return {
      _id: language.name,
      count: result.value
    };
  });
}

var finalResults = updateResultsWithLanguageName(mapReduceResults.results);
printjson(finalResults);
```
