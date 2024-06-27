---
# You can also start simply with 'default'
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: SpiceDB
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# https://sli.dev/guide/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/guide/syntax#mdc-syntax
mdc: true
---

# SpiceDB

---

# Agenda

- Identification, Authentication, **Authorization**
- Kind of **Authentication**
- Zanzibar
- SpiceDB as a solution

---

# Identification, Authentication, Authorization

- **Identification** - Who are you? (@batazor)
- **Authentication** - How do you prove it? (password, token, etc)
- **Authorization** - What are you allowed to do? (RBAC, ABAC, etc)

### Flow

Does `<subject>` have `<permission>` to `<object>`?

**Example:**

> Does `<@batazor>` have permission `<add star>` to `<@spicedb>`?

---

# Authorization + Identification

- Basic Auth
- JWT (go-jwt, etc...)
- OAuth2 (go-oauth2, passportjs, etc...)
- OpenID Connect (go-oidc, etc...)

---

# Authentication

- https://casbin.org/
  - RBAC, ABAC, etc...
  - Go, NodeJS, Python, etc...
- **usually it's handmade**

---

# Kind of Authentication

- RBAC
- ABAC
- ReBAC
- ACL
- other standards...

---

# Kind of Authentication: RBAC (Role-Based Access Control)

![rbac](https://miro.medium.com/v2/resize:fit:601/1*ub3g0nUC6NCkYPHoNB6rFw.png)

---

# Kind of Authentication: RBAC (Role-Based Access Control)

+ Pros
  - Easy to implement
  - Easy to understand
- Cons
  - Not flexible
  - Not scalable
  - Not dynamic

---

# Kind of Authentication: ABAC (Attribute-Based Access Control)

![abac](https://cloudradius.com/wp-content/uploads/2022/09/ABAC-Image-1-2.png)

---

# Kind of Authentication: ABAC (Attribute-Based Access Control)

+ Pros
  - Flexible
  - Scalable
  - Dynamic
- Cons
  - Complex
  - Hard to implement
  - Hard to understand

---

# Kind of Authentication: ReBAC (Relationship-Based Access Control)

![rebac](https://media.graphassets.com/gTrUj0fZRV6RgqdKvXYv)

---

# Kind of Authentication: ReBAC (Relationship-Based Access Control)

+ Pros
  - Flexible
  - Scalable
  - Dynamic
- Cons
  - Complex
  - Hard to implement
  - Hard to understand

### Entities

- Subject
- Action
- Object
- Relationship

---

# Zanzibar

- [Zanzibar: Google’s Consistent, Global Authorization System](https://research.google/pubs/zanzibar-googles-consistent-global-authorization-system/)

---

# Implementation of Zanzibar

- [SpiceDB](https://spicedb.com/)
- [Ory Keto](https://www.ory.sh/keto/)
- and more...

---

# SpiceDB

- Open Source
- 4.7k stars
- UI (editor + test cases + save to file)
- SpiceDB Operator for Kubernetes
- Extentention for VSCode (Language Server)
- Wildcard policy

### References

- [ABAC on SpiceDB: Enabling Netflix’s Complex Identity Types](https://netflixtechblog.com/abac-on-spicedb-enabling-netflixs-complex-identity-types-c118f374fa89)

---

# SpiceDB: Schema

```graphql
// user represents a user that can be granted role(s)
definition user {}

// document represents a document protected by Authzed.
definition document {
    // writer indicates that the user is a writer on the document.
    relation writer: user

    // reader indicates that the user is a reader on the document.
    relation reader: user

    // edit indicates that the user has permission to edit the document.
    permission edit = writer

    // view indicates that the user has permission to view the document, if they
    // are a `reader` *or* have `edit` permission.
    permission view = reader + edit
}
```

---

# SpiceDB: Tuple

### Flow

Does `<subject>` have `<permission>` to `<object>`?

### Format

> `<resource>:<id>#<relation>@<subject>:<id>`

### Example

```graphql
document:firstdoc#writer@user:tom
document:firstdoc#reader@user:fred
document:seconddoc#reader@user:tom
```

---

# SpiceDB: In real life

1. Load schema to SpiceDB by API
2. Load tuples to SpiceDB by API
3. Check permission by API
4. PROFIT

---

# SpiceDB: Playgrounds

[DEMO](https://play.authzed.com/schema)

---

# References

- [SpiceDB Docs](https://authzed.com/docs/spicedb/getting-started/discovering-spicedb)
- [Почему авторизация сложно и причем здесь Занзибар? -Максим Горозий, Тинькофф](https://youtu.be/Tr5H8iG0FzI?si=WjvRZ554IpmPWEDd)