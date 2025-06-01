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

