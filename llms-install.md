# Installing the Outcrawl MCP server

Outcrawl's MCP server is hosted. There is nothing to clone, build or run locally, and no package to install.

If you are Cline, these are the steps:

1. Add a remote server named `outcrawl` with transport streamable HTTP and the URL `https://api.outcrawl.ai/mcp`.
   With the Cline CLI: `cline mcp add outcrawl https://api.outcrawl.ai/mcp --transport streamableHttp --yes`.
   Or add this to `cline_mcp_settings.json`:

   ```json
   {
     "mcpServers": {
       "outcrawl": {
         "type": "streamableHttp",
         "url": "https://api.outcrawl.ai/mcp"
       }
     }
   }
   ```

2. Do not add any env vars. The server uses OAuth 2.1 with dynamic client registration: on the first tool call the client opens the user's browser to sign in to Outcrawl and pick a workspace. Only if the client cannot do OAuth, ask the user for an API key from https://outcrawl.ai/console/keys and add the header `"headers": { "Authorization": "Bearer ock_YOUR_KEY_ID.YOUR_SECRET" }`.
3. Check it works by calling `outcrawl_credits`, which reports the account's remaining credits and spends nothing.

Docs: https://outcrawl.ai/docs/mcp/setup
