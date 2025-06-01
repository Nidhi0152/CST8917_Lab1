# CST8917 – Lab 1: 

# Azure Function with Storage Queue Output Binding

###  Environment Setup (WSL)

1. Installed Azure Functions Core Tools:
   ```bash
   sudo npm install -g azure-functions-core-tools@4 --unsafe-perm=true
   ```

2. Installed Azurite:
   ```bash
   sudo npm install -g azurite
   ```

---

##  Azure Function Project Creation

1. In VS Code:  
   `F1` → `Azure Functions: Create New Project...`
   - Language: Python (Programming Model V2)
   - Template: HTTP Trigger
   - Function name: `HttpExample`
   - Authorization: Anonymous
   - Interpreter: Python 3

2. Output binding added in `function_app.py`:
   ```python
   @app.queue_output(arg_name="msg", queue_name="outqueue", connection="AzureWebJobsStorage")
   def HttpExample(req: func.HttpRequest, msg: func.Out[func.QueueMessage]) -> func.HttpResponse:
       ...
       if name:
           msg.set(name)
           return func.HttpResponse(f"Hello, {name}. This HTTP triggered function executed successfully.")
   ```

---

## Azure Setup

1. **Created Azure Function App** using:
   - `F1` → `Azure Functions: Create Function App in Azure`
   - Selected unique name, region, and Python runtime

2. **Downloaded remote app settings**:
   - `F1` → `Azure Functions: Download Remote Settings...`
   - This pulled the `AzureWebJobsStorage` connection string into `local.settings.json`

---

## Running and Testing the Function

###  Locally:

1. In VS Code, press `F5` to start the function app
2. In Azure tab, expand **Local Project > Functions**
3. Right-click `HttpExample` → **Execute Function Now...**
4. Enter:
   ```json
   { "name": "Azure" }
   ```
5. Press Enter
6. Message is sent to `outqueue`
7. Stop local function with `Ctrl + C`

###  On Azure (Remote Execution):

1. Deployed to Azure:
   ```bash
   F1 → Azure Functions: Deploy to Function App
   ```

2. Triggered the function remotely:
   ```bash
   F1 → Azure Functions: Execute Function Now...
   ```

3. Used same request body:
   ```json
   { "name": "Azure" }
   ```

4. Function executed successfully and sent a message to the Azure Storage Queue

---

##  Queue Verification

1. Opened **Azure Storage Explorer**
2. Connected to Azure account
3. Navigated to:
   ```
   Storage Accounts → Queues → outqueue
   ```
4. Verified that the message `"Azure"` was successfully enqueued

---

# Azure Function with Azure SQL Output Binding
##  Azure Function Project Creation

1. In VS Code:  
   `F1` → `Azure Functions: Create New Project...`
   - Language: Python (Programming Model V2)
   - Template: HTTP Trigger
   - Function name: `HttpExample1`
   - Authorization: Anonymous
   - Interpreter: Python 3

2. Output binding added in `function_app.py`:
   ```python
   @app.generic_output_binding(
       arg_name="toDoItems",
       type="sql",
       CommandText="dbo.ToDo",
       ConnectionStringSetting="SqlConnectionString",
       data_type=DataType.STRING
   )
   def HttpExample1(req: func.HttpRequest, toDoItems: func.Out[func.SqlRow]) -> func.HttpResponse:
       name = req.get_json().get('name')
       if name:
           toDoItems.set(func.SqlRow({
               "Id": str(uuid.uuid4()),
               "title": name,
               "completed": False,
               "url": ""
           }))
           return func.HttpResponse(f"Hello {name}!")
       return func.HttpResponse("Please provide a name.", status_code=400)
   ```

---

##  Azure Setup

1. **Created Azure SQL Database** in Azure Portal:
   - Serverless DB: `mySampleDatabase`
   - Server name:   `nidhisqlserver2025`
   - Admin login: `azureuser`
   - Password: Azure@1234567
   - Enabled: *Allow Azure services to access this server*

2. **Created Table via Query Editor**:
   ```sql
   CREATE TABLE dbo.ToDo (
       [Id] UNIQUEIDENTIFIER PRIMARY KEY,
       [order] INT NULL,
       [title] NVARCHAR(200) NOT NULL,
       [url] NVARCHAR(200) NOT NULL,
       [completed] BIT NOT NULL
   );
   ```

3. **Downloaded remote app settings**:
   - `F1` → `Azure Functions: Download Remote Settings...`
   - This pulled the `SqlConnectionString` into `local.settings.json`
---

##  Running and Testing the Function

###  Locally:

1. In VS Code, press `F5` to start the function app
2. In Azure tab, expand **Local Project > Functions**
3. Right-click `HttpExample1` → **Execute Function Now...**
4. Enter:
   ```json
   { "name": "Azure" }
   ```
5. Press Enter
6. Data is written to the Azure SQL `dbo.ToDo` table


###  On Azure (Remote Execution):

1. Deployed to Azure:
   ```bash
   F1 → Azure Functions: Deploy to Function App
   ```

2. Triggered the function remotely:
   ```bash
   F1 → Azure Functions: Execute Function Now...
   ```

3. Used same request body:
   ```json
   { "name": "Azure" }
   ```

4. Function executed successfully and wrote to the Azure SQL table

---

##  SQL Data Verification

1. Opened Azure Portal → SQL Database → Query Editor
2. Logged in with `azureuser` credentials
3. Ran:
   ```sql
   SELECT TOP 1000 * FROM dbo.ToDo;

   ```
4. Verified output
---
## Demo Video Link
1. Azure Function Apps with Output Bindings: SQL Binding [https://youtu.be/mBmWlE5qtXE]
2. Azure Function Apps with Output Bindings: Storage Queue Binding[https://youtu.be/dBF1IP816n4]


