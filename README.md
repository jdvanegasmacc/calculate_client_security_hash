# Client Security Hash - UiPath REFramework

This project is a solution to the **Client Security Hash** exercise from UiPath's **RPA Developer Advanced Training**, using the **Robotic Enterprise Framework (REFramework)**. It demonstrates key concepts in building a transactional automation solution, such as secure credential handling, exception handling, data scraping, and transaction item processing.

## 🧠 Problem Statement

The robot must log into the **ACME Test platform**, extract open `WI5` work items, process each one by generating a SHA1 hash from specific data, and update the work item with the result and a "Completed" status.

## 📌 Objective

Automate the end-to-end process of:
1. Logging in securely to the ACME Test system.
2. Scraping all `WI5` items with **Status = "Open"**.
3. For each item:
   - Extracting **Client ID**, **Client Name**, and **Client Country**.
   - Concatenating the information in the format:  
     `ClientID-ClientName-ClientCountry`
   - Generating a SHA1 hash of the formatted string using an external tool.
   - Posting the hash as a comment and updating the status of the work item to **"Completed"**.
4. Logging out and closing all applications.

---

## ⚙️ Technologies & Tools

- **UiPath Studio** – REFramework template
- **UiPath.System.Activities**
- **UiPath.UIAutomation.Activities**
- **SHA1 Hash Generator** – `http://www.sha1-online.com/`
- **ACME Test System** – `https://acme-test.uipath.com`
- **Credential Management** – Windows Credential Manager (via `Get Secure Credential` activity)

---

## 🗂️ Project Structure

- `Main.xaml`: Main entry point, handles initialization, transaction loops, and closure.
- `InitAllApplications.xaml`: Logs into ACME using credentials from Windows Credential Manager and opens browser session.
- `GetTransactionData.xaml`: Scrapes the list of WI5 work items and feeds the next one as a transaction item.
- `Process.xaml`: Processes a single WI5 item:
   - Navigates to the item’s detail page.
   - Extracts client information and generates the hash.
   - Updates the work item status with the generated hash.
- `SetTransactionStatus.xaml`: Handles marking transaction as `Success`, `Business Exception`, or `System Exception`.
- `Config.xlsx`: Centralized configuration file with URLs and asset names.

---

## 📈 Key Features

- ✅ REFramework implementation with proper state transitions
- 🔒 Secure credential handling using Credential Manager
- 🔄 Dynamic scraping with each transaction iteration (to reflect real-time updates)
- 📄 Detailed logging using `Log Message` activities for traceability
- 🧪 Fully testable and modular design
- 🛠️ Recovery-ready: Exception handling and retry mechanisms in place

---

## 💡 Notes & Considerations

- The browser session is reused across all workflows via a shared `UiPath.Core.Browser` variable (`ACMEBrowser`).
- Each transaction reopens the work items page to scrape fresh data. While slightly inefficient, this aligns with the REFramework's goal of processing each item independently and safely.
- If scraping fails (e.g., due to UI loading issues), appropriate exceptions are thrown and logged.

---

## 🚀 How to Run

1. Open the solution in UiPath Studio.
2. Set your credentials in **Windows Credential Manager** under the name `ACME_Credential`.
3. Update `Config.xlsx` with:
   - ACME URLs
   - Asset and credential names
4. Run the project using `Main.xaml` in **Debug** or **Run File** mode.

---

## 📸 Example Output
```
For the input:
Client ID: OZ56329
Client Name: Anna Dudas
Client Country: Romania
```

The formatted string becomes:  
`OZ56329-Anna Dudas-Romania`  
→ SHA1 hash is generated and submitted as a comment on the work item.

---

## 📚 Resources

- [UiPath Advanced RPA Developer Certification Training](https://academy.uipath.com)
- [SHA1 Online Tool](http://www.sha1-online.com/)
- [UiPath REFramework Documentation](https://docs.uipath.com)

---

## 👤 Author

**David (aka Mapache / Deivis44)**  
Passionate about automation, system administration, and learning DevOps with real-world projects.  
🔗 [GitHub](https://github.com/Deivis44) | 🌐 [Portfolio](https://ukiyo44.neocities.org/)

---

## 🧼 License

This project is intended for educational and portfolio purposes. Adapt freely, but please credit the author if reused publicly.

# UiPath Readme
### Documentation is included in the Documentation folder ###


### REFrameWork Template ###
**Robotic Enterprise Framework**

* Built on top of *Transactional Business Process* template
* Uses *State Machine* layout for the phases of automation project
* Offers high level logging, exception handling and recovery
* Keeps external settings in *Config.xlsx* file and Orchestrator assets
* Pulls credentials from Orchestrator assets and *Windows Credential Manager*
* Gets transaction data from Orchestrator queue and updates back status
* Takes screenshots in case of system exceptions


### How It Works ###

1. **INITIALIZE PROCESS**
 + ./Framework/*InitiAllSettings* - Load configuration data from Config.xlsx file and from assets
 + ./Framework/*GetAppCredential* - Retrieve credentials from Orchestrator assets or local Windows Credential Manager
 + ./Framework/*InitiAllApplications* - Open and login to applications used throughout the process

2. **GET TRANSACTION DATA**
 + ./Framework/*GetTransactionData* - Fetches transactions from an Orchestrator queue defined by Config("OrchestratorQueueName") or any other configured data source

3. **PROCESS TRANSACTION**
 + *Process* - Process trasaction and invoke other workflows related to the process being automated 
 + ./Framework/*SetTransactionStatus* - Updates the status of the processed transaction (Orchestrator transactions by default): Success, Business Rule Exception or System Exception

4. **END PROCESS**
 + ./Framework/*CloseAllApplications* - Logs out and closes applications used throughout the process


### For New Project ###

1. Check the Config.xlsx file and add/customize any required fields and values
2. Implement InitiAllApplications.xaml and CloseAllApplicatoins.xaml workflows, linking them in the Config.xlsx fields
3. Implement GetTransactionData.xaml and SetTransactionStatus.xaml according to the transaction type being used (Orchestrator queues by default)
4. Implement Process.xaml workflow and invoke other workflows related to the process being automated
