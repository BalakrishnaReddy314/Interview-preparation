# SPFX (SharePoint Framework)

Preparing for an SPFX (SharePoint Framework) interview with 6 years of experience means you’ll need to demonstrate a solid understanding of both foundational and advanced concepts, as well as practical, real-world application. SPFX is a modern development model for SharePoint, so expect questions on its architecture, tooling, and integration with Microsoft 365. Here’s a tailored list of topics to prepare for, considering your experience level:

### 1. Core SPFX Concepts
- **SPFX Architecture**: Understand the client-side development model, web parts, extensions, and the role of the SharePoint workbench.
- **SPFX Components**: Differences between web parts, application customizers, field customizers, and command sets.
- **TypeScript**: Proficiency in TypeScript (used heavily in SPFX), including types, interfaces, and async/await patterns.
- **React**: SPFX’s default framework—know React components, hooks, state management, and lifecycle methods.
- **SPFX Versions**: Familiarity with the evolution of SPFX (e.g., v1.0 to v1.19) and backward compatibility.

### 2. Development Environment and Tooling
- **Node.js and npm**: Managing dependencies, package.json, and version conflicts.
- **Yeoman and Gulp**: SPFX project scaffolding with Yeoman and task automation with Gulp (e.g., `gulp serve`, `gulp bundle`, `gulp package-solution`).
- **Webpack**: Understanding bundling and how SPFX leverages it for optimization.
- **PnP PowerShell and CLI for Microsoft 365**: Deployment and administration tasks.
- **Debugging**: Techniques for debugging SPFX solutions in the browser and VS Code.

### 3. SharePoint Integration
- **SharePoint REST API**: CRUD operations, batching, and querying lists/libraries.
- **Microsoft Graph API**: Integration with Graph for Teams, OneDrive, or user data, including authentication flows.
- **SPFX Context**: Using the `this.context` object to access SharePoint data, user info, and page context.
- **PnPJS**: Familiarity with the PnP JavaScript library for simplified SharePoint operations.

### 4. Advanced Development
- **Custom APIs**: Calling external APIs from SPFX (e.g., Azure Functions or third-party services) and handling CORS.
- **Azure AD Authentication**: Using MSAL or ADAL for secure API calls.
- **Adaptive Cards**: Building dynamic UI for Microsoft Teams or Outlook integrations.
- **SPFX Extensions**: Building application customizers (e.g., headers/footers), field customizers, and command sets with real-world use cases.
- **Performance Optimization**: Lazy loading, code splitting, and minimizing bundle size.

### 5. Deployment and Packaging
- **Solution Packaging**: Creating `.sppkg` files, app catalog deployment, and tenant-wide vs. site-specific deployment.
- **ALM (Application Lifecycle Management)**: Using the ALM API to manage SPFX solutions programmatically.
- **CI/CD Pipelines**: Experience with Azure DevOps, GitHub Actions, or similar for SPFX builds and deployments.

### 6. Microsoft 365 Ecosystem
- **Teams Integration**: Developing SPFX web parts for Teams tabs or personal apps.
- **Viva Connections**: Extending SPFX solutions for Viva dashboards.
- **Power Platform**: Interaction between SPFX and Power Apps/Power Automate (e.g., embedding custom components).

### 7. Real-World Scenarios (Given Your Experience)
- **Migration**: Converting classic SharePoint solutions (e.g., add-ins or script editor web parts) to SPFX.
- **Troubleshooting**: Common issues like version mismatches, CORS errors, or deployment failures.
- **Customization**: Examples of complex business requirements you’ve solved with SPFX (e.g., multi-step forms, dashboards).
- **Security**: Handling permissions, securing APIs, and ensuring compliance with organizational policies.

### 8. Soft Skills and Experience-Based Questions
- **Project Examples**: Be ready to discuss specific SPFX projects you’ve worked on, challenges faced, and solutions implemented.
- **Collaboration**: Experience working with designers, backend devs, or stakeholders to deliver SPFX solutions.
- **Best Practices**: Code organization, reusability, and maintainability in SPFX projects.

### Preparation Tips
- **Brush Up**: Revisit the official SPFX documentation on Microsoft Learn to ensure you’re up-to-date with the latest features (as of April 2025).
- **Hands-On**: Build a small SPFX project (e.g., a web part with Graph API integration) to solidify your understanding.
- **Mock Questions**: Practice explaining concepts like “Why use SPFX over classic add-ins?” or “How would you optimize an SPFX solution for a large tenant?”

With 6 years of experience, interviewers will expect depth in your answers, so focus on practical examples and how you’ve applied these skills in real projects. Good luck with your interview! Let me know if you’d like to dive deeper into any specific topic.

---

# Detailed SPFX Notes with Mock Questions and Code Snippets

### 1. Core SPFX Concepts

#### Notes
- **SPFX Architecture**: SPFX is a client-side framework that runs in the browser, leveraging modern web technologies (TypeScript, React, etc.). It uses a modular approach with web parts and extensions, executed in the context of a SharePoint page.
- **Web Parts vs. Extensions**: Web parts are UI components, while extensions (application customizers, field customizers, command sets) enhance page functionality without a dedicated UI container.
- **TypeScript**: SPFX relies heavily on TypeScript for type safety and modern JavaScript features. You’ll need to understand interfaces, generics, and async patterns.
- **React**: Default UI framework in SPFX. Mastery of functional components and hooks is expected at your level.

#### Mock Questions
1. **What’s the difference between a web part and an application customizer in SPFX?**
   - *Answer*: A web part is a reusable UI component added to a page (e.g., a dashboard), while an application customizer injects custom code (e.g., a header/footer) across pages without a dedicated UI slot. Web parts use `render()`, while customizers use `onInit()` and placeholders.
2. **How would you type a custom props interface for an SPFX web part using TypeScript?**

#### Code Snippet
```typescript
// Custom props interface for a React-based SPFX web part
interface IMyWebPartProps {
  title: string;
  items: { id: number; name: string }[];
  onItemClick: (id: number) => void;
}

// Web part class
public render(): void {
  const element: React.ReactElement<IMyWebPartProps> = React.createElement(
    MyWebPartComponent,
    {
      title: this.properties.title,
      items: this.properties.items,
      onItemClick: this.handleItemClick.bind(this),
    }
  );
  ReactDom.render(element, this.domElement);
}
```

### 2. Development Environment and Tooling

#### Notes
- **Node.js/npm**: Manage SPFX dependencies (e.g., `@microsoft/sp-core-library`). Understand semver and how to resolve conflicts.
- **Yeoman**: The `yo @microsoft/sharepoint` generator scaffolds SPFX projects. Know how to customize the generated structure.
- **Gulp**: Tasks like `gulp serve` (local testing), `gulp bundle` (production build), and `gulp package-solution` (create `.sppkg`) are critical.
- **Webpack**: Bundles SPFX code into optimized JavaScript files. You should know how to tweak it for performance (e.g., externalizing libraries).

#### Mock Questions
1. **What happens when you run `gulp serve` in an SPFX project?**
   - *Answer*: It starts a local Node.js server, compiles the TypeScript code, bundles it with Webpack, and opens the SharePoint workbench (local or online) for testing.
2. **How would you debug an SPFX web part in production?**

#### Code Snippet
```bash
# Common Gulp commands
gulp serve --nobrowser  # Test locally without opening the browser
gulp bundle --ship      # Create production bundle
gulp package-solution --ship  # Generate .sppkg for deployment
```

### 3. SharePoint Integration

#### Notes
- **SharePoint REST API**: Use endpoints like `/_api/web/lists` for data operations. Batching improves performance for multiple calls.
- **Microsoft Graph API**: Access broader Microsoft 365 data (e.g., `/me` for user info). Requires AAD permissions and token handling.
- **PnPJS**: Simplifies API calls with a fluent syntax (e.g., `sp.web.lists.getByTitle("MyList").items`).

#### Mock Questions
1. **How do you fetch items from a SharePoint list in SPFX using PnPJS?**
2. **What’s the difference between using SharePoint REST API and Microsoft Graph API in SPFX?**

#### Code Snippet
```typescript
import { spfi, SPFI } from "@pnp/sp";
import "@pnp/sp/webs";
import "@pnp/sp/lists";
import "@pnp/sp/items";

// Initialize PnPJS with SPFX context
private _sp: SPFI;

public onInit(): Promise<void> {
  this._sp = spfi().using(SPFx(this.context));
  return Promise.resolve();
}

// Fetch list items
public async getListItems(): Promise<any[]> {
  const items = await this._sp.web.lists.getByTitle("Tasks").items.select("Title", "Id").top(10)();
  return items;
}
```

### 4. Advanced Development

#### Notes
- **Custom APIs**: Use `HttpClient` or `AadHttpClient` for external calls. Handle CORS via proxy or backend services like Azure Functions.
- **Azure AD Authentication**: Use MSAL.js for token acquisition (`AadHttpClient` simplifies this in SPFX).
- **Performance**: Lazy load components with `React.lazy` and split code with dynamic imports.

#### Mock Questions
1. **How would you call a secure Azure Function from an SPFX web part?**
2. **How do you optimize an SPFX solution with a large bundle size?**

#### Code Snippet
```typescript
// Calling an Azure Function with AAD token
public async callAzureFunction(): Promise<void> {
  const client = await this.context.aadHttpClientFactory.getClient("api://your-azure-function-app-id");
  const response = await client.get(
    "https://yourfunction.azurewebsites.net/api/data",
    AadHttpClient.configurations.v1
  );
  const data = await response.json();
  console.log(data);
}

// Lazy-loaded component
const LazyComponent = React.lazy(() => import("./HeavyComponent"));
```

### 5. Deployment and Packaging

#### Notes
- **Solution Packaging**: The `.sppkg` file is uploaded to the app catalog. Tenant-wide deployment requires feature XML tweaks.
- **ALM API**: Automate deployment with REST calls (e.g., `/_api/web/tenantappcatalog/AvailableApps`).
- **CI/CD**: Use YAML pipelines in Azure DevOps to build, bundle, and deploy SPFX solutions.

#### Mock Questions
1. **What steps are involved in deploying an SPFX solution to a tenant?**
   - *Answer*: Build (`gulp bundle --ship`), package (`gulp package-solution --ship`), upload `.sppkg` to the app catalog, and deploy (tenant-wide or site-specific).
2. **How would you automate SPFX deployment using GitHub Actions?**

#### Code Snippet
```yaml
# GitHub Actions workflow
name: Deploy SPFX Solution
on: [push]
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with: { node-version: '16' }
      - run: npm install
      - run: gulp bundle --ship
      - run: gulp package-solution --ship
      - name: Deploy to SharePoint
        run: |
          # Use PnP PowerShell or CLI to upload .sppkg
```

### 6. Microsoft 365 Ecosystem

#### Notes
- **Teams Integration**: SPFX web parts can be exported as Teams tabs or personal apps via the manifest.
- **Viva Connections**: Extend SPFX for Adaptive Card-based dashboards.

#### Mock Questions
1. **How do you make an SPFX web part available in Microsoft Teams?**
   - *Answer*: Enable the Teams manifest in `manifest.json` and sync it via the Developer Portal.

#### Code Snippet
```json
// manifest.json (Teams support)
{
  "id": "your-guid",
  "supportsTeams": true,
  "teamsAppId": "your-teams-app-id"
}
```

### 7. Real-World Scenarios

#### Notes
- **Migration**: Convert script editor web parts to SPFX by encapsulating logic in TypeScript/React.
- **Troubleshooting**: Common issues include Node version mismatches or Graph permission errors.

#### Mock Questions
1. **How would you migrate a classic SharePoint script editor web part to SPFX?**
2. **What’s a challenging SPFX issue you’ve faced and how did you resolve it?**

### Final Tips
- **Practice**: Build a sample SPFX project integrating Graph API and deploy it.
- **Explain**: Be ready to walk through code snippets and justify design decisions.
- **Stay Current**: Mention recent SPFX features (e.g., updates as of April 2025) to show you’re up-to-date.

---

# Azure Functions

#### Notes
- **Overview**: Azure Functions is a serverless compute service that lets you run event-driven code without managing infrastructure. It’s ideal for executing small, focused pieces of logic triggered by events (e.g., HTTP requests, timers, or SharePoint events via Graph API).
- **Triggers and Bindings**: Supports triggers (e.g., HTTP, Timer, Queue) and bindings (e.g., Blob, Cosmos DB) to simplify integration with Azure services.
- **Hosting Plans**:
  - **Consumption Plan**: Pay-per-execution, auto-scales, ideal for sporadic workloads.
  - **Premium Plan**: Pre-warmed instances for low latency, VNet support.
  - **App Service Plan**: Runs on dedicated VMs, predictable costs.
- **Durable Functions**: Extends Azure Functions for stateful workflows (e.g., orchestrations, fan-out/fan-in patterns).
- **SPFX Integration**: Commonly used as a backend for SPFX web parts to handle custom logic, API calls, or heavy processing outside SharePoint.
- **Languages**: Supports C#, JavaScript, Python, Java, PowerShell, etc.
- **Security**: Integrates with Azure AD for authentication, supports function keys, and can use managed identities.

#### Mock Questions
1. **How would you use Azure Functions as a backend for an SPFX web part?**
   - *Answer*: I’d create an HTTP-triggered Azure Function to handle requests from the SPFX web part, authenticate using Azure AD tokens via MSAL, and process data (e.g., querying a database or calling Graph API). The function returns JSON to the SPFX client. Deployment would use the Consumption Plan for cost efficiency unless low latency is critical, then Premium Plan.
2. **What’s the difference between a stateless and a Durable Function, and when would you use each in an SPFX context?**
   - *Answer*: Stateless functions execute once per trigger and don’t retain state (e.g., a simple API call). Durable Functions maintain state across multiple executions (e.g., a multi-step approval process). For SPFX, I’d use stateless for quick data retrieval and Durable for complex workflows like document approval.

#### Code Snippet
```csharp
// HTTP-triggered Azure Function for SPFX
using Microsoft.AspNetCore.Http;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using System.Threading.Tasks;

public static class SpfxBackendFunction
{
    [FunctionName("SpfxBackend")]
    public static async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Function, "get", Route = null)] HttpRequest req,
        ILogger log)
    {
        log.LogInformation("SPFX backend function triggered.");
        string name = req.Query["name"];
        return new OkObjectResult(new { message = $"Hello, {name} from Azure Functions!" });
    }
}
```
- **SPFX Call**:
```typescript
const response = await fetch(`${functionUrl}?name=User`, {
  headers: { Authorization: `Bearer ${token}` },
});
const data = await response.json();
```

---

# Azure Logic Apps

#### Notes
- **Overview**: Azure Logic Apps is a serverless workflow orchestration platform for automating business processes with a low/no-code visual designer. It excels at integrating disparate systems via connectors.
- **Types**:
  - **Consumption**: Multi-tenant, pay-per-action, simpler scaling.
  - **Standard**: Single-tenant, runs on Azure Functions runtime, supports VNet and custom code (e.g., .NET, JavaScript).
- **Connectors**: 400+ prebuilt connectors (e.g., SharePoint, Teams, SQL, Graph API) for rapid integration.
- **Triggers**: Starts with events (e.g., SharePoint item created, HTTP request, schedule).
- **Actions**: Steps like conditions, loops, or calling Azure Functions.
- **SPFX Integration**: Can orchestrate processes triggered by SPFX (e.g., a button click sends an HTTP request to a Logic App to start a workflow).
- **Use Cases**: Approval workflows, data sync between SharePoint and external systems, or notifications.
- **Security**: Supports Azure AD, managed identities, and IP restrictions.

#### Mock Questions
1. **How would you integrate an Azure Logic App with an SPFX web part to automate a document approval process?**
   - *Answer*: I’d expose the Logic App with an HTTP Request trigger, generating a URL with a SAS token. The SPFX web part sends a POST request to this URL with document metadata. The Logic App then uses the SharePoint connector to update the item status, sends an approval email via Office 365, and notifies the user via Teams once approved.
2. **What’s the advantage of using a Standard Logic App over a Consumption Logic App in an SPFX-integrated solution?**
   - *Answer*: Standard Logic Apps run on a single-tenant model with the Azure Functions runtime, offering VNet support, custom code execution (e.g., .NET), and better performance for complex workflows. For SPFX, this is useful if I need to integrate with on-premises systems or run custom logic not available in Consumption connectors.

#### Code Snippet
- **Logic App Workflow (JSON Definition)**:
```json
{
  "definition": {
    "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
    "triggers": {
      "manual": {
        "type": "Request",
        "kind": "Http",
        "inputs": {
          "method": "POST",
          "schema": {
            "type": "object",
            "properties": {
              "documentId": { "type": "string" },
              "userEmail": { "type": "string" }
            }
          }
        }
      }
    },
    "actions": {
      "UpdateSharePoint": {
        "type": "ApiConnection",
        "inputs": {
          "host": { "connection": "sharepointonline" },
          "method": "patch",
          "path": "/v1.0/sites/{siteId}/lists/{listId}/items/@{triggerBody()?['documentId']}",
          "body": { "fields": { "Status": "Pending" } }
        }
      }
    }
  }
}
```
- **SPFX Call**:
```typescript
const logicAppUrl = "https://prod-01.westus.logic.azure.com/workflows/...";
const payload = { documentId: "123", userEmail: "user@domain.com" };
await fetch(logicAppUrl, {
  method: "POST",
  body: JSON.stringify(payload),
  headers: { "Content-Type": "application/json" },
});
```

---

# PowerShell (SharePoint Context)

#### Notes
- **Overview**: PowerShell is a scripting language and automation framework used for SharePoint administration, configuration, and bulk operations. For SharePoint Online, it leverages the **SharePoint Online Management Shell** and **PnP PowerShell**, while SharePoint On-Premises uses the **SharePoint Server PowerShell module**.
- **Modules**:
  - **SharePoint Online Management Shell**: Official Microsoft module for tenant-level operations (e.g., site creation, app catalog management).
  - **PnP PowerShell**: Community-driven, feature-rich module for both Online and On-Premises, covering site, list, and SPFX operations.
  - **SharePoint Server**: On-Premises cmdlets like `Add-SPSite`, `Get-SPWeb`.
- **Authentication**: 
  - Online: Supports modern auth (Azure AD) with `Connect-SPOService` or `Connect-PnPOnline` (with `-Interactive`, `-ClientId`, or `-Certificate`).
  - On-Prem: Uses Windows credentials or farm account.
- **SPFX Context**: Deploy SPFX solutions, manage app catalogs, or automate tenant-wide settings.
- **Use Cases**: Provisioning sites, managing permissions, deploying SPFX packages, or migrating content.
- **Advanced Features**: Parallel execution with `ForEach-Object -Parallel`, error handling with `try/catch`, and integration with Azure AD or Microsoft Graph.

#### Mock Questions
1. **How would you deploy an SPFX solution to a SharePoint Online tenant using PowerShell?**
   - *Answer*: I’d use PnP PowerShell to connect to the tenant (`Connect-PnPOnline`), upload the `.sppkg` file to the app catalog (`Add-PnPApp`), publish it (`Publish-PnPApp`), and optionally install it to a site (`Install-PnPApp`). For tenant-wide deployment, I’d use `-Scope Tenant` with `Publish-PnPApp`.
2. **What’s the difference between `Connect-SPOService` and `Connect-PnPOnline`, and when would you use each?**
   - *Answer*: `Connect-SPOService` (SharePoint Online Management Shell) is for tenant-level admin tasks (e.g., site collection management), while `Connect-PnPOnline` (PnP PowerShell) offers broader functionality, including site-level operations, list management, and SPFX support. I’d use SPO for high-level admin tasks and PnP for development or site-specific automation.

#### Code Snippet
```powershell
# Deploy SPFX solution to SharePoint Online
$tenantUrl = "https://contoso-admin.sharepoint.com"
$siteUrl = "https://contoso.sharepoint.com/sites/DevSite"
$sppkgPath = "C:\Projects\my-solution.sppkg"

# Connect to tenant app catalog
Connect-PnPOnline -Url $tenantUrl -Interactive

# Add and publish the SPFX package
Add-PnPApp -Path $sppkgPath -Overwrite
$app = Get-PnPApp -Identity "my-solution-client-side-solution"
Publish-PnPApp -Identity $app.Id -Scope Tenant

# Install to a specific site
Connect-PnPOnline -Url $siteUrl -Interactive
Install-PnPApp -Identity $app.Id

Write-Host "SPFX solution deployed successfully!"
```

- **Bulk Site Creation**:
```powershell
$sites = Import-Csv -Path "sites.csv" # Columns: Title, Url
Connect-PnPOnline -Url $tenantUrl -Interactive
$sites | ForEach-Object -Parallel {
    New-PnPSite -Type TeamSite -Title $_.Title -Url $_.Url -Owner "admin@contoso.com"
}
```

---

# C# (SharePoint Context)

#### Notes
- **Overview**: C# is used for SharePoint development via the **Client-Side Object Model (CSOM)**, **Server-Side Object Model (SSOM)** (On-Prem only), or **REST API** integrations. It’s common in SPFX backend development (e.g., Azure Functions) or custom SharePoint add-ins.
- **CSOM**: Preferred for SharePoint Online and modern development. Uses `Microsoft.SharePoint.Client` namespace for remote operations.
- **SSOM**: Legacy approach for On-Premises, running directly on the server with `Microsoft.SharePoint` namespace.
- **REST API**: Lightweight alternative to CSOM, often wrapped in C# for HTTP requests.
- **SPFX Context**: C# isn’t directly used in SPFX client-side code (TypeScript/React), but it powers backend services (e.g., Azure Functions) or legacy migrations to SPFX.
- **Authentication**: 
  - Online: Uses Azure AD tokens (via MSAL.NET or `SharePointOnlineCredentials` for legacy).
  - On-Prem: Windows auth or farm credentials.
- **Use Cases**: Custom workflows, timer jobs (On-Prem), Azure Function backends for SPFX, or content migrations.
- **Advanced**: Batch operations with `ClientContext.ExecuteQuery()`, exception handling, and performance optimization (e.g., minimizing round-trips).

#### Mock Questions
1. **How would you retrieve all items from a SharePoint Online list using C# and CSOM?**
   - *Answer*: I’d use CSOM to create a `ClientContext`, query the list with a `CamlQuery`, and load items in batches using `ListItemCollectionPosition` to handle large lists efficiently. I’d authenticate with an Azure AD token via MSAL.NET.
2. **How would you integrate a C#-based Azure Function with an SPFX web part to update a SharePoint list?**
   - *Answer*: I’d write an HTTP-triggered Azure Function in C# using CSOM to update the list. The SPFX web part would call it with a JWT token (Azure AD) passed in the header, which the function validates before processing the request.

#### Code Snippet
- **CSOM: Fetch List Items**:
```csharp
using Microsoft.SharePoint.Client;
using Microsoft.Identity.Client; // For Azure AD auth

public async Task<ListItemCollection> GetListItemsAsync(string siteUrl, string listTitle)
{
    // Azure AD auth (replace with your app registration details)
    var app = ConfidentialClientApplicationBuilder
        .Create("your-client-id")
        .WithClientSecret("your-client-secret")
        .WithAuthority("https://login.microsoftonline.com/your-tenant-id")
        .Build();
    var token = await app.AcquireTokenForClient(new[] { "https://contoso.sharepoint.com/.default" }).ExecuteAsync();

    using (var context = new ClientContext(siteUrl))
    {
        context.ExecutingWebRequest += (s, e) =>
            e.WebRequestExecutor.RequestHeaders["Authorization"] = "Bearer " + token.AccessToken;

        var list = context.Web.Lists.GetByTitle(listTitle);
        var query = new CamlQuery { ViewXml = "<View><RowLimit>100</RowLimit></View>" };
        var items = list.GetItems(query);
        context.Load(items);
        await context.ExecuteQueryAsync();
        return items;
    }
}
```

- **Azure Function with CSOM**:
```csharp
using Microsoft.Azure.WebJobs;
using Microsoft.SharePoint.Client;

[FunctionName("UpdateSharePointList")]
public static async Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Function, "post", Route = null)] HttpRequest req,
    ILogger log)
{
    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    dynamic data = JsonConvert.DeserializeObject(requestBody);
    string siteUrl = data.siteUrl;
    string listTitle = data.listTitle;
    int itemId = data.itemId;

    using (var context = new ClientContext(siteUrl))
    {
        // Assume token passed in header or retrieved via managed identity
        context.Credentials = new SharePointOnlineCredentials("user@contoso.com", GetSecureStringPassword());

        var list = context.Web.Lists.GetByTitle(listTitle);
        var item = list.GetItemById(itemId);
        item["Title"] = "Updated via Azure Function";
        item.Update();
        await context.ExecuteQueryAsync();

        return new OkObjectResult("Item updated successfully");
    }
}
```

---

# React (SPFX Context)

#### Notes
- **Overview**: React is a JavaScript library for building user interfaces, heavily used in SPFX for rendering web parts and extensions. SPFX scaffolds projects with React by default (since v1.4), leveraging its component-based architecture.
- **Key Concepts**:
  - **Components**: Functional components are standard in modern SPFX (class components are legacy).
  - **Hooks**: `useState`, `useEffect`, `useContext` for state management and side effects.
  - **Props**: Pass data from the SPFX web part class to the React component.
  - **Rendering**: SPFX uses `ReactDom.render` to mount components in the DOM.
- **SPFX Integration**: The SPFX web part class (`render()`) creates a React element, passing properties and context to the component.
- **Advanced Features**:
  - **Lazy Loading**: Use `React.lazy` and `Suspense` for performance optimization.
  - **Context API**: Share SPFX context (e.g., `this.context`) across components.
  - **Custom Hooks**: Encapsulate reusable logic (e.g., fetching SharePoint data).
- **Libraries**: Common in SPFX—`@fluentui/react` (Microsoft’s UI toolkit), `react-router` (for navigation), or `redux` (for state management in complex solutions).
- **Use Cases**: Dynamic forms, dashboards, or interactive UI in SPFX web parts.

#### Mock Questions
1. **How do you pass SharePoint context from an SPFX web part to a React component?**
   - *Answer*: In the web part’s `render()` method, I pass `this.context` as a prop to the React component. The component can then use it to access SharePoint services like `spHttpClient` or `msGraphClientFactory`.
2. **How would you optimize a React-based SPFX web part with a large dataset?**
   - *Answer*: I’d use `React.memo` to prevent unnecessary re-renders, implement lazy loading with `React.lazy` for heavy components, and paginate data fetching with SharePoint REST API or PnPJS to minimize DOM updates.

#### Code Snippet
```jsx
// MyWebPart.ts (SPFX Web Part)
import * as React from 'react';
import * as ReactDom from 'react-dom';
import { IMyWebPartProps } from './IMyWebPartProps';
import MyComponent from './components/MyComponent';

export default class MyWebPart extends BaseClientSideWebPart<IMyWebPartProps> {
  public render(): void {
    const element: React.ReactElement<IMyWebPartProps> = React.createElement(
      MyComponent,
      {
        spContext: this.context, // Pass SharePoint context
        description: this.properties.description
      }
    );
    ReactDom.render(element, this.domElement);
  }
}

// MyComponent.tsx (React Component)
import * as React from 'react';
import { useEffect, useState } from 'react';
import { WebPartContext } from '@microsoft/sp-webpart-base';

interface IMyComponentProps {
  spContext: WebPartContext;
  description: string;
}

const MyComponent: React.FC<IMyComponentProps> = ({ spContext, description }) => {
  const [items, setItems] = useState<string[]>([]);

  useEffect(() => {
    const fetchData = async () => {
      const response = await spContext.spHttpClient.get(
        `${spContext.pageContext.web.absoluteUrl}/_api/web/lists/getbytitle('Tasks')/items`,
        SPHttpClient.configurations.v1
      );
      const data = await response.json();
      setItems(data.value.map((item: any) => item.Title));
    };
    fetchData();
  }, [spContext]);

  return (
    <div>
      <h1>{description}</h1>
      <ul>{items.map((item, idx) => <li key={idx}>{item}</li>)}</ul>
    </div>
  );
};

export default MyComponent;
```

---

# TypeScript (SPFX Context)

#### Notes
- **Overview**: TypeScript is a superset of JavaScript that adds static typing, used in SPFX for type-safe development. All SPFX projects are written in TypeScript since v1.0.
- **Key Features**:
  - **Interfaces**: Define prop and state shapes (e.g., `IMyWebPartProps`).
  - **Generics**: Create reusable, type-safe utilities (e.g., fetching typed data).
  - **Union/Intersection Types**: Handle complex scenarios (e.g., optional properties or combined types).
  - **Async/Await**: Standard for API calls in SPFX, typed with `Promise<T>`.
- **SPFX Integration**: TypeScript defines the web part’s property pane (`IPropertyPaneConfiguration`), enforces type safety for SharePoint API responses, and integrates with React via `.tsx` files.
- **Advanced Features**:
  - **Decorators**: Used in SPFX for metadata (less common now with functional components).
  - **Type Assertions**: Handle dynamic data from SharePoint APIs.
  - **Utility Types**: Use `Partial`, `Pick`, or `Omit` for flexible prop management.
- **Tooling**: SPFX uses `tsconfig.json` for compilation, with strict mode enabled by default.
- **Use Cases**: Type-safe SharePoint data models, reusable utilities, or typed PnPJS calls.

#### Mock Questions
1. **How do you define a TypeScript interface for an SPFX web part’s properties?**
   - *Answer*: I’d create an interface in a separate file (e.g., `IMyWebPartProps.ts`) with typed fields for the web part’s properties, including optional ones with `?`. For example, `description: string` and `itemCount?: number`. This is passed to the React component and property pane.
2. **How would you type a custom hook in TypeScript for fetching SharePoint list items in SPFX?**
   - *Answer*: I’d define an interface for the list item shape (e.g., `ITask`), then create a typed hook returning the data and loading state, using generics to keep it reusable across lists.

#### Code Snippet
```typescript
// IMyWebPartProps.ts
export interface IMyWebPartProps {
  description: string;
  itemCount?: number;
  spContext: WebPartContext;
}

// Custom Hook: useSharePointList.ts
import { useState, useEffect } from 'react';
import { WebPartContext } from '@microsoft/sp-webpart-base';

interface IListItem {
  Id: number;
  Title: string;
}

export function useSharePointList<T extends IListItem>(context: WebPartContext, listName: string) {
  const [items, setItems] = useState<T[]>([]);
  const [loading, setLoading] = useState<boolean>(true);

  useEffect(() => {
    const fetchItems = async () => {
      const response = await context.spHttpClient.get(
        `${context.pageContext.web.absoluteUrl}/_api/web/lists/getbytitle('${listName}')/items`,
        SPHttpClient.configurations.v1
      );
      const data = await response.json();
      setItems(data.value as T[]);
      setLoading(false);
    };
    fetchItems();
  }, [context, listName]);

  return { items, loading };
}

// Usage in Component
const MyComponent: React.FC<IMyWebPartProps> = ({ spContext }) => {
  const { items, loading } = useSharePointList<{ Id: number; Title: string; Status: string }>(spContext, "Tasks");

  if (loading) return <div>Loading...</div>;
  return <ul>{items.map(item => <li key={item.Id}>{item.Title} - {item.Status}</li>)}</ul>;
};
```

---

# React (Frontend Context)

#### Notes
- **Overview**: React is a JavaScript library for building user interfaces with a component-based architecture, leveraging a virtual DOM for efficient updates.
- **Core Concepts**:
  - **Functional Components**: Preferred over class components, using hooks for state and side effects.
  - **Hooks**:
    - `useState`: Manage local component state.
    - `useEffect`: Handle side effects (e.g., API calls, subscriptions).
    - `useContext`: Share global state without prop drilling.
    - `useReducer`: Manage complex state logic.
    - `useMemo`/`useCallback`: Optimize performance by memoizing values or functions.
  - **Props**: Pass data and callbacks between components.
  - **JSX**: Syntax for writing HTML-like code in JavaScript, compiled to `React.createElement`.
- **Advanced Features**:
  - **Lazy Loading**: Use `React.lazy` and `Suspense` to load components on demand.
  - **Error Boundaries**: Catch JavaScript errors in component trees with a class component or libraries like `react-error-boundary`.
  - **Portals**: Render components outside the parent DOM hierarchy (e.g., modals).
  - **Concurrent Rendering**: Features like `startTransition` (React 18) for smoother updates.
- **State Management**:
  - Local: `useState` or `useReducer`.
  - Global: Context API (simple apps), Redux, Zustand, or Recoil (complex apps).
- **Performance Optimization**: Avoid unnecessary re-renders with `React.memo`, memoize expensive computations, and split code with dynamic imports.
- **Common Libraries**: `react-router-dom` (routing), `axios` (API calls), `styled-components` (CSS-in-JS), `@mui/material` (UI components).
- **Use Cases**: Single-page applications (SPAs), dashboards, forms, or real-time UIs.

#### Mock Questions
1. **How do you prevent a React component from re-rendering unnecessarily?**
   - *Answer*: I’d use `React.memo` to memoize the component if its props don’t change, and `useMemo` or `useCallback` to memoize expensive calculations or callbacks passed to children. For example, wrapping a list item component in `React.memo` ensures it only re-renders when its props update.
2. **How would you implement a custom hook for fetching data from an API?**
   - *Answer*: I’d create a hook that uses `useState` for data/loading/error states and `useEffect` to fetch data, with cleanup to prevent memory leaks. It would accept a URL and options as parameters for reusability.

#### Code Snippet
```jsx
// Custom Hook: useFetch.js
import { useState, useEffect } from 'react';

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    let isMounted = true;
    const fetchData = async () => {
      try {
        setLoading(true);
        const response = await fetch(url);
        const result = await response.json();
        if (isMounted) setData(result);
      } catch (err) {
        if (isMounted) setError(err.message);
      } finally {
        if (isMounted) setLoading(false);
      }
    };
    fetchData();
    return () => { isMounted = false; }; // Cleanup
  }, [url]);

  return { data, loading, error };
}

// Component: UserList.jsx
import React from 'react';

const UserList = () => {
  const { data, loading, error } = useFetch('https://api.example.com/users');

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;

  return (
    <ul>
      {data.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
};

export default UserList;
```

---

# TypeScript (Frontend Context)

#### Notes
- **Overview**: TypeScript enhances JavaScript with static typing, improving code quality, scalability, and developer experience in frontend projects.
- **Key Features**:
  - **Interfaces vs. Types**: Use interfaces for object shapes (extendable) and types for unions or primitives.
  - **Generics**: Write reusable, type-safe functions/components (e.g., a generic list component).
  - **Union/Intersection Types**: Combine types for flexibility (e.g., `string | number`, `TypeA & TypeB`).
  - **Type Assertions**: Handle unknown data (e.g., `as` keyword) with caution.
  - **Utility Types**: `Partial`, `Pick`, `Omit`, `Record` for manipulating types.
  - **Enums**: Define named constants (e.g., `enum Status { Active, Inactive }`).
- **React Integration**:
  - Define prop types with interfaces (e.g., `React.FC<Props>`).
  - Type events (e.g., `React.ChangeEvent<HTMLInputElement>`).
  - Type hooks (e.g., `useState<string>`).
- **Advanced Features**:
  - **Conditional Types**: Use `T extends U ? X : Y` for dynamic typing.
  - **Mapped Types**: Transform object properties (e.g., making all fields optional).
  - **Module Augmentation**: Extend third-party library types (e.g., adding custom props to a library component).
- **Tooling**: Configured via `tsconfig.json` (e.g., `strict` mode, `esModuleInterop`).
- **Use Cases**: Type-safe APIs, component libraries, or large-scale React apps.

#### Mock Questions
1. **How do you type a React component’s props with TypeScript?**
   - *Answer*: I’d define an interface for the props (e.g., `interface MyProps { name: string; age?: number }`) and use it with `React.FC<MyProps>` or as a parameter type in a functional component. I’d prefer the latter for flexibility and to avoid implicit `children` typing.
2. **How would you create a generic TypeScript function to handle different data types in a React app?**
   - *Answer*: I’d use generics to define a function that accepts a type parameter (e.g., `<T>`), ensuring type safety while remaining reusable. For example, a function to filter an array of any type based on a condition.

#### Code Snippet
```typescript
// Generic Component: List.tsx
import React from 'react';

interface ListProps<T> {
  items: T[];
  renderItem: (item: T) => React.ReactNode;
}

function List<T>({ items, renderItem }: ListProps<T>) {
  return (
    <ul>
      {items.map((item, index) => (
        <li key={index}>{renderItem(item)}</li>
      ))}
    </ul>
  );
}

// Usage
interface User {
  id: number;
  name: string;
}

const UserList: React.FC = () => {
  const users: User[] = [
    { id: 1, name: "Alice" },
    { id: 2, name: "Bob" },
  ];

  return <List<User> items={users} renderItem={user => `${user.name} (ID: ${user.id})`} />;
};

export default UserList;

// Typed Custom Hook
function useToggle(initialValue: boolean = false): [boolean, () => void] {
  const [value, setValue] = React.useState<boolean>(initialValue);
  const toggle = () => setValue(prev => !prev);
  return [value, toggle];
}

// Usage
const ToggleButton: React.FC = () => {
  const [isOn, toggle] = useToggle();
  return <button onClick={toggle}>{isOn ? 'ON' : 'OFF'}</button>;
};
```

---

# JavaScript Topics

### 1. Core JavaScript Concepts

#### Notes
- **Variables**: 
  - `var` (function-scoped, hoisted), `let` (block-scoped), `const` (block-scoped, immutable reference).
  - Hoisting: Variable and function declarations are moved to the top of their scope.
- **Scope**: Global, function, block (with `let`/`const`), and lexical scoping (closures).
- **Closures**: Functions that retain access to their outer scope’s variables even after the outer function has finished executing.
- **this Keyword**: Context-dependent—refers to the object calling the function (affected by `call`, `apply`, `bind`, or arrow functions).
- **Prototypes**: Basis of inheritance in JS—objects inherit properties/methods via the prototype chain (`__proto__`, `prototype`).
- **Event Loop**: Manages asynchronous execution—call stack, callback queue, and microtask queue (Promises).

#### Mock Questions
1. **What’s the difference between `var`, `let`, and `const`?**
   - *Answer*: `var` is function-scoped and hoisted with `undefined`, allowing redeclaration. `let` is block-scoped, hoisted but not initialized (temporal dead zone), and prevents redeclaration. `const` is block-scoped, must be initialized, and prevents reassignment (though object properties can mutate).
2. **How does a closure work, and can you give an example?**
   - *Answer*: A closure is a function that remembers its outer scope’s variables. For example, a counter function that retains its count variable across calls.

#### Code Snippet
```javascript
// Closure Example
function createCounter() {
  let count = 0;
  return function() {
    return ++count;
  };
}
const counter = createCounter();
console.log(counter()); // 1
console.log(counter()); // 2

// this Binding
const obj = {
  name: "Alice",
  sayName: function() { console.log(this.name); }
};
obj.sayName(); // "Alice"
const unbound = obj.sayName;
unbound(); // undefined (or error in strict mode)
const bound = obj.sayName.bind(obj);
bound(); // "Alice"
```

### 2. ES6+ Features

#### Notes
- **Arrow Functions**: Concise syntax, no own `this` binding (lexically scoped).
- **Destructuring**: Extract values from arrays/objects (e.g., `const { name } = person`).
- **Template Literals**: String interpolation with backticks (e.g., ``Hello, ${name}!``).
- **Spread/Rest Operators**: `...` for copying/spreading arrays/objects or gathering parameters.
- **Promises**: Handle asynchronous operations with `.then()`, `.catch()`, and `async/await`.
- **Modules**: `import`/`export` for modular code (e.g., `export default function`).
- **Optional Chaining**: `?.` to safely access nested properties (e.g., `obj?.prop?.value`).
- **Nullish Coalescing**: `??` for default values when `null`/`undefined` (e.g., `value ?? "default"`).

#### Mock Questions
1. **What’s the difference between a regular function and an arrow function?**
   - *Answer*: Regular functions have their own `this` binding, determined by how they’re called, while arrow functions inherit `this` from their enclosing scope. Arrow functions are also more concise and can’t be used as constructors.
2. **How would you rewrite a Promise-based function using async/await?**
   - *Answer*: I’d convert `.then()` chains to `await` calls within an `async` function, adding try/catch for error handling.

#### Code Snippet
```javascript
// Destructuring and Arrow Function
const person = { name: "Bob", age: 30 };
const { name, age } = person;
const greet = ({ name }) => `Hello, ${name}!`;
console.log(greet(person)); // "Hello, Bob!"

// Promise with async/await
function fetchData() {
  return new Promise((resolve) => setTimeout(() => resolve("Data"), 1000));
}

async function getData() {
  try {
    const data = await fetchData();
    console.log(data); // "Data"
  } catch (error) {
    console.error("Error:", error);
  }
}
getData();
```

### 3. Asynchronous JavaScript

#### Notes
- **Callbacks**: Functions passed as arguments to handle async results (prone to callback hell).
- **Promises**: Objects representing eventual completion/failure, chaining with `.then()`/`.catch()`.
- **Async/Await**: Syntactic sugar over Promises for cleaner async code.
- **Fetch API**: Modern replacement for XMLHttpRequest, returns Promises for HTTP requests.
- **Event Loop Details**: 
  - Macrotasks: `setTimeout`, `setInterval`.
  - Microtasks: Promises, `queueMicrotask`.
- **Error Handling**: Use `try/catch` with `async/await` or `.catch()` with Promises.

#### Mock Questions
1. **How does the event loop handle Promises vs. setTimeout?**
   - *Answer*: The event loop prioritizes microtasks (Promises) over macrotasks (`setTimeout`). After the call stack clears, microtasks run before the next macrotask, ensuring Promises resolve before timers.
2. **How would you handle multiple API calls concurrently?**
   - *Answer*: I’d use `Promise.all()` to run them in parallel and await all results, handling errors with a single `.catch()` or try/catch.

#### Code Snippet
```javascript
// Promise.all for Concurrent API Calls
async function fetchMultiple() {
  try {
    const [users, posts] = await Promise.all([
      fetch("https://api.example.com/users").then(res => res.json()),
      fetch("https://api.example.com/posts").then(res => res.json())
    ]);
    console.log(users, posts);
  } catch (error) {
    console.error("Failed to fetch:", error);
  }
}
fetchMultiple();

// Event Loop Example
console.log("Start");
setTimeout(() => console.log("Timeout"), 0);
Promise.resolve().then(() => console.log("Promise"));
console.log("End");
// Output: Start, End, Promise, Timeout
```

### 4. Objects and Arrays

#### Notes
- **Object Methods**: `Object.keys()`, `Object.values()`, `Object.entries()` for iteration; `Object.assign()` for shallow copying.
- **Array Methods**: 
  - Transformative: `map`, `filter`, `reduce`.
  - Iterative: `forEach`, `some`, `every`.
  - Modern: `flat()`, `flatMap()`, `at()` (ES2022).
- **Immutability**: Use spread operator or libraries like `immer` to avoid mutating state.
- **Property Descriptors**: `Object.defineProperty` for getters/setters or controlling writability.

#### Mock Questions
1. **How would you remove duplicates from an array in JavaScript?**
   - *Answer*: I’d use `Set` for a concise solution (`[...new Set(array)]`) or `filter` with `indexOf` for more control, depending on whether order or performance matters.
2. **What’s the difference between `map` and `forEach`?**
   - *Answer*: `map` returns a new array with transformed values, while `forEach` executes a function for each element without returning anything.

#### Code Snippet
```javascript
// Remove Duplicates
const arr = [1, 2, 2, 3, 3, 4];
const unique = [...new Set(arr)]; // [1, 2, 3, 4]

// Reduce Example
const sum = arr.reduce((acc, curr) => acc + curr, 0); // 15

// Object.assign
const obj1 = { a: 1 };
const obj2 = Object.assign({}, obj1, { b: 2 }); // { a: 1, b: 2 }
```

### 5. Functional Programming

#### Notes
- **Pure Functions**: Same input, same output, no side effects.
- **Higher-Order Functions**: Functions that take or return functions (e.g., `map`, custom wrappers).
- **Immutability**: Avoid mutating data directly (e.g., use `concat` instead of `push`).
- **Currying**: Transform a function with multiple arguments into a chain of single-argument functions.
- **Composition**: Combine functions for reusable logic (e.g., `pipe` or `compose`).

#### Mock Questions
1. **What makes a function pure, and why is it useful?**
   - *Answer*: A pure function has no side effects and always returns the same output for the same input. It’s useful for predictability, testing, and avoiding bugs in stateful apps.
2. **How would you implement function composition in JavaScript?**
   - *Answer*: I’d create a `pipe` function that applies functions from left to right, passing the output of one as the input to the next.

#### Code Snippet
```javascript
// Pure Function
const add = (a, b) => a + b;

// Function Composition (Pipe)
const pipe = (...fns) => x => fns.reduce((v, f) => f(v), x);
const double = x => x * 2;
const addTen = x => x + 10;
const transform = pipe(double, addTen);
console.log(transform(5)); // 20 (5 * 2 + 10)
```

---

# REST API Concepts

#### Notes
- **Overview**: REST (Representational State Transfer) is an architectural style for designing networked applications, relying on stateless, client-server communication, typically over HTTP.
- **Core Principles**:
  - **Stateless**: Each request contains all the information needed; no server-side session state.
  - **Client-Server**: Separation of UI (client) and data storage/processing (server).
  - **Uniform Interface**: Consistent conventions (e.g., resources, HTTP methods).
  - **Resources**: Identified by URIs (e.g., `/users`, `/users/123`), represented in formats like JSON or XML.
  - **HTTP Methods**:
    - `GET`: Retrieve data (idempotent).
    - `POST`: Create a resource.
    - `PUT`: Update a resource (idempotent).
    - `DELETE`: Remove a resource (idempotent).
    - `PATCH`: Partially update a resource.
- **Status Codes**:
  - `200 OK`: Success.
  - `201 Created`: Resource created.
  - `400 Bad Request`: Invalid input.
  - `401 Unauthorized`: Authentication failed.
  - `403 Forbidden`: Access denied.
  - `404 Not Found`: Resource not found.
  - `500 Internal Server Error`: Server-side issue.
- **Headers**: Metadata for requests/responses (e.g., `Content-Type: application/json`, `Authorization: Bearer <token>`).
- **HATEOAS** (Hypermedia as the Engine of Application State): Responses include links to related resources (less common in practice).
- **Caching**: Use headers like `ETag` or `Cache-Control` to optimize performance.
- **Versioning**: Handle API evolution (e.g., `/v1/users`, `Accept: application/vnd.api.v1+json`).
- **Security**: HTTPS for encryption, OAuth/JWT for authentication, rate limiting for abuse prevention.
- **Use Cases**: Fetching data in SPFX, integrating with Microsoft Graph, or building CRUD apps.

#### Mock Questions
1. **What makes an API RESTful, and how does it differ from SOAP?**
   - *Answer*: A RESTful API follows REST principles—stateless, resource-based, and uses HTTP methods. It’s lightweight and typically JSON-based. SOAP is a protocol, stateful, XML-based, and more rigid, with built-in standards like WS-Security.
2. **How would you design a REST API endpoint for managing a list of tasks?**
   - *Answer*: I’d use `/tasks` for the collection: `GET /tasks` (list all), `POST /tasks` (create), `GET /tasks/{id}` (retrieve one), `PUT /tasks/{id}` (update), `DELETE /tasks/{id}` (delete). I’d return JSON with status codes like `201` for creation and include pagination (e.g., `?limit=10&offset=20`).

#### Code Snippet
```javascript
// Fetching tasks from a REST API
async function getTasks() {
  try {
    const response = await fetch('https://api.example.com/tasks', {
      method: 'GET',
      headers: {
        'Authorization': 'Bearer <token>',
        'Content-Type': 'application/json',
      },
    });
    if (!response.ok) throw new Error(`HTTP error! Status: ${response.status}`);
    const tasks = await response.json();
    console.log(tasks);
  } catch (error) {
    console.error('Fetch failed:', error);
  }
}

// Creating a task
async function createTask(taskData) {
  const response = await fetch('https://api.example.com/tasks', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': 'Bearer <token>',
    },
    body: JSON.stringify(taskData),
  });
  return await response.json(); // { id: 1, title: "New Task", ... }
}
```

---

# OAuth Concepts

#### Notes
- **Overview**: OAuth is an authorization framework that allows third-party applications to access resources on behalf of a user without sharing credentials. OAuth 2.0 is the most widely used version.
- **Key Components**:
  - **Resource Owner**: The user who owns the data (e.g., you).
  - **Client**: The app requesting access (e.g., an SPFX web part).
  - **Authorization Server**: Issues access tokens (e.g., Azure AD).
  - **Resource Server**: Hosts the protected resources (e.g., Microsoft Graph API).
- **Tokens**:
  - **Access Token**: Short-lived token to access resources (e.g., Bearer token in HTTP header).
  - **Refresh Token**: Long-lived token to obtain new access tokens without re-authentication.
  - **ID Token**: (OpenID Connect) JWT with user info.
- **Flows (Grant Types)**:
  - **Authorization Code**: For server-side apps; user logs in, client gets a code, exchanges it for tokens.
  - **Implicit**: For client-side apps (e.g., SPAs); returns access token directly (less secure, deprecated in OAuth 2.1).
  - **Client Credentials**: For app-to-app communication (no user involved).
  - **Resource Owner Password**: Direct username/password (rare, insecure).
  - **Device Code**: For devices without browsers (e.g., TVs).
- **Scopes**: Define permissions (e.g., `User.Read`, `Mail.Send` in Microsoft Graph).
- **Security**: Uses HTTPS, token expiration, and PKCE (Proof Key for Code Exchange) for public clients.
- **OAuth in SPFX**: Used with Azure AD to call Microsoft Graph or custom APIs via `AadHttpClient` or MSAL.js.
- **JWT**: Access/ID tokens are often JSON Web Tokens, signed for integrity (header, payload, signature).

#### Mock Questions
1. **What’s the difference between the Authorization Code flow and the Implicit flow in OAuth 2.0?**
   - *Answer*: Authorization Code flow is for server-side apps, involving a code exchange step for security, while Implicit flow is for client-side apps (e.g., SPAs), returning the access token directly in the URL. Authorization Code with PKCE is now preferred for SPAs due to security concerns with Implicit.
2. **How would you implement OAuth 2.0 in a frontend app to call a protected API?**
   - *Answer*: I’d use a library like MSAL.js or Auth0. The app redirects the user to the auth server’s login page (Authorization Code flow), retrieves a code, exchanges it for an access token via a backend or PKCE, and includes the token in API requests (`Authorization: Bearer <token>`).

#### Code Snippet
```javascript
// Using MSAL.js for OAuth in a frontend app
import * as msal from '@azure/msal-browser';

const msalConfig = {
  auth: {
    clientId: 'your-client-id',
    authority: 'https://login.microsoftonline.com/your-tenant-id',
    redirectUri: 'http://localhost:3000',
  },
};

const msalInstance = new msal.PublicClientApplication(msalConfig);

async function login() {
  try {
    const loginResponse = await msalInstance.loginPopup({
      scopes: ['User.Read'], // Microsoft Graph scope
    });
    const accessToken = loginResponse.accessToken;
    console.log('Access Token:', accessToken);
    return accessToken;
  } catch (error) {
    console.error('Login failed:', error);
  }
}

async function callApi(token) {
  const response = await fetch('https://graph.microsoft.com/v1.0/me', {
    headers: {
      Authorization: `Bearer ${token}`,
    },
  });
  const user = await response.json();
  console.log('User:', user);
}

login().then(token => callApi(token));
```

- **Backend Token Exchange (Authorization Code Flow)**:
```javascript
// Node.js example (simplified)
const axios = require('axios');

async function exchangeCodeForToken(code) {
  const response = await axios.post('https://login.microsoftonline.com/your-tenant-id/oauth2/v2.0/token', {
    client_id: 'your-client-id',
    client_secret: 'your-client-secret',
    code: code,
    redirect_uri: 'http://localhost:3000',
    grant_type: 'authorization_code',
  });
  return response.data.access_token;
}
```

---

# PowerApps

#### Notes
- **Overview**: PowerApps is a low-code/no-code platform within Microsoft Power Platform for building custom business applications. It enables rapid development of apps with a visual interface, connecting to 400+ data sources (e.g., SharePoint, Dataverse, SQL Server).
- **Types of Apps**:
  - **Canvas Apps**: Drag-and-drop UI design with high customization, ideal for tailored user experiences. Logic is defined using Power Fx (Excel-like formulas).
  - **Model-Driven Apps**: Data-first approach, auto-generates UI based on Dataverse schemas, suited for complex business processes.
  - **Portals**: External-facing websites built on Dataverse, with authentication and data access for external users.
- **Key Components**:
  - **Screens**: Containers for UI elements (e.g., galleries, forms).
  - **Controls**: UI elements like buttons, text inputs, galleries.
  - **Collections**: In-memory data tables for temporary storage.
  - **Connectors**: Link to external data/services (e.g., SharePoint, Graph API).
- **Dataverse**: The underlying data platform (formerly Common Data Service) for structured storage, with entities, relationships, and security roles.
- **Power Fx**: A low-code language for logic (e.g., `If`, `Filter`, `Patch`), inspired by Excel.
- **SPFX Integration**: PowerApps can be embedded in SharePoint via custom web parts or called from SPFX solutions using connectors.
- **Security**: Role-based access via Dataverse, Azure AD integration, and data loss prevention (DLP) policies.
- **Advanced Features**:
  - **Components**: Reusable UI/logic blocks across apps.
  - **AI Builder**: Adds AI capabilities (e.g., sentiment analysis, object detection).
  - **Delegation**: Offloads data processing to the source for performance (e.g., SharePoint supports up to 2,000 items by default).

#### Mock Questions
1. **What’s the difference between Canvas Apps and Model-Driven Apps, and when would you choose one over the other?**
   - *Answer*: Canvas Apps offer flexible UI design with drag-and-drop controls, ideal for custom layouts and connecting multiple data sources. Model-Driven Apps focus on data structure, auto-generating responsive UIs from Dataverse, perfect for structured processes like CRM. I’d choose Canvas for a custom task tracker and Model-Driven for an approval system with complex workflows.
2. **How would you optimize a PowerApps app handling a large SharePoint list?**
   - *Answer*: I’d use delegation-friendly functions like `Filter` and `Search` with SharePoint, limit retrieved items to 2,000 (configurable up to 2,000 in settings), and implement pagination with `First`/`Last` or a gallery. I’d also cache data in collections for offline use with `Collect`.

#### Code Snippet
```powerapps
// Fetch and filter SharePoint list items (Canvas App)
Set(
  TaskCollection,
  Filter(
    'Tasks', // SharePoint list name
    Status = "Pending" // Delegable filter
  )
);

// Update a SharePoint item with Patch
Patch(
  'Tasks',
  LookUp('Tasks', ID = 1),
  { Title: "Updated Task", Status: "Completed" }
);

// Navigate to another screen
Navigate(Screen2, ScreenTransition.Fade);
```

---

# Power Automate

#### Notes
- **Overview**: Power Automate (formerly Microsoft Flow) is a workflow automation tool within Power Platform, enabling process automation across apps and services with a low-code interface.
- **Flow Types**:
  - **Automated Flows**: Triggered by events (e.g., SharePoint item created).
  - **Instant Flows**: Manually triggered (e.g., button click in PowerApps).
  - **Scheduled Flows**: Run on a timetable (e.g., daily data sync).
  - **Business Process Flows**: Guide users through multi-step processes in Dataverse.
  - **Desktop Flows**: Automate desktop/web tasks via Robotic Process Automation (RPA).
- **Triggers and Actions**: 
  - Triggers start flows (e.g., “When an item is created” in SharePoint).
  - Actions perform tasks (e.g., send email, update Dataverse).
- **Connectors**: 400+ connectors (e.g., SharePoint, Outlook, Graph API) for integration.
- **Expressions**: Use Power Fx-like syntax for dynamic logic (e.g., `addDays()`, `concat()`).
- **SPFX Integration**: Power Automate can be triggered from SPFX web parts (e.g., via HTTP request) or enhance SharePoint processes (e.g., approvals).
- **Security**: Azure AD authentication, encrypted data, and governance via DLP policies.
- **Advanced Features**:
  - **Conditionals/Loops**: Control flow with `if`, `for each`.
  - **Error Handling**: Try-catch blocks or configure run-after settings.
  - **AI Builder**: Integrate AI (e.g., text recognition in flows).
- **Use Cases**: Automating approvals, syncing data between SharePoint and external systems, or notifying teams via Teams/Outlook.

#### Mock Questions
1. **How would you automate a SharePoint approval process using Power Automate?**
   - *Answer*: I’d create an Automated Flow with the “When an item is created” SharePoint trigger. I’d add a “Start and wait for an approval” action, configure approvers dynamically from a SharePoint column, and update the item status (“Approved”/”Rejected”) based on the outcome using “Update item.”
2. **What’s the difference between a Scheduled Flow and an Automated Flow, and when would you use each?**
   - *Answer*: Scheduled Flows run on a fixed schedule (e.g., daily report generation), while Automated Flows react to events (e.g., new SharePoint item). I’d use Scheduled for recurring tasks like backups and Automated for real-time responses like email notifications.

#### Code Snippet
```json
// Power Automate Flow Example (JSON Definition)
// Trigger: When an item is created in SharePoint
{
  "trigger": {
    "type": "When_an_item_is_created",
    "inputs": {
      "siteAddress": "https://contoso.sharepoint.com/sites/Team",
      "listName": "Tasks"
    }
  },
  "actions": [
    {
      "type": "Send_an_email",
      "inputs": {
        "to": "user@contoso.com",
        "subject": "New Task: @{triggerBody()?['Title']}",
        "body": "A new task was created."
      }
    }
  ]
}

// Calling from PowerApps (Instant Flow)
Button.OnSelect = 
PowerAutomateFlow.Run(JSON({ "title": TextInput1.Text }))
```