# HTTP Handoff

Turn a folder of JetBrains `.http` requests into one Postman collection for a customer.

Keep your API examples beside your code. When a customer needs Postman, select the request folder, review the conversion, and save a collection they can import. Each source file becomes a named folder in the collection.

## Use

1. Save changes to your request files.
2. Open **Tools → Export HTTP Folder to Postman…**, or use the same action in the Project view's context menu.
3. Select an `.http` file, a `.rest` file, or a folder. Subfolders are included.
4. Leave literal credentials excluded unless you intentionally need them in this handoff.
5. Select **Review Export**. Resolve every **FIX** notice in the source, then export again. Check **REVIEW** notices and the collection preview.
6. Save to a new JSON filename. Existing exports are preserved.
7. In Postman, choose **Import** and select the JSON file. Set missing environment variables, run the requests against an appropriate test system, and review the collection before sharing it.

A Marketplace trial or license is required for the paid release. Version 1.0.0 is submitted to Marketplace for review as a hidden plugin. Public sales and trial activation are not available yet.

## What converts

| Source | Postman result |
|---|---|
| HTTP/HTTPS methods, headers and query strings | Collection requests |
| Multiple files and nested folders | One collection with a folder for each source file |
| `@name = value` file variables | Values scoped to their source file, including nested references |
| `Authorization: Basic username password` | Postman's Basic authentication configuration |
| Bearer headers and ordinary variable placeholders | Preserved headers/placeholders |
| Multiline URLs | Joined URL path and query |
| Inline text, JSON and XML bodies | Raw request bodies |
| `< ./payload.json` and other local UTF-8 text bodies | Included body content |
| Simple `client.test`, `client.assert`, `client.log`, `response.status` and `response.body` scripts | A Postman response-test adapter; review its behavior |

**Variable policy:** a selected Postman environment overrides file variable defaults. This is an explicit handoff policy. The downloaded JetBrains command-line runner behaved differently in an override test; do not assume all environments behave identically. Nested file references are resolved in dependency order. Circular definitions block export.

## Deliberate limits

This version blocks gRPC, WebSocket and CONNECT requests; multipart bodies; dynamic variables, JSONPath and response-reference variables; duplicate headers; unsupported request directives; pre-request scripts; external scripts; and response-script APIs outside the small adapter above. It produces a file and line diagnostic instead of offering a partial export when unsupported syntax is detected. Response script checks are a limited compatibility screen, not a general JavaScript interpreter.

Private environment files are not imported. Supply values in Postman. Referenced text bodies must be inside the selected folder (or the selected file's folder). Symbolic request files and outside references are rejected. Limits: 1,000 files, 5,000 requests, 128 distinct file variable names, 10 MB per file and 50 MB of source/body reads.

## Data and credentials

Conversion runs on your computer. HTTP Handoff does not call your endpoints, execute source scripts, upload request files, or change your source files. Scripts included in a collection can run later when you run that collection in Postman.

Literal credential headers and sensitively named file variables are replaced with placeholders by default. This is not a complete secret scanner: URLs, body content, scripts and unusually named variables may still contain private data. Inspect every collection before sharing. The IDE and Marketplace handle licensing and their normal network activity separately.

## Support

Email teagan101@gmail.com with **HTTP Handoff** in the subject. Include your IDE version, plugin version, the diagnostic, expected behavior, and a small synthetic example that reproduces the problem. Do not send production tokens, private environment files or customer datasets.

Billing, subscription cancellation and refunds are handled through JetBrains. HTTP Handoff support covers the documented conversion behavior; custom integrations and bespoke migration work are outside this product's scope.

## Verification scope

The 1.0.0 release archive's converter passed nine synthetic cases executed by Newman 6.2.2 against a local echo service. Eleven core and IntelliJ Platform tests cover source preservation, blocking incomplete exports, dialog state and synthetic licensing states. Plugin Verifier reports compatibility with IntelliJ IDEA 2026.1.4 (IU-261.26222.65). The complete interactive export and actual Marketplace trial activation remain pending. These checks are not evidence of customer adoption or a completed purchase.

HTTP Handoff is an independent product. JetBrains and Postman names identify the tools it works with; they do not imply endorsement.
