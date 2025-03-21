---
title: Naming Convention
---
[Home](Home) | [Style Guide](Style-Guide)
---
# Controller Naming Convention  

Consistent naming conventions are crucial for creating maintainable and readable code. Follow these guidelines to ensure clarity and uniformity across all controllers.  

## Action Naming: CRUD-Based  
Use a **CRUD-based naming convention** for actions inside controllers. For more details, refer to the [CRUD-based naming convention guide](Style-Guide/Controllers/Cruddy-By-Design#crud-based-naming-convention).  

## File Naming Convention  
To maintain consistency and readability, adhere to the following file naming rules:  

1. **Use PascalCase**  
   Ensure file names follow the PascalCase format.  

2. **Start with the Persona**  
   Always make the **Persona** the first word in the file name.  
   - **Example**:  
     ```scala
     UserSession.scala ✅  
     SessionUser.scala ❌  
     ```  

3. **Be Descriptive**  
   File names should clearly describe their purpose.  

### Example Folder Structure  
![Folder Structure Example](uploads/87f32dacc736fbe41a0d58f8c39dfc73/image.png)  
--- 
[Previous:Controllers](Style-Guide/Controllers)