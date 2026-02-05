# Estimate & Proforma Invoice App

A lightweight, client-side web application for generating professional estimates and proforma invoices for **Smart Ohm**. The app allows users to select products from an Excel-based database, manage a shopping cart, and generate PDF invoices.

## Technical Flow

The application follows a modern client-side architecture using standard web technologies:

1.  **Frontend Layout**: Built with **HTML5** and **Vanilla CSS**. The UI is designed to be responsive and mobile-friendly, featuring a containerized layout with sections for customer details, item selection, price details, and cart management.
2.  **Logic Layer**: Written in **Vanilla JavaScript** (`script.js`). It handles DOM manipulation, event listening, and data processing without the overhead of heavy frameworks.
3.  **Data Source**: Utilizes an external Excel file (`data.xlsx`) as a flexible, non-volatile database. This allows non-technical users to update product prices and categories easily.
4.  **Core Libraries**:
    -   **[SheetJS (XLSX)](https://sheetjs.com/)**: Used to fetch and parse the `data.xlsx` file directly in the browser.
    -   **[jsPDF](https://github.com/parallax/jsPDF)**: Handles the generation of the PDF document on the client side.
    -   **[jsPDF-AutoTable](https://github.com/simonbengtsson/jspdf-autotable)**: A plugin for jsPDF to render the cart details as a clean, structured table within the invoice.
    -   **Font Awesome**: Provides vector icons for UI elements like the delete action in the cart.

## Data Flow

The movement of data through the application follows these steps:

### 1. Data Initialization
Upon loading, the application executes `fetchExcelData()`, which:
-   Fetches `data.xlsx` via the Fetch API.
-   Converts the binary data into a JSON structure using the `xlsx` library.
-   Processes the rows to populate hierarchical dropdowns (Category -> Subcategory -> Sub-Subcategory).

### 2. Product Selection & Pricing
When a user selects a product:
-   The app retrieves the **Cost Price (CP)** and **Selling Price (SP)** from the parsed JSON.
-   Users select a **Protocol** (WiFi, ZigBee, etc.), which adds a fixed cost/selling price based on predefined constants in `script.js`.
-   Users can adjust quantities for the item itself and optional components like **Sockets** and **RJ45** ports.

### 3. Cart Management
Clicking "Add to Cart":
-   Calculates the item's total CP and SP: `(Base + Protocol + Extras) * Quantity`.
-   Appends the item as a row in the HTML `cart-table`.
-   Updates the global totals: **Total Selling Price**, **Total Cost Price**, and the calculated **Profit**.
-   Recalculates values dynamically when items are edited or deleted.

### 4. Discounting & Final Calculation
-   Users can input a percentage discount.
-   The app dynamically updates the **Final Amount** and **Profit** as the user types, ensuring real-time visibility into the margins.

### 5. Invoice Generation
When "Generate Proforma Invoice" is triggered:
-   The app collects customer metadata (Name, Phone, Email, Address).
-   It serializes the cart's visible data (excluding internal cost prices).
-   `jsPDF` renders a structured document with the **Smart Ohm** branding, customer details, a professional table of items, and a summary of the final amount.

---

## Deployment
This project is configured for deployment via **GitHub Pages**. Any changes pushed to the repository are automatically built and deployed using GitHub Actions.
