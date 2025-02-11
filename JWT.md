# Mastering JWT (JSON Web Tokens): A Deep Dive

JSON Web Token (abbreviated as JWT) is currently the most popular cross-domain authentication solution. This article introduces its principles and usage.

## I. The Problem of Cross-Domain Authentication
Internet services are inseparable from user authentication. The general process is as follows:

1. The user sends the username and password to the server.
2. After the server verifies successfully, it saves relevant data in the current session, such as the user role, login time, etc.
3. The server returns a `session_id` to the user and writes it into the user's Cookie.
4. For each subsequent request from the user, the `session_id` will be sent back to the server through the Cookie.
5. After receiving the `session_id`, the server finds the pre-saved data and thus knows the user's identity.

The problem with this model is that it has poor scalability. There is no problem for a single machine. However, if it is a server cluster or a cross-domain service-oriented architecture, session data sharing is required, and each server should be able to read the session. For example, website A and website B are related services of the same company. Now it is required that once a user logs in to one of the websites, when accessing the other website, they will be automatically logged in. How to achieve this?

One solution is to persist the session data and write it into a database or other persistent layer. After receiving requests, various services request data from the persistent layer. The advantage of this solution is that the architecture is clear, but the disadvantage is that the workload is relatively large. In addition, if the persistent layer fails, there will be a single-point failure.

Another solution is that the server simply does not save session data. All data is saved on the client and sent back to the server with each request. JWT is a representative of this solution.

## II. The Principle of JWT
The principle of JWT is that after the server authenticates, it generates a JSON object and sends it back to the user, like this:

```json
{
  "name": "Alice",
  "role": "admin",
  "expiration time": "0:00, July 1, 2024"
}
```

After that, when the user communicates with the server, this JSON object needs to be sent back. The server determines the user's identity entirely based on this object. To prevent the user from tampering with the data, the server will add a signature when generating this object (details will be described later). The server no longer saves any session data. That is to say, the server becomes stateless, which makes it easier to achieve expansion.

## III. The Data Structure of JWT
The actual JWT is probably like this:

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

It is a very long string, separated into three parts by dots (`.`). **Note** that **there is no line break inside JWT**. Here it is written in several lines for easy display. The three parts of JWT are as follows:

- **Header**
- **Payload**
- **Signature**

Written in one line, it looks like this: `Header.Payload.Signature`.

### 3.1 Header
The **Header** part is a JSON object that describes the metadata of JWT, usually like this:

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

In the above code, the `alg` attribute represents the signature algorithm (algorithm), and the default is HMAC SHA256 (written as `HS256`); the `typ` attribute represents the type of this token, and JWT tokens are uniformly written as `JWT`. Finally, the above JSON object is converted into a string using the Base64URL algorithm (details will be described later).

### 3.2 Payload
The **Payload** part is also a JSON object used to store the data that actually needs to be transmitted. JWT defines 7 official fields for selection:

- `iss` (issuer): Issuer
- `exp` (expiration time): Expiration time
- `sub` (subject): Subject
- `aud` (audience): Audience
- `nbf` (Not Before): Effective time
- `iat` (Issued At): Issuing time
- `jti` (JWT ID): Serial number

In addition to the official fields, you can also define private fields in this part. The following is an example:

```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```

Note that JWT is not encrypted by default, and anyone can read it. Therefore, do not put secret information in this part. This JSON object also needs to be converted into a string using the Base64URL algorithm.

### 3.3 Signature
The **Signature** part is the signature of the first two parts to prevent data tampering. First, a secret key (`secret`) needs to be specified. This key is only known to the server and cannot be leaked to the user. Then, use the signature algorithm specified in the Header (the default is HMAC SHA256) to generate the signature according to the following formula:

```
HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload), secret)
```

After calculating the signature, combine the Header, Payload, and Signature parts into a string, separated by "dots" (`.`) between each part, and then it can be returned to the user.

### 3.4 Base64URL
As mentioned before, the serialization algorithm for Header and Payload is **Base64URL**. This algorithm is basically similar to the Base64 algorithm, but there are some small differences. As a token, JWT may be placed in a URL in some cases (such as `api.example.com/?token=xxx`). The Base64 has three characters `+`, `/`, and `=`, which have special meanings in the URL, so they need to be replaced: 

- `=` is omitted,
- `+` is replaced with `-`, and
- `/` is replaced with `_`.

This is the **Base64URL** algorithm.

## IV. The Usage of JWT
After receiving the JWT returned by the server, the client can store it in the Cookie or in `localStorage`. Thereafter, every time the client communicates with the server, it needs to bring this JWT. You can put it in the Cookie and send it automatically, but this cannot be cross-domain. So a better way is to put it in the **Authorization** field of the HTTP request header.

```text
Authorization: Bearer <token>
```

Another way is that when cross-domain, the JWT is placed in the data body of the POST request.

## V. Several Characteristics of JWT
1. JWT is not encrypted by default, but it can be encrypted. After generating the original Token, it can be encrypted again with a secret key.
2. In the case that JWT is not encrypted, secret data cannot be written into JWT.
3. JWT can be used not only for authentication but also for exchanging information. Using JWT effectively can reduce the number of times the server queries the database.
4. The biggest disadvantage of JWT is that since the server does not save the session state, it is impossible to revoke a certain token during use or change the token's permissions. That is to say, once a JWT is issued, it will be valid until it expires, unless the server deploys additional logic.
5. JWT itself contains authentication information. Once it is leaked, anyone can obtain all the permissions of the token. To reduce theft, the validity period of JWT should be set relatively short. For some more important permissions, the user should be authenticated again when using them.
6. To reduce theft, JWT should not be transmitted in plaintext using the HTTP protocol, but should be transmitted using the HTTPS protocol.

