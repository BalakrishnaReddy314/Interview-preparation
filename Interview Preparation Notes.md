# Interview Preparation Notes

## SPFX (SharePoint Framework)

### 1. Core SPFX Concepts
- **SPFX Architecture**: Client-side framework for SharePoint, leveraging TypeScript, React, and modern web tech.
- **Components**: Web parts (UI), extensions (app customizers, field customizers, command sets).
- **TypeScript**: Used for type safety; key in SPFX projects.
- **React**: Default UI framework; uses functional components and hooks.

**Mock Questions**:
- What’s the difference between a web part and an application customizer?
- How would you type a custom props interface for an SPFX web part?

**Code Snippet**:
```typescript
interface IMyWebPartProps {
  title: string;
  items: { id: number; name: string }[];
}
public render(): void {
  const element: React.ReactElement<IMyWebPartProps> = React.createElement(MyComponent, { title: this.properties.title });
  ReactDom.render(element, this.domElement);
}
```

### 2. Development Environment and Tooling
- **Node.js/npm**: Dependency management.
- **Yeoman/Gulp**: Scaffolding and task automation.
- **Webpack**: Bundles SPFX code.

**Mock Questions**:
- What happens when you run `gulp serve`?
- How would you debug an SPFX web part in production?

**Code Snippet**:
```bash
gulp bundle --ship
gulp package-solution --ship
```

### 3. SharePoint Integration
- **REST API**: CRUD operations on SharePoint data.
- **Microsoft Graph**: Broader M365 integration.
- **PnPJS**: Simplifies SharePoint operations.

**Mock Questions**:
- How do you fetch items from a SharePoint list using PnPJS?
- REST API vs. Graph API?

**Code Snippet**:
```typescript
import { spfi } from "@pnp/sp";
const sp = spfi().using(SPFx(this.context));
const items = await sp.web.lists.getByTitle("Tasks").items();
```

---

## Azure Functions

### Notes
- **Overview**: Serverless compute for event-driven code.
- **Triggers**: HTTP, Timer, Queue, etc.
- **Hosting**: Consumption (pay-per-use), Premium (low latency), App Service (dedicated).

**Mock Questions**:
- How would you use Azure Functions with SPFX?
- Stateless vs. Durable Functions?

**Code Snippet**:
```csharp
[FunctionName("SpfxBackend")]
public static async Task<IActionResult> Run([HttpTrigger] HttpRequest req) {
  string name = req.Query["name"];
  return new OkObjectResult(new { message = $"Hello, {name}" });
}
```

---

## Azure Logic Apps

### Notes
- **Overview**: Serverless workflow orchestration.
- **Types**: Consumption (multi-tenant), Standard (single-tenant).
- **Connectors**: 400+ for integration.

**Mock Questions**:
- How would you integrate Logic Apps with SPFX for document approval?
- Standard vs. Consumption Logic Apps?

**Code Snippet**:
```json
{
  "trigger": { "type": "Request", "kind": "Http" },
  "actions": { "UpdateSharePoint": { "type": "ApiConnection", "inputs": { "method": "patch" } } }
}
```

---

## PowerShell (SharePoint Context)

### Notes
- **Overview**: Automation for SharePoint admin tasks.
- **Modules**: SPO Management Shell, PnP PowerShell.
- **SPFX Use**: Deploy solutions, manage app catalogs.

**Mock Questions**:
- How would you deploy an SPFX solution with PowerShell?
- `Connect-SPOService` vs. `Connect-PnPOnline`?

**Code Snippet**:
```powershell
Connect-PnPOnline -Url "https://contoso.sharepoint.com"
Add-PnPApp -Path "my-solution.sppkg" -Publish
```

---

## C# (SharePoint Context)

### Notes
- **Overview**: Used with CSOM, SSOM, or REST for SharePoint dev.
- **CSOM**: Remote operations for SharePoint Online.
- **SPFX Use**: Backend logic (e.g., Azure Functions).

**Mock Questions**:
- How do you retrieve SharePoint list items with CSOM?
- Integrate C# Azure Function with SPFX?

**Code Snippet**:
```csharp
using (var context = new ClientContext(siteUrl)) {
  var list = context.Web.Lists.GetByTitle("Tasks");
  var items = list.GetItems(CamlQuery.CreateAllItemsQuery());
  context.Load(items);
  await context.ExecuteQueryAsync();
}
```

---

## React (SPFX Context)

### Notes
- **Overview**: UI framework for SPFX web parts.
- **Hooks**: `useState`, `useEffect` for state/side effects.
- **SPFX Integration**: Passed via `render()`.

**Mock Questions**:
- How do you pass SharePoint context to a React component?
- Optimize a React SPFX web part?

**Code Snippet**:
```jsx
const MyComponent = ({ spContext }) => {
  const [items, setItems] = useState([]);
  useEffect(() => {
    spContext.spHttpClient.get("...").then(res => setItems(res.json().value));
  }, [spContext]);
  return <ul>{items.map(item => <li>{item.Title}</li>)}</ul>;
};
```

---

## TypeScript (SPFX Context)

### Notes
- **Overview**: Type safety for SPFX projects.
- **Features**: Interfaces, generics, async/await.
- **SPFX Use**: Defines props, API responses.

**Mock Questions**:
- Define a TypeScript interface for SPFX props?
- Type a custom hook for SharePoint data?

**Code Snippet**:
```typescript
interface IProps { spContext: WebPartContext; }
function useSharePointList<T>(context: WebPartContext, listName: string) {
  const [items, setItems] = useState<T[]>([]);
  // Fetch logic
  return { items };
}
```

---

## React (Frontend Context)

### Notes
- **Overview**: Component-based UI library.
- **Hooks**: `useState`, `useEffect`, `useMemo`.
- **Advanced**: Lazy loading, error boundaries.

**Mock Questions**:
- Prevent unnecessary re-renders?
- Implement a custom fetch hook?

**Code Snippet**:
```jsx
function useFetch(url) {
  const [data, setData] = useState(null);
  useEffect(() => {
    fetch(url).then(res => res.json()).then(setData);
  }, [url]);
  return { data };
}
```

---

## TypeScript (Frontend Context)

### Notes
- **Overview**: Static typing for JS.
- **Features**: Interfaces, generics, utility types.
- **React Use**: Props, hooks typing.

**Mock Questions**:
- Type a React component’s props?
- Generic TypeScript function?

**Code Snippet**:
```typescript
interface ListProps<T> { items: T[]; renderItem: (item: T) => ReactNode; }
function List<T>({ items, renderItem }: ListProps<T>) {
  return <ul>{items.map((item, i) => <li key={i}>{renderItem(item)}</li>)}</ul>;
}
```

---

## JavaScript Topics

### 1. Core Concepts
- **Variables**: `var`, `let`, `const`.
- **Closures**: Retain outer scope access.

**Mock Questions**:
- `var` vs. `let` vs. `const`?
- How does a closure work?

**Code Snippet**:
```javascript
function counter() { let c = 0; return () => ++c; }
const c = counter();
console.log(c()); // 1
```

### 2. ES6+ Features
- **Arrow Functions**: No own `this`.
- **Promises**: Async handling.

**Mock Questions**:
- Regular vs. arrow function?
- Rewrite Promise with async/await?

**Code Snippet**:
```javascript
const fetchData = async () => await (await fetch("url")).json();
```

---

## REST API Concepts

### Notes
- **Overview**: Resource-based, stateless API over HTTP.
- **Methods**: `GET`, `POST`, `PUT`, `DELETE`.
- **Status Codes**: `200`, `401`, `404`.

**Mock Questions**:
- What makes an API RESTful?
- Design a tasks API?

**Code Snippet**:
```javascript
fetch('https://api.example.com/tasks', { method: 'GET', headers: { 'Authorization': 'Bearer <token>' } });
```

---

## OAuth Concepts

### Notes
- **Overview**: Authorization framework.
- **Flows**: Authorization Code, Implicit.
- **Tokens**: Access, refresh.

**Mock Questions**:
- Authorization Code vs. Implicit flow?
- Implement OAuth in a frontend app?

**Code Snippet**:
```javascript
const msalInstance = new msal.PublicClientApplication(config);
const token = await msalInstance.loginPopup({ scopes: ['User.Read'] }).accessToken;
```

---

## PowerApps

### Notes
- **Overview**: Low-code app platform.
- **Types**: Canvas, Model-Driven, Portals.
- **Power Fx**: Logic language.

**Mock Questions**:
- Canvas vs. Model-Driven Apps?
- Optimize for large SharePoint list?

**Code Snippet**:
```powerapps
Patch('Tasks', LookUp('Tasks', ID = 1), { Title: "Updated" });
```

---

## Power Automate

### Notes
- **Overview**: Workflow automation tool.
- **Flow Types**: Automated, Instant, Scheduled.
- **Connectors**: 400+ integrations.

**Mock Questions**:
- Automate SharePoint approval?
- Scheduled vs. Automated Flow?

**Code Snippet**:
```json
{ "trigger": { "type": "When_an_item_is_created" }, "actions": { "Send_an_email": {} } }
```