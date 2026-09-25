# 📚 HTML Table Tags

## 📁 Topics

- 🌐 HTML Table Basics
- 🔹 Table Structure Tags
- 🧱 Rows & Cells
- 🏷️ Headers & Caption
- 📊 Column Grouping
- ✅ Best Practices

---

## 📘 Notes

---

### 🌐 HTML Table Basics

🧠 **Concepts**

HTML tables are used to display data in a structured format using **rows and columns**, just like an Excel sheet. They are mainly used when you have **tabular data**, such as student records, pricing tables, reports, etc.

A table is created using the `<table>` tag, and everything inside it (rows, columns, headings) is organized using specific tags.

---

### 🔹 Table Structure Tags

These tags define the **overall structure of a table**:

- `table` → This is the main container. Every table starts and ends with this tag.
- `thead` → Used to define the **top section** of the table, usually containing headings.
- `tbody` → Contains the **main data** of the table (most of the rows).
- `tfoot` → Represents the **bottom section**, often used for totals or summaries.

👉 Using these tags makes your table **more readable, structured, and SEO-friendly**.

---

### 🧱 Rows & Cells

Tables are built using rows and cells:

- `tr` (Table Row) → Defines a row in the table. Each row contains multiple cells.
- `td` (Table Data) → Defines a normal data cell inside a row.

👉 Think of it like:

- `tr` = one horizontal row
- `td` = individual boxes inside that row

---

### 🏷️ Headers & Caption

- `th` (Table Header) → Used for headings (like Name, Age, Price).
    - By default, it is **bold and centered**, which makes it visually different from normal data.
- `caption` → Used to give a **title or description** to the table.
    - It appears above the table and helps users understand what the table is about.

---

### 📊 Column Grouping

These are advanced tags used for styling or managing columns:

- `colgroup` → Used to group multiple columns together
- `col` → Used to define properties for individual columns

👉 These are mostly used when you want to apply **same styling to multiple columns at once**.

---

💻 **Code Example**

```
<tableborder="1">
<caption>Student Data</caption>

<thead>
<tr>
<th>Name</th>
<th>Age</th>
</tr>
</thead>

<tbody>
<tr>
<td>Anubhav</td>
<td>24</td>
</tr>
<tr>
<td>Rahul</td>
<td>23</td>
</tr>
</tbody>

<tfoot>
<tr>
<tdcolspan="2">End of Data</td>
</tr>
</tfoot>
</table>
```

👉 This example shows:

- Header (`thead`)
- Data (`tbody`)
- Footer (`tfoot`)

---

📎 **Resources**

- Add YouTube explanation link
- Add MDN Docs link

---

📝 **Summary**

- Tables are used to display structured data
- `table` is the main container
- `tr` creates rows, `td` creates data cells
- `th` is used for headings
- `thead`, `tbody`, `tfoot` improve structure

---

## ✅ Best Practices

- Always use `thead`, `tbody`, `tfoot` for better structure
- Use `th` instead of `td` for headings
- Add a `caption` to describe the table
- Keep tables clean and readable
- Avoid using tables for layout (use CSS instead)

---

🔥 **Pro Tip**

> Tables should only be used for **data representation**, not for designing layouts. Use CSS (Flexbox/Grid) for layout.
>