# Calculations

##### Number of Customers
Total count of unique customers.

    COUNTD([Customer ID])

##### Customer LTV (avg)
The sum of sales the average customer will make over their lifetime.

    AVG( { FIXED [Customer ID] : SUM([Sales]) } )

##### Null Invoices
Tagged orders where the invoice number is missing.

    ISNULL([Invoice No])

##### Invoice Total Quanity
Summed sales on order LOD.

    { FIXED [Invoice No] : SUM([Quantity] ) }

##### Product Description
After self-joining, found that product marked "Indoor Pet Camera" has two slightly different descriptions. This calculated field standarized it into a single description.

    IF [Description] = 'Indoor Pet Camera (Wi-Fi)'
    THEN 'Indoor Pet Camera'
    ELSE [Description]
    END

##### COGS
Calculation for the cost of goods sold.

    [Landed Cost] * [Quantity]

##### Shipping (Baseline)
The base formula currently used for calculating shipping costs.

    [Shipping Cost 1000 mile] + (([Quantity] - 1) * [Shipping Cost 1000 mile] * 0.7)

##### Profit (Baseline)
The profit formula, using current shipping cost model.

    [Sales] - [COGS] - [Shipping (Baseline)]

##### Profit %
Profit as a percentage of total revenue.

    SUM([Profit (Baseline)]) / SUM([Sales])

##### Blended Shipping Cost Factor
Parameter-controlled variable coefficient for the "What-If" shipping formula.

    IF [What-If Quantity] <= 1 THEN 1
    ELSEIF  [What-If Quantity] <= 2 THEN 0.8
    ELSEIF  [What-If Quantity] <= 4 THEN 0.6
    ELSEIF  [What-If Quantity] <= 7 THEN 0.5
    ELSEIF  [What-If Quantity] <= 9 THEN 0.4
    ELSE 0.3
    END

##### Shipping (What-If)
Proposed shipping cost model, using What-If variable coefficient.

    [Shipping (Baseline)] - [Shipping (What-If)]

##### Shipping (Difference)
The difference in savings/losses between the baseline and what-if shipping models.

    [Shipping (Baseline)] - [Shipping (What-If)]

##### Warehouse-LAX
Geographic coordinate for the Los Angeles warehouse.

    MAKEPOINT(34.0544, -118.2439)

##### Destination
Geographic coordinate for the shipping destination of the order.

    MAKEPOINT([Latitude],[Longitude])

##### Destination-LAX
Distance in miles between the LAX Warehouse location and the customer's shipping destination.

    DISTANCE([Warehouse-LAX], [Destination], 'miles')
