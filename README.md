
# 📄 README

## UiPath - KMD Nova Document Transfer Robot

**KMD Nova Document Transfer** is a UiPath process developed for **Teknik og Miljø, Aarhus Kommune**. It automates retrieval of case and document metadata from KMD Nova, performs fuzzy matching to ensure document consistency, downloads files, uploads them to Filarkiv, and logs processing results in the central database.

---

## 🚀 Features

✅ **KMD Nova Integration**  
- Retrieves secure access tokens automatically  
- Queries case and document metadata via Nova REST API  

📄 **Data Retrieval and Validation**  
- Locates all documents linked to a specific case  
- Fetches metadata for each document (title, type, date)  
- Uses Levenshtein distance to match and validate document names and types, tolerating small typos or formatting differences  

📥 **Download & Transfer**  
- Downloads documents from KMD Nova  
- Uploads documents to Filarkiv automatically  

📝 **Database Logging**  
- Records processing status, timestamps, and any errors to SQL Server  

🔐 **Credential Management**  
- Manages all secrets via Orchestrator Assets  

---

## 🧭 Process Flow

1. **Access Token Retrieval**  
   - Fetches a valid KMD Nova token (`GetAccessToken.xaml`)  

2. **Case Identification**  
   - Queries KMD to retrieve the Case UUID (`GetCaseUuid.xaml`)  

3. **Metadata Retrieval**  
   - Collects case details (`GetKMDWebInfo.xaml`)  
   - Gathers all related document metadata (`GetCaseAndDocumentInfo.xaml`, `GetDocumentInfo_New.xaml`)  

4. **Fuzzy Document Matching (Levenshtein)**  
   - Compares document titles and types against expected patterns  
   - Accepts matches within a configurable Levenshtein distance threshold  
   - Ensures robust classification even when minor variations are present  

5. **Document Transfer**  
   - Downloads validated documents  
   - Sends them to Filarkiv (`SendDocumentsToFilarkiv.xaml`)  

6. **Database Logging**  
   - Logs success or failure details (`LogInfoToDataBase.xaml`)  

---

## ✨ Levenshtein Algorithm Usage

The process uses **Levenshtein distance** to perform *fuzzy matching* of document titles.  
This ensures documents are correctly recognized even if their names differ slightly (e.g., spacing, typos).

**Example Logic:**
- For each retrieved document:
  - Compare its title against known expected labels
  - Calculate Levenshtein distance
  - If the distance is less than e.g., 3, consider it a match
  - Otherwise, flag for manual review

---

## 🔐 Privacy & Security

- All API calls are over HTTPS  
- Credentials and tokens are stored in Orchestrator Assets  
- No sensitive data is exported or persisted locally  

---

## ⚙️ Dependencies

- UiPath Studio 2021.10+ (or newer)
- UiPath.System.Activities
- UiPath.WebAPI.Activities
- UiPath.Excel.Activities
- UiPath.Database.Activities
- (Optional) String Similarity / Levenshtein library if implemented via custom activities

---

## 🗂️ Main Workflows

| Workflow File                    | Purpose                                             |
|----------------------------------|-----------------------------------------------------|
| **Process.xaml**                 | Main dispatcher workflow                            |
| **GetAccessToken.xaml**          | Retrieves KMD Nova access token                     |
| **GetCaseUuid.xaml**             | Retrieves Case UUID                                 |
| **GetKMDWebInfo.xaml**           | Extracts case-level metadata                        |
| **GetCaseAndDocumentInfo.xaml**  | Retrieves all documents linked to a case            |
| **GetDocumentInfo_New.xaml**     | Fetches detailed document metadata and performs fuzzy matching |
| **SendDocumentsToFilarkiv.xaml** | Uploads downloaded documents to Filarkiv            |
| **LogInfoToDataBase.xaml**       | Records processing outcomes to SQL Server           |

---

## 👷 Maintainer

Gustav Chatterton  
*Digital udvikling, Teknik og Miljø, Aarhus Kommune*
