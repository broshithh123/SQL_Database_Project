# SQL_Database_Project

## 📂 Project Structure

* **`clinic.db`**: The generated SQLite database file containing normalized tables, foreign keys, and over 1,000 transaction rows[cite: 8, 9, 10].
* **`SQL_Database_Project.pdf`**: The complete design, implementation, and data governance report detailing schema choices, data types, and ethical considerations[cite: 8, 9, 10, 13].
* **Python Generation Script**: Automated script utilizing a fixed random seed (`random.seed(42)`) to ensure full reproducibility[cite: 8].

---

## 📊 Database Architecture & Schema

The database consists of three normalized tables to eliminate data redundancy and enforce relational integrity[cite: 9, 10]:

1. **`doctors`**: Stores clinical specialist details.
   * `doctor_id` (INTEGER, Primary Key, Auto-Increment)
   * `doctor_name` (TEXT, Not Null)
   * `department` (TEXT, Not Null) — *Nominal Data*[cite: 10]
2. **`patients`**: Stores patient demographic and profile information.
   * `patient_id` (INTEGER, Primary Key, Auto-Increment)
   * `full_name` (TEXT, Not Null)
   * `contact_email` (TEXT, Unique, Nullable for missing data realism)[cite: 8, 10]
   * `birth_year` (INTEGER, Not Null, Bounded 1940–2024) — *Interval Data*[cite: 8, 10]
   * `insurance_status` (TEXT, Not Null, Checked: `Insured`, `Uninsured`, `State-Aided`) — *Nominal Data*[cite: 8, 10]
3. **`appointments`**: Central transactional table logging clinic visits.
   * `appointment_id` (INTEGER, Not Null)
   * `patient_id` (INTEGER, Not Null, Foreign Key referencing `patients` with `ON DELETE CASCADE`)[cite: 8, 10]
   * `doctor_id` (INTEGER, Not Null, Foreign Key referencing `doctors` with `ON DELETE CASCADE`)[cite: 8, 10]
   * `triage_priority` (TEXT, Not Null, Checked: `Low`, `Medium`, `High`, `Critical`) — *Ordinal Data*[cite: 8, 10]
   * `consultation_fee` (REAL, Not Null, $\ge 0$) — *Ratio Data*[cite: 8, 10]
   * `waiting_time_minutes` (INTEGER, Not Null, $\ge 0$) — *Ratio Data*[cite: 8, 10]
   * `appointment_date` (TEXT, Not Null)
   * **Composite Primary Key**: `(appointment_id, patient_id, doctor_id)`[cite: 8, 10]

---

## 📈 Key Project Features

* **Volume & Scaling**: Seeds **1,234 total appointment rows** (1,204 unique records plus 30 controlled duplicate transaction subsets), surpassing the 1,000-row minimum threshold[cite: 9].
* **Simulated Real-World Imperfections**: 
  * *Missing Data*: Patient email addresses feature intentional `NULL` values to simulate privacy opt-outs or incomplete registrations[cite: 8, 9].
  * *Duplicate Data*: Controlled transaction overlaps are introduced with unique composite keys mapped to model natural logging errors[cite: 9].
* **Strict Constraint Enforcement**: Utilizes `CHECK` constraints, `NOT NULL` rules, and cascading foreign keys to uphold relational integrity[cite: 8, 10].

---

## 🔒 Ethical Considerations & Data Privacy

* **Anonymization**: All patient and doctor names are synthetically generated via randomized dictionary scripts, ensuring zero exposure of Personally Identifiable Information (PII)[cite: 13].
* **Data Minimization**: Captures strictly operational parameters required for clinical queue tracking, omitting unnecessary sensitive personal attributes[cite: 13].
* **Compliance Alignment**: Optional email fields mirror professional data masking and user disengagement protocols consistent with GDPR standards[cite: 13].

---

## 🚀 Getting Started

To recreate or interact with the database locally:

1. Clone the repository or open the Google Colab environment provided in the report.
2. Execute the Python script to generate a fresh instance of `clinic.db`:
   ```bash
