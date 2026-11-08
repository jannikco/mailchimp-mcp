# Mailchimp MCP server

Inspect audiences and their members before explicitly requesting double opt-in for a consenting subscriber.

A direct-provider MCP server with useful local execution. No PipMCP account is required for local use. Provider API access and any provider charges are your responsibility.

## Quick start

Requires Node.js22+. Clone this repository, then run:

```sh
npm ci --ignore-scripts
npm test
npm start
```

The default command inspects the tools offline. Configure your own credentials in your environment:

- `MAILCHIMP_USERNAME`
- `MAILCHIMP_API_KEY`
- `MAILCHIMP_DC`



Credential help: https://us22.api.mailchimp.com/schema/3.0/Paths/Lists/Collection.json

## Provider setup

MAILCHIMP_DC is the data-center suffix of your own API key, e.g. us22. Set MAILCHIMP_USERNAME to any nonempty label and MAILCHIMP_API_KEY to your key. Add members only with the required consent. Pending status may send a confirmation email; provider audience requirements remain in force.

## Execute a real read

Replace example record IDs with records you are authorized to access.

```sh
npm start -- --call mailchimp_list_audiences '{"count":10}'
```

## Use with an MCP client

Launch `node /absolute/path/mailchimp-mcp/server.mjs` using stdio transport; pass credentials through the client environment. Stdout is reserved for JSON-RPC.

## Tools

| Tool | Effect | Purpose |
| --- | --- | --- |
| mailchimp_list_audiences | Read | List one page of audiences. |
| mailchimp_list_members | Read | List members in one authorized audience. |
| mailchimp_add_pending_member | Explicit approval required | Add a consenting subscriber with pending status, triggering Mailchimp double opt-in confirmation. |

## Concrete workflow

Inspect audiences and their members before explicitly requesting double opt-in for a consenting subscriber.

1. mailchimp_list_audiences
2. mailchimp_list_members
3. mailchimp_add_pending_member

## Optional writes

Writes are disabled by default. Set CONNECTOR_ALLOW_WRITES=true and supply `_approval: {approved:true,idempotencyKey:"unique-request-id"}` on the specific call. Do not reuse a key for a different request. Uncertain outcomes are never automatically replayed; inspect the provider before another attempt.

## Capability evidence

Local: **Tested — HTTP fixtures and real stdio tools/call**. Managed: **Documented**. Provider authentication and live operations have not been established by these fixture tests. Evidence applies per operation and execution mode; setup discovery does not imply provider execution.

## Managed production

[Import with PipMCP](https://pipmcp.com/github/start?integration=openapi-to-mcp&utm_source=github&utm_campaign=github-mailchimp-mcp&utm_content=readme) when you want the hosted service. This provider is not currently listed as a managed template. Import selected-openapi.json (REST) or the official GraphQL schema linked in provenance, configure authentication, and review the tools before execution. Local credentials are not uploaded by this code. Review provider permissions in the managed onboarding. [Directory and related projects](https://jannikco.github.io/pipmcp-connectors/integrations/mailchimp/).

## Provenance and maintenance

- https://us22.api.mailchimp.com/schema/3.0/Paths/Lists/Collection.json
- https://us22.api.mailchimp.com/schema/3.0/Paths/Lists/Members/Collection.json

Maintainer:jannikco. MIT license for this code. See connector.json for selected operations and integration.json for evidence. Runtime dependencies retain their own licenses. Report sensitive issues through https://pipmcp.com/contact; do not post credentials or customer data in public issues.
