# CST8917 – Lab 1: Azure Function with Storage Queue Output Binding

This project implements a Python Azure Function (v2 programming model) that writes to an Azure Storage Queue via an output binding. The function is deployed to Azure and verified using Azure Storage Explorer.

---

## 🚀 What I Did

### 🔧 Environment Setup (WSL)

1. Installed Node.js:
   ```bash
   curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
   sudo apt install -y nodejs
   ```

2. Installed Azure Functions Core Tools:
   ```bash
   sudo npm install -g azure-functions-core-tools@4 --unsafe-perm=true
   ```

3. Installed Azurite (local storage emulator – optional):
   ```bash
   sudo npm install -g azurite
   ```

---

## 🧰 Azure Function Project Creation

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

## 🌐 Azure Setup

1. **Created Azure Function App** using:
   - `F1` → `Azure Functions: Create Function App in Azure`
   - Selected unique name, region, and Python runtime

2. **Downloaded remote app settings**:
   - `F1` → `Azure Functions: Download Remote Settings...`
   - This pulled the `AzureWebJobsStorage` connection string into `local.settings.json`

---

## ☁️ Deployment and Testing

1. **Deployed to Azure**:
   ```bash
   F1 → Azure Functions: Deploy to Function App
   ```

2. **Triggered the function in Azure**:
   ```bash
   F1 → Azure Functions: Execute Function Now...
   ```

   Request Body:
   ```json
   { "name": "Azure" }
   ```

3. ✅ Function successfully executed and wrote message to `outqueue`.

---

## 📦 Queue Verification

1. Opened **Azure Storage Explorer**
2. Connected to my Azure account
3. Navigated to:
   ```
   Storage Accounts → [my account] → Queues → outqueue
   ```
4. Verified the message `"Azure"` was successfully enqueued

---

## 💡 What I Learned

- How to write Azure Functions in Python using the v2 model
- How to configure and use output bindings to Azure Storage Queues
- How to run and deploy Azure Functions in a WSL development environment
- How to verify queue messages using Azure Storage Explorer

---



# Azure Function to Azure SQL Output Binding (Remote Demo)

This short guide demonstrates how to trigger a deployed Azure Function and verify its output in an Azure SQL Database — all done remotely.

---

## 🎯 Objective

- Execute the Azure Function from Visual Studio Code (remotely).
- Confirm the data is written to the `ToDo` table in Azure SQL Database.

---

## ✅ Step 1: Execute Function in Azure

### 💬 Short Script

> "I’m triggering the Azure Function deployed in the cloud using VS Code.  
> The function runs successfully and sends data to the Azure SQL Database."

### ▶️ Instructions

1. Open **VS Code**
2. Go to **Azure: Functions** tab (left sidebar)
3. Expand your function app → Right-click the function (e.g., `HttpTrigger1`)
4. Select **Execute Function Now**
5. Enter this request body:
```json
{ "name": "Azure" }
```
6. Press **Enter**

---

## ✅ Step 2: Verify Output in Azure SQL

### 💬 Short Script

> "Now, I’ll check the table using Query Editor.  
> And here’s the new row inserted — the output binding is working as expected."

### ▶️ Instructions

1. Go to **Azure Portal**
2. Navigate to your **SQL Database**
3. Open **Query editor**
4. Log in using your `azureuser` and password
5. Run:
```sql
SELECT * FROM dbo.ToDo;
```

6. ✅ You should see the new row with the `name` value you passed

---

## ✅ Closing Line (Optional)

> "This confirms that my Azure Function is successfully inserting data into the Azure SQL Database, all running in the cloud."

---


