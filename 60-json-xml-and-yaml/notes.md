# **CCNA Revision Notes: JSON, XML & YAML**

## **1. Data Serialization**

- Converts data into a standardized format so different applications can exchange and understand it.
- Used to store or transmit data over a network.
- Example: JSON, XML, YAML.

## **2. JSON (JavaScript Object Notation)**

- Human-readable data interchange format.
- Language-independent.
- Commonly used by REST APIs.
- CCNA exam topic 6.7: Interpret JSON-encoded data.
- Whitespace is insignificant (spaces and line breaks don't affect meaning).

**Six JSON data types:**

| Type    | Meaning              | Example         |
| ------- | -------------------- | --------------- |
| String  | Text (double quotes) | `"R1"`          |
| Number  | Numeric value        | `100`           |
| Boolean | True or false        | `true`          |
| Null    | No value             | `null`          |
| Object  | Key-value pairs `{}` | `{"name":"R1"}` |
| Array   | List of values `[]`  | `["R1","R2"]`   |

**JSON syntax rules:**

- Objects use `{ }`.
- Arrays use `[ ]`.
- Keys must always be strings (double quotes).
- Colon `:` separates keys and values.
- Comma `,` separates multiple pairs or array values.
- No comma after the final item.
- Objects can contain nested objects and arrays.
- Array values can have different data types.

Example:

```
{
  "device": {
    "name": "R1",
    "vendor": "Cisco"
  },
  "is_up": true,
  "interfaces": ["G0/0", "G0/1"],
  "speed": 1000
}
```

## **3. XML (Extensible Markup Language)**

- Uses tags to represent key-value pairs.
- Similar structure to HTML.
- Whitespace is insignificant.
- Used by REST APIs.
- Generally less human-readable than JSON.

Example:

```
<interface>GigabitEthernet0/0</interface>
```

## **4. YAML (YAML Ain't Markup Language)**

- Human-readable data serialization format.
- Used by Ansible for network automation.
- Whitespace and indentation are significant.
- Starts with three hyphens `---`.
- Uses `key: value` format.
- Uses `-` to indicate list items.

Example:

```
---
device: R1
interfaces:
  - G0/0
  - G0/1
```

## **5. Key Differences**

| Feature                 | JSON            | XML             | YAML            |
| ----------------------- | --------------- | --------------- | --------------- |
| Whitespace significant? | No              | No              | Yes             |
| Structure               | `{}` and `[]`   | Tags            | Indentation     |
| Human-readable          | Yes             | Less            | Yes             |
| REST APIs               | Yes             | Yes             | Not highlighted |
| Ansible                 | Not highlighted | Not highlighted | Yes             |

## **CCNA Exam Reminders**

- **JSON:** Focus on interpreting data, identifying data types and understanding syntax.
- **XML:** Recognize HTML-like opening and closing tags.
- **YAML:** Remember indentation matters and it is used by Ansible.
- `true` = Boolean; `"true"` = String.
- `100` = Number; `"100"` = String.
- `{}` = Object; `[]` = Array.
- A JSON key is always a string, not a separate data type.
