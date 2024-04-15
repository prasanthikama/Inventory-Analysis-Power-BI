# Inventory-Analysis-Power-BI
 The goal of this project is to increase sales by analyzing inventory data.
Inventory Analysis
In this project, you will:
•	Prepare your data ready for analysis using Power Query
•	Write intermediate DAX functions for data manipulation

Cost of goods sold, or COGS: refers to the cost of producing or acquiring products that a company sells in a determined period.
COGS calculated column
COGS=[factory_labor]+[factory_equipment_rent]+[raw_material]

Revenue : Revenue is the amount of money a company receives in exchange for its goods and services. The simplest way companies calculate revenue is by multiplying the total number of units sold by their selling price.

Caluculate column Revenue _2020year
Revenue _2020year=[2020_units_sold]*[Retail_Price]

Profit: The gross profit is the amount resulting after subtracting costs associated with making or acquiring products. It is calculated by subtracting COGS from total revenue.
Caluculate column for profit_2020
profit_2020= [Revenue_2020]-[COGS]

Average value of inventory=starting value+ending value/2
Inventory turnover =COGS/Avg value of inventory
ABC Analysis
1.Revenue
2.percentage of total value.
3.cumulative increase
4.ABC
What is ABC Analysis: A items will cover up to seventy percent of total revenue, B items will cover the following twenty percent, and C items will represent the remaining percentage
Revenue_2020=Stock[2020_units_sold]*Stock[Retail_Price]
profit_2020 = Stock[2020_units_sold]*Stock[Retail_Price]-Stock[2020_units_sold]*Stock[COGS]
Total_orders = CALCULATE(sum(Orders[Quantity]),FILTER(orders,Stock[SKU-ID]=Orders[SKU]))

Quantity_2021 = 
CALCULATE(
    SUM(Orders[Quantity]),
    FILTER(
        Orders,
        Orders[SKU] = Stock[SKU-ID] && FORMAT(Orders[Year], "0") = "2021"
    )
)

Quantity_2022 = 
CALCULATE(
    SUM(Orders[Quantity]),
    FILTER(
        Orders,
        Orders[SKU] = Stock[SKU-ID] && FORMAT(Orders[Year], "0") = "2022"
    )
)

Inventory_turn over caluculating 
Creating Table using Dax formula
Turnover = SELECTCOLUMNS(stock,[SKUID],[Description],[2021_start_stock],[Quantity_2021],[COGS])

Avg_inventory = (Turnover[2021_Start_stock] + Turnover[2021_End_stock])*Turnover[COGS] / 2

2021_End_stock = Turnover[2021_Start_stock] - Turnover[Quantity_2021]

Inventory_turnover =Turnover[COGS]*Turnover[Quantity_2021]/Turnover[Avg_inventory]


Revenue_2021 = Stock[Quantity_2021]*Stock[Retail_Price]

percent_revenue_2020 = Stock[Revenue_2020]/sum(Stock[Revenue_2020])*100
percent_revenue_2021 = Stock[Revenue_2021]/sum(Stock[Revenue_2020])*100


ABC Analysis 
Selectcolumns is used to get columns from other tables
ABC = SELECTCOLUMNS(stock,Stock[SKU-ID],Stock[Description],Stock[Revenue_2021],Stock[percent_revenue_2021])
Cumulative revenue caluculation
cp_revenue = CALCULATE(sum(ABC[Stock_percent_revenue_2021]),filter(ABC,ABC[Stock_percent_revenue_2021]>=EARLIER(ABC[Stock_percent_revenue_2021])))
creating column to classify iteam with ABC Analysis
ABC_class = 
    IF(ABC[cp_revenue] <= 70, "A",
        IF(ABC[cp_revenue] <= 90, "B",
            "C"
        )
    )






