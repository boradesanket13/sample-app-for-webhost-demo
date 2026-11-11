# Webhost E2E Test

A minimal static website for validating the Webhost deployment pipeline.

## Requirements

- Node.js 20 or later
- npm

## Build locally

```powershell
npm install
npm run build
Test-Path dist/index.html
```

The final command should return `True`. The build copies `public/` into `dist/`, which is the deployment output directory expected by this test.

## Deploy

Push this repository to a public GitHub repository, then submit its HTTPS clone URL to the Webhost API:

```powershell
$KEY = "dev-token-change-me-0123456789"
$body = @{
    repoUrl = "https://github.com/YOUR_USERNAME/webhost-e2e-test"
} | ConvertTo-Json

$response = Invoke-RestMethod `
    -Uri "http://localhost:3000/v1/deployments" `
    -Method Post `
    -Headers @{ Authorization = "Bearer $KEY" } `
    -ContentType "application/json" `
    -Body $body

$response | ConvertTo-Json -Depth 10
```

Use the deployment ID returned by the API to inspect its status and logs.
