# Crisp Mini API Gateway

This mini project makes a fresh, modern vision for book discovery.

A lightweight, local Node.js API gateway with a GitHub repository connection and configurable upstream routes.

## Run locally

Requires Node.js 18 or newer. No npm dependencies are required.

```powershell
npm start
```

Open [http://127.0.0.1:3000](http://127.0.0.1:3000). The server binds to loopback only. Gateway settings are saved in `data/config.json`.

The customer-facing bookstore is available at [http://127.0.0.1:3000/bookstore.html](http://127.0.0.1:3000/bookstore.html). It includes the original 12-book catalog, search, category filters, sorting, and a demo shopping bag.

## Connect GitHub

Enter a public repository URL such as `https://github.com/octocat/Hello-World`. The gateway retrieves its name and default branch from GitHub's public API. It does not store a token, clone the repository, deploy it, or execute its code.

## Add a gateway route

Create a route with a method, a gateway path, and an HTTP(S) upstream URL. For example:

| Method | Gateway path | Upstream URL |
|---|---|---|
| GET | `/users` | `https://jsonplaceholder.typicode.com/users` |

Send a request to `http://127.0.0.1:3000/gateway/users`. Query parameters and supported request headers are forwarded. Route parameters such as `/users/:id` can be used with an upstream URL containing `{id}`, for example `https://jsonplaceholder.typicode.com/users/{id}`.

The admin API is local-only:

- `GET /api/config` — repository and route configuration
- `POST /api/github/connect` — connect a public GitHub repository (`{"url":"https://github.com/owner/repo"}`)
- `DELETE /api/github/connect` — disconnect the repository
- `GET /api/endpoints` is represented through `/api/config`; `POST /api/endpoints` creates a route
- `PUT /api/endpoints/:id` and `DELETE /api/endpoints/:id` update or remove a route
- `GET /api/logs` — recent request metadata (no payloads or credentials)

This is a local development MVP, not a production-hardened public gateway: it has no user authentication, TLS termination, or persistent audit log. Do not expose it to an untrusted network.