---
name: NotebookEdit
---

Completely replaces the contents of a specific cell in a Jupyter notebook (.ipynb file) with new source. Jupyter notebooks are interactive documents that combine code, text, and visualizations, commonly used for data analysis and scientific computing. The notebook_path parameter must be an absolute path, not a relative path. The cell_number is 0-indexed. Use edit_mode=insert to add a new cell at the index specified by cell_number. Use edit_mode=delete to delete the cell at the index specified by cell_number.

---
# Tool Params
- notebook_path: string, The absolute path to the Jupyter notebook file to edit (must be absolute, not relative), required
- cell_id: string, The ID of the cell to edit. When inserting a new cell, the new cell will be inserted after the cell with this ID, or at the beginning if not specified., optional
- new_source: string, The new source for the cell, required
- cell_type: string, The type of the cell (code or markdown). If not specified, it defaults to the current cell type. If using edit_mode=insert, this is required., optional, enum: code | markdown
- edit_mode: string, The type of edit to make (replace, insert, delete). Defaults to replace., optional, enum: replace | insert | delete