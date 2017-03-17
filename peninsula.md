# Peninsula
Occasionaly, in software design, the choices we make are dictated not by best practices and what we believe is right. Instead they are more influenced by a desire to reduce our cognitive load and avoid complexity. Which is not bad per se, it's just that the perfect design should have both: have a feeling of the rigt way of doing things and also minimize the cognitive load.

One area where we found enginners choose less complexity over good design is dealing with jsons in our scala microservices.

And this is caused by the complexity of dealing with simple jsons in scala using existing solutions.

JSON - stands for Javascript Object Notation.

Let's see how easy it is to perform a simple task on a javascipt object in Javascript.

```javascript
var json = {"response": { "status": "error", "error": "Some error message" }}

if (json.status === "error") {
  console.log(json.error)
}
```

In Peninsula we made doing simple things simple - like in Javascript.
```
import com.wix.peninsula.Json;

val json = Json.parse({"response": { "status": "error", "error": "Some error message" }});

if (json.contains("response.status", "error")) {
  println(json.extractString("response.error"))
}
```

You're working with json data - so the main domain object is `Json`. No ObjectMappers. No module registrations. No impicit Formats or serialization strategies.

With the simplicity in mind we developed a simple notation to define a path of a json value. We took inspiration from how Javascript operates thus making it familiar to developers who know javascript or simple oop.







```
