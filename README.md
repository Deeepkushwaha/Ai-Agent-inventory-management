# AI-Powered Inventory & Stock Management Automation

An automated Inventory and Stock Management workflow built using **n8n, AI Agent, JavaScript, and Google Sheets**. The system processes an uploaded inventory CSV file, identifies products that need restocking, calculates the required quantity and estimated cost, and stores the results in separate Google Sheets reports.

## 📌 Project Overview

Managing inventory manually can be time-consuming and may lead to stock shortages or unnecessary purchases.

This project automates inventory analysis by checking the current stock of each product against its reorder level. It separates low-stock products from products with sufficient stock and makes the results available through Google Sheets. An AI Agent can also retrieve inventory information and answer questions using connected Google Sheets tools.

## 🎯 Objectives

- Automate inventory analysis using n8n.
- Accept inventory data through a CSV upload form.
- Identify products that require restocking.
- Calculate the number of units needed and estimated restocking cost.
- Separate low-stock and sufficient-stock products.
- Store reports in Google Sheets.
- Use an AI Agent to answer inventory-related questions.
- Reduce manual effort and improve inventory visibility.

## 🛠️ Technologies Used

- **n8n:** Workflow automation and orchestration.
- **AI Agent:** Natural-language inventory assistance.
- **OpenRouter:** Chat model integration.
- **JavaScript:** Stock calculations and data transformation.
- **Google Sheets:** Inventory reports and data retrieval.
- **CSV:** Inventory input format.

## 🏗️ Workflow Architecture

```text
CSV Upload Form
       |
       v
Extract Data from CSV
       |
       v
Check Current Stock
against Reorder Level
       |
       v
   IF Condition
       |
       +------------------------+
       |                        |
       v                        v
  Low Stock              Sufficient Stock
       |                        |
       v                        v
JavaScript Calculation   Data Transformation
       |                        |
       v                        v
Restock Google Sheet     Sufficient Stock Sheet
       |                        |
       +-----------+------------+
                   |
                   v
             Merge Results
                   |
                   v
               AI Agent
                   |
          +--------+--------+
          |                 |
          v                 v
    OpenRouter Model    Google Sheets Tools
          |                 |
          +--------+--------+
                   |
                   v
        Inventory Information
          and AI Responses
```

## ⚙️ How It Works

### 1. Upload Inventory CSV

The workflow starts when a user submits the inventory form and uploads a CSV file containing product information.

Expected input columns:

| Column | Description |
|---|---|
| `product_id` | Unique product identifier |
| `product_name` | Product name |
| `current_stock` | Available stock quantity |
| `reorder_level` | Minimum stock threshold |
| `unit_price` | Price per unit |

### 2. Extract CSV Data

The Extract from File node converts the uploaded CSV file into structured records that can be processed by subsequent nodes.

### 3. Check Stock Levels

The IF node evaluates the following condition for each product:

```javascript
Number(current_stock) <= Number(reorder_level)
```

- **True:** The product is sent to the restock branch.
- **False:** The product is sent to the sufficient-stock branch.

### 4. Calculate Restocking Requirements

The JavaScript Code node calculates the required quantity and estimated restocking cost.

```javascript
units_needed = Math.max(0, reorder_level - current_stock);

estimated_cost = units_needed * unit_price;
```

For example, if a product has 5 units in stock, its reorder level is 20, and its unit price is ₹100:

- Current stock: 5
- Reorder level: 20
- Units needed: 15
- Estimated restocking cost: ₹1,500

These calculations assume the reorder level is the target quantity. A business may choose a different target-stock policy.

### 5. Generate Google Sheets Reports

The workflow saves the processed records into two separate Google Sheets tabs.

**Restock Report**

Contains products that need restocking, including:

- Product ID
- Product name
- Current stock
- Reorder level
- Unit price
- Units needed
- Stock status
- Estimated cost

**Sufficient Stock Report**

Contains products whose current stock is above the reorder level, including product ID, product name, current stock, reorder level, and unit price.

### 6. AI Agent Integration

The AI Agent uses an OpenRouter chat model and connected Google Sheets tools to retrieve inventory information and respond to inventory-related questions.

Example questions:

- Which products need restocking?
- Show me the products with sufficient stock.
- What is the estimated restocking cost for a product?
- What inventory information is available in the reports?

The answers depend on the data returned by the connected Google Sheets tools.

## 📊 Sample Input

Create a CSV file named `sample_inventory.csv`:

```csv
product_id,product_name,current_stock,reorder_level,unit_price
P001,Laptop,5,10,45000
P002,Mouse,50,20,500
P003,Keyboard,8,15,1200
P004,Monitor,25,10,12000
P005,Headphones,4,12,1500
P006,Webcam,18,10,2500
P007,USB Cable,6,25,200
P008,Printer,7,10,8500
P009,Office Chair,30,15,6000
P010,External SSD,3,10,7500
```

## 🧪 Expected Results

Using the sample input above:

| Product | Current Stock | Reorder Level | Status |
|---|---:|---:|---|
| Laptop | 5 | 10 | Low Stock |
| Mouse | 50 | 20 | Sufficient Stock |
| Keyboard | 8 | 15 | Low Stock |
| Monitor | 25 | 10 | Sufficient Stock |
| Headphones | 4 | 12 | Low Stock |
| Webcam | 18 | 10 | Sufficient Stock |
| USB Cable | 6 | 25 | Low Stock |
| Printer | 7 | 10 | Low Stock |
| Office Chair | 30 | 15 | Sufficient Stock |
| External SSD | 3 | 10 | Low Stock |

**Expected summary:**

- Total products: 10
- Products requiring restocking: 6
- Products with sufficient stock: 4
- Total estimated restocking cost: ₹2,77,800

*The estimated cost uses the difference between reorder level and current stock multiplied by unit price.*

## 🚀 Setup and Configuration

### Prerequisites

- An n8n instance (self-hosted or cloud).
- A Google account with Google Sheets access.
- An OpenRouter account and API key for the configured chat model.
- An inventory CSV file following the expected column format.

### Step 1: Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/ai-agent-inventory-management.git
cd ai-agent-inventory-management
```

Replace `YOUR_USERNAME` with your GitHub username.

### Step 2: Import the n8n Workflow

1. Open your n8n instance.
2. Create or open a workflow.
3. Use the workflow import option.
4. Import `workflows/inventory_stock_management.json`.
5. Review the imported nodes and connections.

### Step 3: Configure Google Sheets

1. Connect your Google Sheets account in n8n.
2. Select the correct spreadsheet document in each Google Sheets node.
3. Verify the `Restock` and `Sufficient Stock` sheet tabs.
4. Confirm the column headers match the configured field mappings.
5. Test reading and appending records.

### Step 4: Configure OpenRouter

1. Set up your OpenRouter API credentials in n8n.
2. Select the configured chat model.
3. Verify that the AI Agent is connected to the chat model, memory, and Google Sheets tools.
4. Test an inventory-related question.

### Step 5: Execute the Workflow

1. Open the inventory upload form.
2. Upload `sample_inventory.csv`.
3. Execute the workflow.
4. Review the IF node outputs.
5. Verify both Google Sheets reports.
6. Test the AI Agent response.

**Note:** Credentials are configured separately in n8n and are not included in the exported workflow JSON.

## 📁 Repository Structure

```text
ai-agent-inventory-management/
├── data/
│   └── AI_Agent_Workflow_Assessment.xlsx
├── workflows/
│   └── inventory_stock_management.json
├── examples/
│   └── sample_inventory.csv
├── screenshots/
│   └── workflow-and-reports.png
├── README.md
└── .gitignore
```

Include the original assignment Excel file only if you are permitted to share it. Add screenshots if available. Remove unused folders or files if they are not included in your repository.

## 🔍 Testing Checklist

- [ ] Upload a CSV containing multiple products.
- [ ] Verify that all CSV records are extracted.
- [ ] Verify the low-stock and sufficient-stock conditions.
- [ ] Confirm the units-needed and estimated-cost calculations.
- [ ] Confirm records are appended to the correct Google Sheets tabs.
- [ ] Ask the AI Agent an inventory-related question.
- [ ] Verify the response against the actual sheet data.
- [ ] Test missing or invalid CSV columns.
- [ ] Check the behavior when Google Sheets or the model returns an error.

## ⚠️ Limitations and Future Improvements

Potential improvements include:

- Dynamic workflow selection based on user intent.
- Input validation and more robust error handling.
- Duplicate-record prevention and inventory update logic.
- Low-stock alerts through email or messaging platforms.
- Purchase-order generation and approval workflows.
- A configurable reorder target rather than treating the reorder level as the desired stock quantity.

This implementation focuses on the Inventory/Stock Management workflow selected for the assignment. It does not claim to implement the other workflows in the assessment.

## 🔐 Security

- Never upload API keys, passwords, OAuth tokens, or private credentials to GitHub.
- Keep credentials inside n8n's credential manager.
- Use a `.gitignore` file to exclude secrets and local configuration files.
- Avoid publishing private customer or inventory data.

## 👨‍💻 Author

**Deepak Kumar**

Aspiring AI/ML Engineer | AI Automation | Data Science

LinkedIn: https://linkedin.com/in/deepak-kushwaha-4105b5351

## 📌 Project Summary

This project demonstrates how workflow automation, JavaScript-based business logic, Google Sheets integration, and an AI Agent can work together to automate inventory analysis and restocking reports.

The solution reduces repetitive manual work and provides a foundation for extending inventory operations with additional automation capabilities.
