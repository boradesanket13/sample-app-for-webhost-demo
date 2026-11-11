# Webhost E2E Test

A minimal static website for validating the Webhost deployment pipeline.


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
    repoUrl = "https://github.com/boradesanket13/sample-app-for-webhost-demo"
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




## What we're validating
1. GitHub source retrieval: Webhost clones the public repository.
2. Build execution: Runner executes npm run build.
3. Artifact detection: Webhost finds dist/index.html.
4. Storage: Built files are uploaded to MinIO.
5. Site serving: Site service serves the deployed application.