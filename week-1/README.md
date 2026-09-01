## 🎯 Hands-on Class Activity: Let’s Build a Database Together
_Today’s mission: Create and explore a database using both Terminal and MySQL Workbench._

## 🖥️ Part 1: Using MySQL Workbench (a.k.a. Click & Code Mode 🖱️)

 🌀 **Step 1: Open Workbench & Connect:**
   - Open MySQL Workbench
   - Connect to your local database server

🧱 **Step 2: Create Your Database:**
  - Go to the SQL Editor
  - Paste this SQL command:
     
```sql
CREATE DATABASE <your_cool_database_name>;
```
- Click the ⚡ Execute button.

🔍 **Step 3: View Your Database:**
   - Refresh the Schemas section on the left
   - Find your database name there

💥 **Step 5: Drop a Database (Carefully):**
```bash
DROP DATABASE <your_cool_database_name>;
```
 _📣 Ask before running: Are you sure you want to delete it?_

## 💻 Part 2: Using the Terminal (a.k.a. Hacker Mode 😎)

🪄 **Step 1: Log in to MySQL:**
```bash
mysql -u root -p
```
_📌 Tip: type it in confidently_

🏗️ **Step 2: Create a New Database:**
```bash
CREATE DATABASE <your_cool_database_name>;
```
_🎉 Be creative with the name,Some fun ideas: school, journal, adventure._

👀 **Step 3: Check if It’s There:**
```bash
SHOW DATABASES;
```
_🕵️‍♂️ Can you find yours in the list?_

🎯 **Step 4: Start Using It:**
```bash
USE <your_cool_database_name>;
```
_💡 This tells MySQL, “Hey, I’m working on this one now."_

⚠️ **Step 5: Drop (Delete) a Database:**
```bash
DROP DATABASE <your_cool_database_name>;
```
_🚨 Warning: This command permanently deletes the database and everything inside it._
