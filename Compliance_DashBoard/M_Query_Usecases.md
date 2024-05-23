## M Query Applications (To go beyond what Power Query UI offers)

### List of Lists :

Products and Topics are multi-choice fields in Tracker App :

![image](https://github.com/Arpit-Agrawal-Git/Power-BI-Dashboards/assets/88269770/60833bba-9a60-4f20-9bc9-4cb4844fb3da)


To display Products and Topics in single cell value :

![image](https://github.com/Arpit-Agrawal-Git/Power-BI-Dashboards/assets/88269770/694a2dcb-3b79-4856-b789-025f65ef197f)


"Grouped Rows" = Table.Group(#"Filtered Rows", {"ID","Created_Date", "Trainer"}, {{"PV", each List.Combine({[Product_Category.Value]}), type text}}),

{{"PV", each List.Combine({[Product_Category.Value]}), type text}} : 
This is the aggregation specification of the Table.Group function. 
It's a list of lists where each inner list defines an aggregation to perform on the grouped data.
   - "PV" is the name of the new column to be created.
   - each List.Combine({[Product_Category.Value]}) defines the aggregation function. List.Combine takes a list of lists and combines them into a single list. 
      In this case, for each group, it takes all values from the Product_Category.Value column and combines them into a single list. 
      The [Product_Category.Value] notation is a way of dynamically accessing the Product_Category.Value field from each row in the group.
   - type text suggests that the resulting combined list is expected to be treated as text.

![image](https://github.com/Arpit-Agrawal-Git/Power-BI-Dashboards/assets/88269770/cb4fed94-e490-435a-987f-8c349f886f71)

Next we add a customer column joining each element of the above list with separator as new line character.

    #"Added Custom" = Table.AddColumn(#"Grouped Rows", "Custom", each Text.Combine(List.Transform([PV],Text.From),"
"))  <- new line 

....each Text.Combine(List.Transform([PV],Text.From),"\n")* : 

This part of the function defines the transformation for each row to generate the values for the new column
  - *each* : This keyword indicates that the following function is to be applied to each row of the table.
  - *Text.Combine(List.Transform([PV], Text.From), "\n")* : This is the function applied to each row:
    - *List.Transform([PV], Text.From)* : This transforms a list by applying a function to each item in the list.
    - Here, it converts each item in the list [PV] from each row into text. The [PV] is a column presumably created in an earlier step that contains lists.                Text.From is a function that converts various types of values into text.
    - *Text.Combine(..., "\n")* : This function takes the transformed list (now a list of text strings) and concatenates (combines) all the items into a single             text string, with each item separated by a newline character (\n).

Final Output

![image](https://github.com/Arpit-Agrawal-Git/Power-BI-Dashboards/assets/88269770/cdb1711b-ec61-4691-a876-8e48a1e803a6)


