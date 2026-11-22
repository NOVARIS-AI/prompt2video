# Prompt 2 Real Video

Prompt2Video is a browser-based video creation studio with prompt/storyboard tools, local Canvas + MediaRecorder motion rendering, and an optional MCP-connected local-computer bridge.

## Features

- Existing Prompt2Video web studio and AI provider integrations.
- Offline Motion Studio: procedural Canvas animation and WebM export in the browser.
- Motion modes including story motion, kinetic graphics, procedural 3D-style illusions, image motion, and audio visualizer.
- MCP endpoint at `/mcp` with tools for studio status, storyboard planning, FFmpeg capability checks, local bridge status, local render jobs, and job status.
- Optional local bridge (`local-bridge.js`) that can detect Node.js, FFmpeg, Blender, and Python on a paired computer. The bridge only accepts the app's allowlisted procedural motion-render job; it does not expose arbitrary shell execution.
- FFmpeg support when installed in the deployment environment.

## Requirements

- Node.js 20 or newer recommended.
- npm.
- FFmpeg is optional for cloud-side FFmpeg features; install it in the host/container if needed.

## Run

```bash
npm ci
npm start
```

The server listens on port `8080` by default and binds to `0.0.0.0`.

## Configuration and secrets

Configure provider credentials as environment variables/secrets in the deployment host. Do not commit API keys, tokens, `.env` files, credential files, logs, generated media, or private pairing tokens.

The application may use configured OpenAI, Replicate, and other provider credentials for its existing API-powered features. The offline browser renderer does not require a video-generation API.

## MCP

The Streamable HTTP JSON-RPC endpoint is `/mcp`. Use an MCP client that supports Streamable HTTP. Available tools are returned by `tools/list`.

## Optional local computer bridge

1. Open MCP Hub in the deployed app and choose **Pair a computer**.
2. Download `local-bridge.js` from `/local-bridge.js` or use this repository copy.
3. On the computer, run `node local-bridge.js` and enter the short-lived pairing code.
4. Keep the bridge process running while local jobs are needed.

The bridge saves generated MP4 files to `Prompt2Video-Local-Renders/` on the computer. Pairing codes and bridge tokens are runtime credentials; never commit them.

## Deployment

Deploy as a Node.js service with `npm ci` followed by `npm start`, exposing port `8080`. Configure secrets in the hosting provider dashboard, not in source control.

GitHub Actions deploys pushes to `main` to the existing BasicDeploy production container. The workflow requires the repository Actions secret `BASICDEPLOY_API_KEY`. Manual runs are also available from the Actions tab.

## Limitations

Procedural Canvas motion and the local bridge's sample FFmpeg renderer create designed motion graphics, not photorealistic generative video. Blender and Python are currently detected by the bridge but their full render pipelines are not yet wired into the application. Browser `MediaRecorder` and audio capture support varies by browser/device.
