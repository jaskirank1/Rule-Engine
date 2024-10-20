# Rule Engine Application with AST

## Overview
This MERN stack web application implements a rule engine using Abstract Syntax Trees (AST). The application determines user eligibility based on attributes like age, department, income, spend, etc. It allows for dynamic creation, combination, and modification of rules using a user-friendly interface. The system uses AST to represent conditional rules, ensuring efficient and flexible rule handling.

## Features
1. **Create Rule Page (Home Page):**
   - Users can define custom rules using logical operators and conditions.
   - The rule input area includes a placeholder with an example to guide users in writing correct rules, e.g., 
     `((age > 30 AND department = 'Sales') OR (age < 25 AND department = 'Marketing')) AND (salary > 50000 OR experience > 5)`.
   - Upon creating a rule, the application validates the input and stores the rule and its AST representation in the database.
   - Visualizes the AST using the `react-d3-tree` library.

2. **Edit Rules:**
   - Users can view all existing rules stored in the database via a dropdown menu.
   - The selected rule is displayed in a text area where users can modify it.
   - The updated rule is validated and, upon success, is saved to the database, and its AST is regenerated, updated and displayed  accordingly.

3. **Evaluate User Eligibility:**
   - Users can input values for attributes such as age, department, salary, and experience, along with an optional rule.
   - If no specific rule is provided, the system evaluates the eligibility based on a combined AST of all rules in the database.
   - Returns success or error messages based on the evaluation result.

## Tech Stack
- **Frontend:** React, react-d3-tree for AST visualization
- **Backend:** Node.js, Express.js for API handling and rule validation
- **Database:** MongoDB for rule and AST storage

## Installation Instructions
1. **Clone the repository:**
   ```bash
   git clone https://github.com/jaskirank1/Rule-Engine.git
   ```
2. **Navigate to the main project folder:**
   ```bash
   cd RULE\ ENGINE
   ```
3. **Install backend dependencies:**
   ```bash
   cd Backend
   npm install
   ```
4. **Install frontend dependencies:**
   ```bash
   cd ../RuleFrontend
   npm install
   ```

### Running the Application
1. **Start the backend server:**
   ```bash
   cd ../Backend
   npm start
   ```
2. **Start the frontend React app:**
   ```bash
   cd ../RuleFrontend
   npm run dev
   ```

## Database Schema
The rules are stored in MongoDB in the following structure:
- **type**: String indicating the node type (e.g., "operator" or "operand")
- **left**: Reference to another Node (left child for operators)
- **right**: Reference to another Node (right child for operators)
- **value**: Optional value for operand nodes (e.g., numerical value for comparisons)

## API Design

### 1. `create_rule(rule_string)`
- **Description**: This function takes a rule string as input, creates a new entry of the rule in the database, and returns a message along with the root node of the Abstract Syntax Tree (AST). If there are any errors during the process, it returns those errors as well. The input rule is also validated to ensure its correctness.
- **Input**: 
  - `rule_string`: A string representing the rule to be created.
- **Response**: 
  - On success: `{ message: "Rule created successfully", ast: ast }`
  - On error: `{ errors: [...] }`

### 2. `combine_rules(rules)`
- **Description**: This function is used to combine rules that are already present in the backend into a single Abstract Syntax Tree (AST). This combined AST can then be used to evaluate user eligibility based on the defined rules. This function is used internally in the backend.
- **Input**: 
  - `rules`: An array of rule strings to be combined.
- **Response**: 
  - Returns the root node of the combined AST.

### 3. `evaluate_rule(JSON data, rule)`
- **Description**: This function takes JSON input data containing user attributes and evaluates the user's eligibility based on a specified rule. The input data can include fields like age, department, salary, and experience. If a specific rule string is provided, it evaluates against that rule. If the rule is not passed, it defaults to using the combined AST created from all rules in the database to determine eligibility.
- **Input**:
  - `data`: A JSON object containing user attributes, e.g., `{"age": 35, "department": "Sales", "salary": 60000, "experience": 3}`
  - `rule` (optional): A string representing a specific rule to evaluate against.
- **Response**: 
  - Returns `true` if the user is eligible based on the rule, which will be reflected on the frontend. 
  - Returns `false` if the user is not eligible, also reflected on the frontend.

### Sample Rules
- **Rule 1**: `((age > 30 AND department = 'Sales') OR (age < 25 AND department = 'Marketing')) AND (salary > 50000 OR experience > 5)`
- **Rule 2**: `((age > 30 AND department = 'Marketing')) AND (salary > 20000 OR experience > 5)`

## Additional Features (Bonus Points)
1. **Error Handling:** The backend handles invalid rule strings or data formats (e.g., mismatched parentheses, invalid conditions) using `validateRule.js` in the utils folder.
2. **Attribute Validation:** Attributes are checked against a predefined catalog to ensure only valid attributes are included in the rules.
3. **Rule Modification:** Users can dynamically edit existing rules, including changing operators, operand values, or adding/removing sub-expressions through the Edit Rules page.
