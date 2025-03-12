# @JsonIgnore

`@JsonIgnore` is an annotation from Jackson (a popular JSON processing library in Java) that is used to **exclude** a field from being serialized (converted to JSON) or deserialized (read from JSON).

### Use Case
- To prevent sensitive data (like passwords) from being exposed in API responses.
- To avoid circular references when serializing entities in a bidirectional relationship.
- To exclude unnecessary fields from JSON responses.

### Example
Without `@JsonIgnore`
```java
import com.fasterxml.jackson.databind.ObjectMapper;

class User {
    public String username;
    public String password; // Should not be exposed in JSON!
}

public class Main {
    public static void main(String[] args) throws Exception {
        User user = new User();
        user.username = "RajSony";
        user.password = "superSecret123";

        ObjectMapper objectMapper = new ObjectMapper();
        String json = objectMapper.writeValueAsString(user);
        System.out.println(json);
    }
}
```

### Output (Sensitive Data Exposed!)

```json
{"username":"RajSony","password":"superSecret123"}
```

---

### Example
With `@JsonIgnore`

```java
import com.fasterxml.jackson.annotation.JsonIgnore;
import com.fasterxml.jackson.databind.ObjectMapper;

class User {
    public String username;

    @JsonIgnore
    public String password; // Now excluded from JSON
}

public class Main {
    public static void main(String[] args) throws Exception {
        User user = new User();
        user.username = "RajSony";
        user.password = "superSecret123";

        ObjectMapper objectMapper = new ObjectMapper();
        String json = objectMapper.writeValueAsString(user);
        System.out.println(json);
    }
}
```

### Output (Secure)

```json
{"username":"RajSony"}
```

Key Points
- ✔ Prevents sensitive data exposure in JSON responses.
- ✔ Only affects Jackson serialization/deserialization, does not impact database storage.
- ✔ Can be applied to fields or getters (`@JsonIgnore` on a getter prevents JSON serialization but allows setting values normally in Java).
- ✔ Useful for bidirectional relationships in JPA to prevent infinite recursion.
