# Atlas Travel Catalogue

A complete implementation for Cloud Computing Assignment 2: Public Cloud. It supports adding destinations, listing all destinations, filtering by search/continent/mood/budget, validation with friendly errors, and deployment to a public Azure URL.

## Local development

1. Install Node.js 18 or newer.
2. Run `npm install`.
3. Run `npm start`.
4. Open `http://localhost:3000`.

Without environment variables, the app stores destinations in `data.json`, which is useful for local development. Copy `.env.example` to `.env` when connecting Azure services.

## Azure architecture

- **Azure App Service** hosts the Node.js Express web application and provides the public HTTPS URL.
- **Azure Cosmos DB for NoSQL** stores destination documents. The app creates the database/container on first connection and partitions by `continent`.
- **Azure Blob Storage** stores optional uploaded destination images. This is cloud feature 1.
- **Application Insights** captures request/dependency telemetry and failures. This is cloud feature 2.

When deployed, set `COSMOS_ENDPOINT`, `COSMOS_KEY`, `COSMOS_DATABASE`, `COSMOS_CONTAINER`, `AZURE_STORAGE_CONNECTION_STRING`, `AZURE_STORAGE_CONTAINER`, and `APPLICATIONINSIGHTS_CONNECTION_STRING` in App Service Configuration. Never commit `.env` or service keys.

## Suggested Azure deployment

Create a GitHub repository containing this folder. In Azure Portal, create a resource group, a Cosmos DB account, a Storage account, an Application Insights resource, and an App Service running Node 18/20. In App Service Deployment Center, select GitHub and the repository/branch. Add the variables above under **Configuration > Application settings**. App Service will run `npm install` and `npm start`; the public URL will look like `https://<app-name>.azurewebsites.net`.

For a low-cost submission, use student credits/free tiers where available, stop or delete the App Service after grading, and remove unused storage/Cosmos resources.

## Assignment checklist

- Data layer: Cosmos DB stores destinations; local JSON is only a development fallback.
- Web application: create, list, filter, validation/error messages, public App Service URL.
- Two cloud features: Blob Storage image upload and Application Insights monitoring.
- Documentation: use `docs/architecture.md` as the basis for the PDF submitted to Moodle.
