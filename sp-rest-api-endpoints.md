
### **Core Endpoints**

#### **1. Web Endpoint**
- **Purpose**: Access and manipulate site (web) properties and collections.
- **Base URI**: `/_api/web`
- **Methods and Examples**:
  - **GET**: Retrieve site details.
    - **Example**: `GET https://contoso.sharepoint.com/_api/web`
    - **Use**: Fetches metadata like Title, Description, or URL of the site.
    - **Response**: JSON with properties like `Title`, `Id`, `Url`.
  - **POST**: Update site properties (requires X-RequestDigest header).
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/web
      Headers: { "X-RequestDigest": "<form-digest-value>", "Content-Type": "application/json;odata=verbose" }
      Body: { "__metadata": { "type": "SP.Web" }, "Title": "Updated Site Title" }
      ```
    - **Use**: Updates properties like the site title.
- **Limitations**:
  - Cannot create a new web via REST (use `/_api/web/webs/add` instead).
  - Updates are limited to properties exposed by the API; some advanced settings (e.g., navigation) require additional endpoints.

#### **2. Lists Endpoint**
- **Purpose**: Manage lists in a site.
- **Base URI**: `/_api/web/lists`
- **Methods and Examples**:
  - **GET**: Retrieve all lists.
    - **Example**: `GET https://contoso.sharepoint.com/_api/web/lists`
    - **Use**: Returns a collection of all lists in the site.
  - **GET (by Title or GUID)**: Retrieve a specific list.
    - **Example**: `GET https://contoso.sharepoint.com/_api/web/lists/getbytitle('Tasks')`
    - **Use**: Fetches details of the "Tasks" list.
  - **POST**: Create a new list.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/web/lists
      Headers: { "X-RequestDigest": "<form-digest-value>", "Content-Type": "application/json;odata=verbose" }
      Body: { "__metadata": { "type": "SP.List" }, "Title": "NewList", "BaseTemplate": 100, "Description": "A new list" }
      ```
    - **Use**: Creates a generic list (BaseTemplate: 100).
  - **POST (Update)**: Update list properties using MERGE.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/web/lists/getbytitle('Tasks')
      Headers: { "X-RequestDigest": "<form-digest-value>", "X-HTTP-Method": "MERGE", "If-Match": "*" }
      Body: { "__metadata": { "type": "SP.List" }, "Title": "UpdatedTasks" }
      ```
    - **Use**: Updates the list title without overwriting other properties.
  - **DELETE**: Delete a list.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/web/lists/getbytitle('Tasks')
      Headers: { "X-RequestDigest": "<form-digest-value>", "X-HTTP-Method": "DELETE", "If-Match": "*" }
      ```
    - **Use**: Deletes the "Tasks" list.
- **Limitations**:
  - List view threshold (5,000 items) applies; queries returning over 5,000 items fail unless indexed columns are used.
  - Cannot directly manage list views or advanced settings via this endpoint.

#### **3. List Items Endpoint**
- **Purpose**: Perform CRUD operations on list items.
- **Base URI**: `/_api/web/lists/getbytitle('<ListName>')/items`
- **Methods and Examples**:
  - **GET**: Retrieve all items or a specific item.
    - **Example**: `GET https://contoso.sharepoint.com/_api/web/lists/getbytitle('Tasks')/items?$select=Title,Id&$filter=Id eq 1`
    - **Use**: Fetches items, optionally filtered or with specific fields.
  - **POST**: Create a new item.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/web/lists/getbytitle('Tasks')/items
      Headers: { "X-RequestDigest": "<form-digest-value>", "Content-Type": "application/json;odata=verbose" }
      Body: { "__metadata": { "type": "SP.Data.TasksListItem" }, "Title": "New Task" }
      ```
    - **Use**: Adds a new item to the "Tasks" list.
  - **POST (Update)**: Update an item using MERGE.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/web/lists/getbytitle('Tasks')/items(1)
      Headers: { "X-RequestDigest": "<form-digest-value>", "X-HTTP-Method": "MERGE", "If-Match": "*" }
      Body: { "__metadata": { "type": "SP.Data.TasksListItem" }, "Title": "Updated Task" }
      ```
    - **Use**: Updates item with ID 1.
  - **DELETE**: Delete an item.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/web/lists/getbytitle('Tasks')/items(1)
      Headers: { "X-RequestDigest": "<form-digest-value>", "X-HTTP-Method": "DELETE", "If-Match": "*" }
      ```
    - **Use**: Deletes item with ID 1.
- **Limitations**:
  - 5,000-item threshold applies; use `$top` and `$skiptoken` for pagination.
  - Complex filters with OR conditions may fail if intermediate results exceed 5,000 items, even with `$top`.
  - Lookup fields require `$expand` to retrieve related data.

#### **4. Files and Folders Endpoint**
- **Purpose**: Manage files and folders in document libraries.
- **Base URI**: `/_api/web/GetFolderByServerRelativeUrl('/Shared Documents')` or `/_api/web/GetFileByServerRelativeUrl('/Shared Documents/file.docx')`
- **Methods and Examples**:
  - **GET (Folder)**: Retrieve folder contents.
    - **Example**: `GET https://contoso.sharepoint.com/_api/web/GetFolderByServerRelativeUrl('/Shared Documents')/Files`
    - **Use**: Lists all files in "Shared Documents".
  - **POST (Upload File)**: Upload a file.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/web/GetFolderByServerRelativeUrl('/Shared Documents')/Files/add(url='test.docx',overwrite=true)
      Headers: { "X-RequestDigest": "<form-digest-value>", "Content-Type": "application/octet-stream" }
      Body: <binary file content>
      ```
    - **Use**: Uploads "test.docx" to "Shared Documents".
  - **GET (File)**: Download a file.
    - **Example**: `GET https://contoso.sharepoint.com/_api/web/GetFileByServerRelativeUrl('/Shared Documents/test.docx')/$value`
    - **Use**: Retrieves the file content.
  - **DELETE**: Delete a file.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/web/GetFileByServerRelativeUrl('/Shared Documents/test.docx')
      Headers: { "X-RequestDigest": "<form-digest-value>", "X-HTTP-Method": "DELETE" }
      ```
    - **Use**: Deletes "test.docx".
- **Limitations**:
  - File size limit for upload via REST is 2GB (use chunked upload for larger files with `startupload`, `continueupload`, `finishupload`).
  - No direct support for folder creation; use `/_api/web/folders` instead.

#### **5. Users and Groups Endpoint**
- **Purpose**: Manage site users and groups.
- **Base URI**: `/_api/web/siteusers` or `/_api/web/sitegroups`
- **Methods and Examples**:
  - **GET (Users)**: Retrieve all site users.
    - **Example**: `GET https://contoso.sharepoint.com/_api/web/siteusers`
    - **Use**: Lists all users with access to the site.
  - **GET (Current User)**: Get current user info.
    - **Example**: `GET https://contoso.sharepoint.com/_api/web/currentuser`
    - **Use**: Fetches details like email, ID, and login name.
  - **GET (Groups)**: Retrieve all groups.
    - **Example**: `GET https://contoso.sharepoint.com/_api/web/sitegroups`
    - **Use**: Lists all site groups.
  - **POST**: Add user to a group.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/web/sitegroups(5)/users
      Headers: { "X-RequestDigest": "<form-digest-value>", "Content-Type": "application/json;odata=verbose" }
      Body: { "__metadata": { "type": "SP.User" }, "LoginName": "i:0#.f|membership|user@contoso.com" }
      ```
    - **Use**: Adds a user to group with ID 5.
- **Limitations**:
  - Cannot create new groups via REST; use SharePoint UI or CSOM.
  - User management is limited to site-level permissions; tenant-level operations require Microsoft Graph.

#### **6. Search Endpoint**
- **Purpose**: Query SharePoint content via search.
- **Base URI**: `/_api/search/query`
- **Methods and Examples**:
  - **GET**: Perform a search.
    - **Example**: `GET https://contoso.sharepoint.com/_api/search/query?querytext='test'&$selectproperties='Title,Path'`
    - **Use**: Searches for "test" and returns Title and Path.
- **Limitations**:
  - Results depend on crawl schedules; recent changes may not appear immediately.
  - Limited to 500 rows per query by default; use `rowlimit` and `startrow` for pagination.

#### **7. Context Info Endpoint**
- **Purpose**: Retrieve form digest value for POST requests.
- **Base URI**: `/_api/contextinfo`
- **Methods and Examples**:
  - **POST**: Get digest value.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/contextinfo
      Headers: { "Accept": "application/json;odata=verbose" }
      ```
    - **Use**: Returns `FormDigestValue` for subsequent POST requests.
- **Limitations**:
  - Digest expires after ~30 minutes; must be refreshed periodically.

---

### **Modern Endpoints**

#### **8. SPSiteManager Endpoint**
- **Purpose**: Manage modern site collections (create, delete, or check status).
- **Base URI**: `/_api/SPSiteManager`
- **Methods and Examples**:
  - **POST (Create)**: Create a modern site (e.g., communication or team site).
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/SPSiteManager/create
      Headers: { "Accept": "application/json;odata.metadata=none", "Content-Type": "application/json" }
      Body: {
        "request": {
          "Title": "New Comm Site",
          "Url": "https://contoso.sharepoint.com/sites/NewCommSite",
          "Lcid": 1033,
          "WebTemplate": "SITEPAGEPUBLISHING#0",
          "Description": "A new communication site"
        }
      }
      ```
    - **Use**: Creates a communication site (SITEPAGEPUBLISHING#0) or non-group team site.
  - **POST (Delete)**: Delete a site.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/SPSiteManager/delete
      Headers: { "Accept": "application/json;odata.metadata=none", "Content-Type": "application/json" }
      Body: { "siteId": "d11e59ca-1465-424c-be90-c847ba849af5" }
      ```
    - **Use**: Deletes a site by its ID.
  - **GET (Status)**: Check site creation status.
    - **Example**: `GET https://contoso.sharepoint.com/_api/SPSiteManager/status?url='https://contoso.sharepoint.com/sites/NewCommSite'`
    - **Use**: Returns status (e.g., 2 for completed) and site ID.
- **Limitations**:
  - Only works for modern sites; classic site creation requires other methods (e.g., `/_api/web/webs/add`).
  - Requires tenant admin permissions for most operations.
  - Site design application requires separate calls to utility endpoints.

#### **9. Site Pages Endpoint**
- **Purpose**: Manage modern site pages (e.g., create, update, or publish).
- **Base URI**: `/_api/sitepages/pages`
- **Methods and Examples**:
  - **POST**: Create a modern page.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/sites/SiteName/_api/sitepages/pages
      Headers: { "X-RequestDigest": "<form-digest-value>", "Content-Type": "application/json;odata=verbose" }
      Body: {
        "__metadata": { "type": "SP.Publishing.SitePage" },
        "Name": "NewPage.aspx",
        "Title": "New Page"
      }
      ```
    - **Use**: Adds a new modern page to the Site Pages library.
  - **GET**: Retrieve page details.
    - **Example**: `GET https://contoso.sharepoint.com/sites/SiteName/_api/sitepages/pages/GetById(1)`
    - **Use**: Fetches metadata for page with ID 1.
  - **POST (Publish)**: Publish a page.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/sites/SiteName/_api/sitepages/pages(1)/publish
      Headers: { "X-RequestDigest": "<form-digest-value>" }
      ```
    - **Use**: Publishes page with ID 1.
- **Limitations**:
  - Limited to modern Site Pages library; classic pages use different endpoints (e.g., `/_api/web/lists`).
  - Cannot manipulate page content (e.g., web parts) directly; requires additional calls to `/_api/web/lists`.

#### **10. Hub Sites Endpoint**
- **Purpose**: Manage hub site associations and properties.
- **Base URI**: `/_api/hubsites`
- **Methods and Examples**:
  - **GET**: List all hub sites.
    - **Example**: `GET https://contoso.sharepoint.com/_api/hubsites`
    - **Use**: Returns a collection of hub sites in the tenant.
  - **POST**: Associate a site with a hub.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/sites/SiteName/_api/web/HubSiteData
      Headers: { "X-RequestDigest": "<form-digest-value>", "Content-Type": "application/json" }
      Body: { "hubSiteId": "123e4567-e89b-12d3-a456-426614174000" }
      ```
    - **Use**: Links a site to a hub site.
- **Limitations**:
  - Requires hub site feature enabled at tenant level.
  - Limited control over hub site creation (admin-only via PowerShell or UI).

---

### **Utility Endpoints**

#### **11. Site Script Utility Endpoint**
- **Purpose**: Manage site scripts and designs for modern site customization.
- **Base URI**: `/_api/Microsoft.SharePoint.Utilities.WebTemplateExtensions.SiteScriptUtility`
- **Methods and Examples**:
  - **POST (Create Site Script)**: Add a site script.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/Microsoft.SharePoint.Utilities.WebTemplateExtensions.SiteScriptUtility/CreateSiteScript(Title=@title)?@title='CustomScript'
      Headers: { "Content-Type": "application/json" }
      Body: {
        "actions": [{ "verb": "applyTheme", "themeName": "Contoso Theme" }],
        "version": 1
      }
      ```
    - **Use**: Creates a script to apply a theme.
  - **GET (List Site Scripts)**: Retrieve all site scripts.
    - **Example**: `GET https://contoso.sharepoint.com/_api/Microsoft.SharePoint.Utilities.WebTemplateExtensions.SiteScriptUtility/GetSiteScripts`
    - **Use**: Returns all site scripts in JSON.
  - **POST (Create Site Design)**: Create a site design.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/Microsoft.SharePoint.Utilities.WebTemplateExtensions.SiteScriptUtility/CreateSiteDesign
      Headers: { "Content-Type": "application/json" }
      Body: {
        "Title": "Contoso Design",
        "SiteScriptIds": ["7647d3d6-1046-41fe-a798-4ff66b099d12"],
        "WebTemplate": "64"  // 64 = Communication Site
      }
      ```
    - **Use**: Defines a reusable site design.
  - **POST (Apply Site Design)**: Apply a design to a site.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/Microsoft.SharePoint.Utilities.WebTemplateExtensions.SiteScriptUtility/ApplySiteDesign
      Headers: { "Content-Type": "application/json" }
      Body: {
        "siteDesignId": "614f9b28-3e85-4ec9-a961-5971ea086cca",
        "webUrl": "https://contoso.sharepoint.com/sites/SiteName"
      }
      ```
    - **Use**: Applies a design to an existing site.
- **Limitations**:
  - Site scripts are JSON-based and complex to debug; errors are often vague.
  - Limited to modern sites; classic sites don’t support site designs.

#### **12. Social Feed Endpoint**
- **Purpose**: Manage social interactions (e.g., posts, likes, follows).
- **Base URI**: `/_api/social.feed`
- **Methods and Examples**:
  - **GET**: Retrieve a user’s feed.
    - **Example**: `GET https://contoso.sharepoint.com/_api/social.feed/my/feed`
    - **Use**: Fetches the current user’s social feed.
  - **POST**: Create a post.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/social.feed/post
      Headers: { "X-RequestDigest": "<form-digest-value>", "Content-Type": "application/json;odata=verbose" }
      Body: {
        "restCreationData": {
          "__metadata": { "type": "SP.Social.SocialRestPostCreationData" },
          "content": "Hello from REST API!",
          "updateStatusText": true
        }
      }
      ```
    - **Use**: Adds a post to the user’s feed.
- **Limitations**:
  - Social features are less prominent in modern SharePoint; Microsoft Graph is preferred for broader social integration.
  - Limited to SharePoint-specific social data, not Teams or Yammer.

#### **13. Suggest Endpoint (Search Suggestions)**
- **poonse**: Manage search query suggestions.
- **Base URI**: `/_api/search/suggest`
- **Methods and Examples**:
  - **GET**: Get search suggestions.
    - **Example**: `GET https://contoso.sharepoint.com/_api/search/suggest?querytext='shar'`
    - **Use**: Returns suggestions like "SharePoint" based on "shar".
- **Limitations**:
  - Dependent on search index freshness.
  - Limited customization of suggestion logic.

#### **14. Role Assignments/Permissions Endpoint**
- **Purpose**: Manage permissions (role assignments) for sites, lists, or items.
- **Base URI**: `/_api/web/roleassignments` or `/_api/web/lists/getbytitle('<ListName>')/roleassignments`
- **Methods and Examples**:
  - **GET**: Retrieve role assignments for a web.
    - **Example**: `GET https://contoso.sharepoint.com/_api/web/roleassignments`
    - **Use**: Lists all permission assignments (e.g., user/group roles like "Contribute").
  - **POST**: Add a role assignment (e.g., grant permissions).
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/web/roleassignments/addroleassignment(principalid=10,roleDefId=1073741827)
      Headers: { "X-RequestDigest": "<form-digest-value>" }
      ```
    - **Use**: Grants "Contribute" (roleDefId 1073741827) to a user/group with principal ID 10.
  - **POST (Delete)**: Remove a role assignment.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/web/roleassignments/removeroleassignment(principalid=10)
      Headers: { "X-RequestDigest": "<form-digest-value>" }
      ```
    - **Use**: Revokes permissions for principal ID 10.
- **Limitations**:
  - Requires breaking inheritance first if modifying permissions at a lower level (e.g., list or item).
  - Role definition IDs (e.g., 1073741827 for Contribute) must be known; no direct way to fetch them via REST alone.

#### **15. Workflow Services Endpoint**
- **Purpose**: Interact with SharePoint 2013-style workflows (not Power Automate).
- **Base URI**: `/_api/SP.WorkflowServices.WorkflowInstanceService.Current` or `/_api/SP.WorkflowServices.WorkflowSubscriptionService.Current`
- **Methods and Examples**:
  - **GET (Subscriptions)**: List workflow definitions.
    - **Example**: `GET https://contoso.sharepoint.com/_api/SP.WorkflowServices.WorkflowSubscriptionService.Current/EnumerateSubscriptions`
    - **Use**: Retrieves available workflow subscriptions (definitions).
  - **POST (Start Workflow)**: Start a workflow instance.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/SP.WorkflowServices.WorkflowInstanceService.Current/StartWorkflow
      Headers: { "X-RequestDigest": "<form-digest-value>", "Content-Type": "application/json;odata=verbose" }
      Body: {
        "parameters": {
          "itemId": 1,
          "subscriptionId": "7647d3d6-1046-41fe-a798-4ff66b099d12"
        }
      }
      ```
    - **Use**: Triggers a workflow on item ID 1 using a specific subscription ID.
- **Limitations**:
  - Only supports SharePoint 2013 workflows; Power Automate flows use Microsoft Graph or separate APIs.
  - Deprecated in SharePoint Online; Microsoft pushes Power Automate instead.

#### **16. Analytics Endpoint**
- **Purpose**: Retrieve usage or analytics data for sites or files.
- **Base URI**: `/_api/web/GetFileByServerRelativeUrl('/path/to/file')/GetAnalytics`
- **Methods and Examples**:
  - **GET**: Get file analytics.
    - **Example**: `GET https://contoso.sharepoint.com/_api/web/GetFileByServerRelativeUrl('/Shared Documents/report.docx')/GetAnalytics`
    - **Use**: Returns view counts, last accessed date, etc., for the file.
- **Limitations**:
  - Limited to tenant-enabled analytics; not all tenants expose detailed data.
  - Data may lag due to processing delays.

#### **17. Navigation Endpoint**
- **Purpose**: Manage site navigation (Quick Launch or Top Navigation).
- **Base URI**: `/_api/web/navigation`
- **Methods and Examples**:
  - **GET**: Retrieve Quick Launch nodes.
    - **Example**: `GET https://contoso.sharepoint.com/_api/web/navigation/quicklaunch`
    - **Use**: Fetches the Quick Launch menu items.
  - **POST**: Add a navigation node.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/web/navigation/quicklaunch
      Headers: { "X-RequestDigest": "<form-digest-value>", "Content-Type": "application/json;odata=verbose" }
      Body: {
        "__metadata": { "type": "SP.NavigationNode" },
        "Title": "New Link",
        "Url": "/sites/SiteName/Pages/NewPage.aspx"
      }
      ```
    - **Use**: Adds a new link to Quick Launch.
- **Limitations**:
  - Modern sites favor hub navigation or mega menus, reducing reliance on this endpoint.
  - No direct support for reordering nodes via REST.

#### **18. Regional Settings Endpoint**
- **Purpose**: Access or update regional settings (e.g., time zone, locale).
- **Base URI**: `/_api/web/RegionalSettings`
- **Methods and Examples**
  - **GET**: Retrieve regional settings.
    - **Example**: `GET https://contoso.sharepoint.com/_api/web/RegionalSettings`
    - **Use**: Returns time zone, locale ID, etc.
  - **POST**: Update time zone.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/web/RegionalSettings
      Headers: { "X-RequestDigest": "<form-digest-value>", "X-HTTP-Method": "MERGE", "If-Match": "*" }
      Body: { "__metadata": { "type": "SP.RegionalSettings" }, "TimeZoneId": 13 }  // 13 = UTC
      ```
    - **Use**: Sets the site’s time zone to UTC.
- **Limitations**:
  - Limited to properties exposed by the API; some settings (e.g., calendar type) require CSOM.

---

### **CAML Query Endpoints**

#### **19. GetItems Endpoint (CAML Query for List Items)**
- **Purpose**: Retrieve list items using a CAML query for precise filtering, ordering, or joins.
- **Base URI**: `/_api/web/lists/getbytitle('<ListName>')/GetItems`
- **Methods and Examples**:
  - **POST**: Execute a CAML query to fetch items.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/web/lists/getbytitle('Tasks')/GetItems
      Headers: { "X-RequestDigest": "<form-digest-value>", "Content-Type": "application/json;odata=verbose" }
      Body: {
        "query": {
          "__metadata": { "type": "SP.CamlQuery" },
          "Query": "<View><Query><Where><Eq><FieldRef Name='Status'/><Value Type='Text'>Completed</Value></Eq></Where></Query></View>"
        }
      }
      ```
    - **Use**: Retrieves all items in the "Tasks" list where the "Status" field equals "Completed".
    - **Response**: JSON array of matching items with their field values.
  - **Advanced Example (with OrderBy and RowLimit)**:
    ```
    POST https://contoso.sharepoint.com/_api/web/lists/getbytitle('Tasks')/GetItems
    Headers: { "X-RequestDigest": "<form-digest-value>", "Content-Type": "application/json;odata=verbose" }
    Body: {
      "query": {
        "__metadata": { "type": "SP.CamlQuery" },
        "Query": "<View><Query><Where><Geq><FieldRef Name='DueDate'/><Value Type='DateTime'><Today/></Value></Geq></Where><OrderBy><FieldRef Name='Title' Ascending='TRUE'/></OrderBy></Query><RowLimit>10</RowLimit></View>"
      }
    }
    ```
    - **Use**: Fetches up to 10 tasks due today or later, sorted by Title ascending.
- **Limitations**:
  - List view threshold (5,000 items); queries exceeding this fail unless filtered by indexed columns or scoped to a folder.
  - No pagination support; custom logic needed for large datasets.
  - CAML syntax is verbose and error-prone.

#### **20. RenderListDataAsStream Endpoint**
- **Purpose**: Render list data (items and metadata) based on a CAML query, often mimicking a list view’s output.
- **Base URI**: `/_api/web/lists/getbytitle('<ListName>')/RenderListDataAsStream`
- **Methods and Examples**:
  - **POST**: Fetch list data with a CAML query.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/web/lists/getbytitle('Tasks')/RenderListDataAsStream
      Headers: { "X-RequestDigest": "<form-digest-value>", "Content-Type": "application/json;odata=verbose" }
      Body: {
        "parameters": {
          "ViewXml": "<View><Query><Where><Eq><FieldRef Name='Priority'/><Value Type='Text'>High</Value></Eq></Where></Query></View>"
        }
      }
      ```
    - **Use**: Returns items from the "Tasks" list where Priority is "High", with view metadata.
  - **Advanced Example (with Pagination)**:
    ```
    POST https://contoso.sharepoint.com/_api/web/lists/getbytitle('Tasks')/RenderListDataAsStream
    Headers: { "X-RequestDigest": "<form-digest-value>", "Content-Type": "application/json;odata=verbose" }
    Body: {
      "parameters": {
        "ViewXml": "<View><Query><Where><Eq><FieldRef Name='Status'/><Value Type='Text'>In Progress</Value></Eq></Where></Query></View>",
        "Paging": "ListItemCollectionPositionNext=<paging-token>"
      }
    }
    ```
    - **Use**: Fetches "In Progress" tasks with pagination.
- **Limitations**:
  - 5,000-item threshold applies unless mitigated.
  - Returns extra metadata (e.g., schema), increasing payload size.
  - Best used with existing view CAML for consistency.

#### **21. RenderListData Endpoint (Legacy)**
- **Purpose**: Similar to `RenderListDataAsStream`, but older and simpler.
- **Base URI**: `/_api/web/lists/getbytitle('<ListName>')/RenderListData`
- **Methods and Examples**:
  - **POST**: Execute a CAML query.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/web/lists/getbytitle('Tasks')/RenderListData('<View><Query><Where><Eq><FieldRef Name="AssignedTo" /><Value Type="User">John Doe</Value></Eq></Where></Query></View>')
      Headers: { "X-RequestDigest": "<form-digest-value>" }
      ```
    - **Use**: Returns tasks assigned to "John Doe".
- **Limitations**:
  - Deprecated; superseded by `RenderListDataAsStream`.
  - Less efficient for large datasets.
  - Basic output lacks rich metadata.

---

### **Content Types and Metadata Endpoints**

#### **22. Web Content Types Endpoint**
- **Purpose**: Manage content types at the site (web) level.
- **Base URI**: `/_api/web/contenttypes`
- **Methods and Examples**:
  - **GET**: Retrieve all content types in the site.
    - **Example**: `GET https://contoso.sharepoint.com/_api/web/contenttypes`
    - **Use**: Lists all site content types (e.g., "Item", "Document").
  - **GET (Specific Content Type)**: Retrieve a single content type by ID.
    - **Example**: `GET https://contoso.sharepoint.com/_api/web/contenttypes('0x0101')`
    - **Use**: Fetches details of the "Document" content type.
  - **POST**: Create a new content type.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/web/contenttypes
      Headers: { "X-RequestDigest": "<form-digest-value>", "Content-Type": "application/json;odata=verbose" }
      Body: {
        "__metadata": { "type": "SP.ContentType" },
        "Id": "0x0101009189A8F7B6C6A14B8F9D2E6C9B8A5F5F",
        "Name": "Custom Document",
        "Description": "A custom document type",
        "Group": "Custom Content Types"
      }
      ```
    - **Use**: Creates a new content type inheriting from "Document".
  - **POST (Update)**: Update a content type using MERGE.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/web/contenttypes('0x0101009189A8F7B6C6A14B8F9D2E6C9B8A5F5F')
      Headers: { "X-RequestDigest": "<form-digest-value>", "X-HTTP-Method": "MERGE", "If-Match": "*" }
      Body: { "__metadata": { "type": "SP.ContentType" }, "Description": "Updated description" }
      ```
    - **Use**: Updates the description.
  - **DELETE**: Delete a content type.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/web/contenttypes('0x0101009189A8F7B6C6A14B8F9D2E6C9B8A5F5F')
      Headers: { "X-RequestDigest": "<form-digest-value>", "X-HTTP-Method": "DELETE", "If-Match": "*" }
      ```
    - **Use**: Removes the custom content type.
- **Limitations**:
  - Inheritance required; ID must follow hierarchical format.
  - Changes don’t propagate to lists automatically.
  - Adding fields requires separate `fieldlinks` calls.

#### **23. List Content Types Endpoint**
- **Purpose**: Manage content types associated with a specific list or library.
- **Base URI**: `/_api/web/lists/getbytitle('<ListName>')/contenttypes`
- **Methods and Examples**:
  - **GET**: Retrieve all content types in a list.
    - **Example**: `GET https://contoso.sharepoint.com/_api/web/lists/getbytitle('Documents')/contenttypes`
    - **Use**: Lists content types enabled for the "Documents" library.
  - **POST**: Add an existing content type to a list.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/web/lists/getbytitle('Documents')/contenttypes
      Headers: { "X-RequestDigest": "<form-digest-value>", "Content-Type": "application/json;odata=verbose" }
      Body: {
        "__metadata": { "type": "SP.ContentTypeId" },
        "ContentTypeId": "0x0101009189A8F7B6C6A14B8F9D2E6C9B8A5F5F"
      }
      ```
    - **Use**: Adds the custom content type to the "Documents" library.
  - **DELETE**: Remove a content type from a list.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/web/lists/getbytitle('Documents')/contenttypes('0x0101009189A8F7B6C6A14B8F9D2E6C9B8A5F5F')
      Headers: { "X-RequestDigest": "<form-digest-value>", "X-HTTP-Method": "DELETE" }
      ```
    - **Use**: Removes the custom content type from the list.
- **Limitations**:
  - System content types (e.g., "Item") can’t be removed via REST.
  - Field updates require additional `fieldlinks` calls.

#### **24. Content Type Fields (FieldLinks) Endpoint**
- **Purpose**: Manage fields (columns) associated with a content type.
- **Base URI**: `/_api/web/contenttypes('<ContentTypeId>')/fieldlinks` or `/_api/web/lists/getbytitle('<ListName>')/contenttypes('<ContentTypeId>')/fieldlinks`
- **Methods and Examples**:
  - **GET**: Retrieve fields in a content type.
    - **Example**: `GET https://contoso.sharepoint.com/_api/web/contenttypes('0x0101')/fieldlinks`
    - **Use**: Lists fields in the "Document" content type (e.g., "Title", "Name").
  - **POST**: Add a field's to a content type.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/web/contenttypes('0x0101009189A8F7B6C6A14B8F9D2E6C9B8A5F5F')/fieldlinks
      Headers: { "X-RequestDigest": "<form-digest-value>", "Content-Type": "application/json;odata=verbose" }
      Body: {
        "__metadata": { "type": "SP.FieldLink" },
        "Id": "fa564e0f-0c70-4ab9-b863-0177e6ddd247"  // GUID of an existing site column
      }
      ```
    - **Use**: Adds an existing site column (e.g., "Title") to the custom content type.
- **Limitations**:
  - Can only link to existing site columns; creating new fields requires the `fields` endpoint.
  - Deleting a field link is limited via REST; often requires CSOM or UI.

#### **25. Fields Endpoint (Site Columns)**
- **Purpose**: Manage site-level fields (columns) that can be linked to content types or lists.
- **Base URI**: `/_api/web/fields`
- **Methods and Examples**:
  - **GET**: Retrieve all site columns.
    - **Example**: `GET https://contoso.sharepoint.com/_api/web/fields`
    - **Use**: Lists all fields available at the site level.
  - **POST**: Create a new site column.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/web/fields
      Headers: { "X-RequestDigest": "<form-digest-value>", "Content-Type": "application/json;odata=verbose" }
      Body: {
        "__metadata": { "type": "SP.FieldText" },
        "Title": "CustomField",
        "FieldTypeKind": 2,  // 2 = Single line of text
        "Group": "Custom Columns"
      }
      ```
    - **Use**: Creates a text field named "CustomField".
  - **POST (Update)**: Update a field using MERGE.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/web/fields/getbytitle('CustomField')
      Headers: { "X-RequestDigest": "<form-digest-value>", "X-HTTP-Method": "MERGE", "If-Match": "*" }
      Body: { "__metadata": { "type": "SP.FieldText" }, "Description": "Updated description" }
      ```
    - **Use**: Updates the field’s description.
  - **DELETE**: Delete a field.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/web/fields/getbytitle('CustomField')
      Headers: { "X-RequestDigest": "<form-digest-value>", "X-HTTP-Method": "DELETE" }
      ```
    - **Use**: Removes the "CustomField".
- **Limitations**:
  - Must use correct `__metadata` type based on `FieldTypeKind`.
  - Cannot delete fields in use by content types or lists without breaking references.

#### **26. List Fields Endpoint**
- **Purpose**: Manage fields specific to a list or library.
- **Base URI**: `/_api/web/lists/getbytitle('<ListName>')/fields`
- **Methods and Examples**:
  - **GET**: Retrieve all fields in a list.
    - **Example**: `GET https://contoso.sharepoint.com/_api/web/lists/getbytitle('Tasks')/fields`
    - **Use**: Lists fields like "Title", "DueDate" in the "Tasks" list.
  - **POST**: Add a field to a list.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/web/lists/getbytitle('Tasks')/fields
      Headers: { "X-RequestDigest": "<form-digest-value>", "Content-Type": "application/json;odata=verbose" }
      Body: {
        "__metadata": { "type": "SP.FieldText" },
        "Title": "TaskNotes",
        "FieldTypeKind": 2
      }
      ```
    - **Use**: Adds a "TaskNotes" text field to the "Tasks" list.
- **Limitations**:
  - Fields are list-specific unless promoted to site columns.
  - Complex fields (e.g., managed metadata) require additional configuration.

#### **27. Taxonomy/Managed Metadata Endpoint**
- **Purpose**: Interact with the term store for managed metadata fields.
- **Base URI**: `/_api/SP.Taxonomy.TaxonomyService`
- **Methods and Examples**:
  - **GET**: Retrieve terms from a term set.
    - **Example**: `GET https://contoso.sharepoint.com/_api/SP.Taxonomy.TaxonomyService/GetTerms?termSetId='8ed8c9ea-7052-4c1d-a4d7-b9c10bffea6f'`
    - **Use**: Fetches terms from a specified term set.
  - **POST (Field Value)**: Set a managed metadata field value on an item.
    - **Example**:
      ```
      POST https://contoso.sharepoint.com/_api/web/lists/getbytitle('Documents')/items(1)
      Headers: { "X-RequestDigest": "<form-digest-value>", "X-HTTP-Method": "MERGE", "If-Match": "*" }
      Body: {
        "__metadata": { "type": "SP.Data.DocumentsItem" },
        "TaxonomyField": { "Label": "Term1", "TermGuid": "123e4567-e89b-12d3-a456-426614174000", "WssId": -1 }
      }
      ```
    - **Use**: Updates a managed metadata field on item ID 1.
- **Limitations**:
  - Most taxonomy operations require tenant admin permissions.
  - Limited REST support for term store management; CSOM or PowerShell often needed.
