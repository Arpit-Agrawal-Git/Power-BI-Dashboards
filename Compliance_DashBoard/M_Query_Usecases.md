## M Query Applications (To go beyond what Power Query UI offers)

### List of Lists :

"Grouped Rows" = Table.Group(#"Filtered Rows", {"ID","Created_Date", "Trainer"}, {{"PV", each List.Combine({[Product_Category.Value]}), type text}}),

{{"PV", each List.Combine({[Product_Category.Value]}), type text}} : 
This is the aggregation specification of the Table.Group function. 
It's a list of lists where each inner list defines an aggregation to perform on the grouped data.
   - "PV" is the name of the new column to be created.
   - each List.Combine({[Product_Category.Value]}) defines the aggregation function. List.Combine takes a list of lists and combines them into a single list. 
      In this case, for each group, it takes all values from the Product_Category.Value column and combines them into a single list. 
      The [Product_Category.Value] notation is a way of dynamically accessing the Product_Category.Value field from each row in the group.
   - type text suggests that the resulting combined list is expected to be treated as text.
