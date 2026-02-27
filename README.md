# unofficial-htb-api

Unofficial, community-maintained OpenAPI specs for Hack The Box platform APIs.

[Interactive docs (Swagger UI)](https://gubarz.github.io/unofficial-htb-api/)

## Why This Exists

HTB's platform APIs are not officially documented. This repo tracks observed API behavior as OpenAPI specs so people can:

- Generate typed clients instead of hardcoding endpoints by hand
- Explore request/response contracts quickly
- Diff schema changes over time with regular tooling

## Start Here (Tool Builders)

Use the bundled specs directly:

- v4: `https://raw.githubusercontent.com/Gubarz/unofficial-htb-api/main/docs/openapi.v4.yaml`
- v5: `https://raw.githubusercontent.com/Gubarz/unofficial-htb-api/main/docs/openapi.v5.yaml`

## Generate Code Quickly

### TypeScript (fetch)

```bash
openapi-generator-cli generate \
  -i https://raw.githubusercontent.com/Gubarz/unofficial-htb-api/main/docs/openapi.v5.yaml \
  -g typescript-fetch \
  -o ./generated/htb-v5-client
```

### Python

```bash
openapi-generator-cli generate \
  -i https://raw.githubusercontent.com/Gubarz/unofficial-htb-api/main/docs/openapi.v4.yaml \
  -g python \
  -o ./generated/htb-v4-client
```

### Go (oapi-codegen)

```bash
curl -fsSL https://raw.githubusercontent.com/Gubarz/unofficial-htb-api/main/docs/openapi.v5.yaml -o /tmp/htb-openapi.v5.yaml
oapi-codegen -generate types,client -package htb -o ./generated/htb/client.gen.go /tmp/htb-openapi.v5.yaml
```

## Choose Your Integration Path

- Use generated clients if you want full control in your language/runtime.
- Use [gohtb](https://github.com/Gubarz/gohtb) if you want a higher-level Go library already built from these specs.

```bash
go get github.com/gubarz/gohtb
```

## Authentication

Most endpoints require a bearer token.

```http
Authorization: Bearer <HTB_APP_TOKEN>
```

Tokens are created on `app.hackthebox.com`.

## Repository Layout

```text
openapi/v4/openapi.yaml            # v4 root spec
openapi/v4/paths/*.yaml            # v4 path operations
openapi/v5/openapi.yaml            # v5 root spec
openapi/v5/paths/*.yaml            # v5 path operations
openapi/v5/components/**/*.yaml    # shared v5 schemas/responses
docs/index.html                    # hosted Swagger UI
docs/openapi.v4.yaml               # bundled v4 spec for docs/codegen
docs/openapi.v5.yaml               # bundled v5 spec for docs/codegen
```

## Local Workflow

1. Update or add path/component YAML files.
2. Wire references from the relevant root `openapi.yaml`.
3. Lint and bundle before pushing:

```bash
redocly lint openapi@v4
redocly lint openapi@v5
redocly bundle openapi@v4 --output openapi.v4.yaml
redocly bundle openapi@v5 --output openapi.v5.yaml
```

4. Open the hosted docs for visual checks:

- https://gubarz.github.io/unofficial-htb-api/

## Use in API Tools

Import any of the following into Postman, Insomnia, or Prism:

- `openapi/v4/openapi.yaml`
- `openapi/v5/openapi.yaml`
- bundled raw URLs from `Start Here`

## Reverse-Engineering Notes

Specs are maintained from observed web app/API behavior and refined continuously.
Because upstream is undocumented and can change at any time, occasional mismatches are expected.

## Contributing

Contributions are welcome. For endpoint updates, include:

1. Method and path
2. Auth requirement
3. Response/request shape changes (sanitized)
4. Updated references in `paths`, `components`, and root `openapi.yaml`

If you spot regressions or stale fields, open an issue with reproducible details.

## FAQ

### Why not just call endpoints directly?

You can, but generated clients usually give better type safety, fewer copy/paste bugs, and easier upgrades.

### Which spec should I use: v4 or v5?

Use whichever surface your target endpoints live on. Many tools consume both.

### Is this official?

No. This is community-maintained reverse-engineered documentation.

## Disclaimer

- This project is not affiliated with or endorsed by Hack The Box.
- All API docs here are unofficial and reverse-engineered.
- Use at your own risk.
