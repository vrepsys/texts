# Peninsula
Occasionaly, in software design, the choices we make are dictated not by best practices and what we believe is right. Instead they are more influenced by a desire to reduce our cognitive load and avoid complexity. Which is not bad per se, it's just that the perfect design should have both: have a feeling of the rigt way of doing things and also minimize the cognitive load.

One area where we found enginners choose less complexity over good design is dealing with jsons in our scala microservices. And this is caused by the complexity of dealing with simple jsons in scala using existing solutions. JSON - stands for Javascript Object Notation. Let's see how easy it is to perform a simple task on a javascipt object in Javascript.

```javascript
var json = {"response": { "status": "error", "error": "Some error message" }}

if (json.status === "error") {
  console.log(json.error)
}
```

In Peninsula we made doing simple things simple - like in Javascript.
```scala
import com.wix.peninsula.Json;

val json = Json.parse({"response": { "status": "error", "error": "Some error message" }});

if (json.contains("response.status", "error")) {
  println(json.extractString("response.error"))
}
```

You're working with json data - so the main domain object is `Json`. No ObjectMappers. No module registrations. No impicit Formats or serialization strategies.

With the simplicity in mind we developed a simple notation to define a path of a json value. We took inspiration from how Javascript operates thus making it familiar to developers who know javascript or even a simple oop.

`json.extractString('response.status')` - the obvious case
`json.extractString` - without any parameters - if the top level json is just a string value

Peninsula also lets you work conveniently with json arrays
```
val json = JSON PATH
```


### Extractions and inspections - making it predictable

We have extractions for primitive types.

In order for out API to be consistent and more intuitive surveyed developers about how they expect the extractions to work.

For example

`json.extractString(path)`
`json.extractStringTry(path)`


General rule is to use extractString when you expect the value to exist and not to be null. If the property's absence is an expected scenario one should use `json.extractStringTry`

Another approach is to make sure the value exists in advance using inspections e.g.

```
if (json.isString(path) {
  json.extractString(path)
}
```

`isString` true guarantees the success of `extractString` with the same path.


extractString, extractInt .. methods behave the same way with regards to presence and types:
JsonElementIsNull exception if value is null
JsonPathDoesntExist exception if the property on the path is not present
UnexpectedJsonElementException if the type of the value doesn't match the type expected

extractType methods do not do type coercion - meaning that getString("key") will always throw exception if the value of the key is anything else but a string.

It's a strict behaviour - but on the other hand you always know what to expect and it's consistent.

If you require or want some flexibility in how values are read you can use extractAs methods - that will try to coerce the values into the desired type.

### Extracting case classes and formats

Extract case classes using plain extract and extractOpt methods

```
json.extract[Preferences]("users(0).preferences")
```

When constructing or parsing json it is possible to specify json4s formats.

These formats will work for extract and extractOpt.

However, it does not impact extractType and extractTypeTry or inspection methods in any way. 














```
