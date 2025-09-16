# api-key-auth

This plugin allows you to protect routes with an API key specified in a header, query string param or path segment. If the user does not provide a valid key the middleware will return a 403 (unless in permissive mode).

## Config

### Static file

Add to your Traefik static configuration

#### yaml

```yaml
experimental:
  plugins:
    traefik-api-key-auth:
      moduleName: "github.com/rinokadijk/traefik-permissive-api-key-auth"
      version: "v0.0.4"
```

#### toml

```toml
[experimental.plugins.traefik-api-key-auth]
  moduleName = "github.com/rinokadijk/traefik-permissive-api-key-auth"
  version = "v0.0.4"
```

### CLI

Add to your startup args:

```sh
--experimental.plugins.traefik-api-key-auth.modulename=github.com/rinokadijk/traefik-permissive-api-key-auth
--experimental.plugins.traefik-api-key-auth.version=v0.0.4
```

### K8s CRD

```yaml
apiVersion: traefik.io/v1alpha1
kind: Middleware
metadata:
  name: extract-api-key
spec:
  plugin:
    traefik-api-key-auth:
      authenticationHeader: true
      authenticationHeaderName: X-API-KEY
      bearerHeader: true
      bearerHeaderName: Authorization
      queryParam: true
      queryParamName: token
      pathSegment: true
      permissiveMode: false
      removeHeadersOnSuccess: true
      internalForwardHeaderName: ''
      internalErrorRoute: ''
```

## Plugin options

| option                      | default           | type     | description                                                       | optional |
|:----------------------------|:------------------|:---------|:------------------------------------------------------------------|:-------|
| `authenticationHeader`      | `true`            | bool     | Use an authentication header to pass a valid key.                 | ⚠️      |
| `authenticationHeaderName`  | `"X-API-KEY"`     | string   | The name of the authentication header.                            | ✅     |
| `bearerHeader`              | `true`            | bool     | Use an authorization header to pass a bearer token (key).         | ⚠️      |
| `bearerHeaderName`          | `"Authorization"` | string   | The name of the authorization bearer header.                      | ✅     |
| `queryParam`                | `true`            | bool     | Use a query string param to pass a valid key.                     | ⚠️      |
| `queryParamName`            | `"token"`         | string   | The name of the query string param.                               | ✅     |
| `pathSegment`               | `true`            | bool     | Use match on path segment to pass a valid key.                    | ⚠️      |
| `permissiveMode`            | `false`           | bool     | Dry-run option to allow the request even if no valid was provided | ✅     |
| `removeHeadersOnSuccess`    | `true`            | bool     | If true will remove the header on success.                        | ✅     |
| `internalForwardHeaderName` | `""`              | string   | Optionally forward validated key as header to next middleware.    | ✅     |
| `internalErrorRoute`        | `""`              | string   | Optionally route to backend at specified path on invalid key      | ✅     |
| `keys`                      | `[]`              | []string | A list of valid keys that can be passed using the headers.        | ✅      |

⚠️ - Is optional but at least one of `authenticationHeader`, `bearerHeader`, `queryparam` or `pathSegment` must be set to `true`.

❌ - Required.

✅ - Is optional and will use the default values if not set.
