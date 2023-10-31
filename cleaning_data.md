What issues will you address by cleaning the data?

Answer:
There seems to be many outliers, unreasonable or just outright wrong entries in many columns of the tables.
First and foremost, the problem tables will be used to build a view. The view's creation will double as the cleaning of the obvious troubles.

Lets break these down table by table, then by column.

all_sessions:

|Name of Column | Observed Possible Problems | Solutions|
|---|---|---|
|fullVisitorId| Nothing inherently wrong, but it feels like a foreign key from a table we do not have. | In a professional setting, a table might need to be made for visitors. For our purposes, no action needed.| 
| channelGrouping| Purpose not immediatly clear, but no obvious issues. | No action needed.|
| time | Nothing far out of the ordinary aside from seeming to be entirely tracked by seconds. | No action needed for now.|
| country | 24 rows have their country listed as "(not set)". | With such an insignificant amount, the rows marked "(not set)" will be excluded from the view.| 
| city |  354 rows "(not set)", 8302 rows "not available in demo dataset". In total over 57% is not accounted for. No null values. | The wording "not available in demo dataset" might imply that these would be filled in a professional setting. For our purposes, it would likely be best to keep the column and remain aware of it should any queries require this column.|
| totalTransactionRevenue | 15053 out of 15134 rows have null entries. | Column exempted from view |
| transactions | 15053 out of 15134 rows have null entries. | Column exempted from view |
|timeOnSite| 3300, or ~22% of entries null. Non-null entries presumably in seconds vary from 0 to 4661. | Set null entries to zero.|
| pageviews | No obvious issues. | No action needed. |
|sessionQualityDim | 13906 entries out of 15134 are null | Excluding column from view. |
| date | Stored as integer rather than datetime or formated string. | Will set format as "YYYY-MM-DD" when inputing to view.| 
| visitId | No Obvious problems. | No action needed|
| type | No obvious issues. | I personally question the column's purpose, but no action needed. |
| productRefundAmount | Column entirely null. | Excluding column from view.|
| product Quantity | 15081 entries out of 15134 are null. |Excluding column from view. |
| productPrice | All prices are set to multiple millions. By comparing the price to the v2ProductName and currencyCode columns, these prices are ludicrous.| When making the view, the entire column will be cast to double precision and divided by one million.| 
| productRevenue | 15130 entries out of 15134 are null. | Excluding column from view. |
| productSKU | No obvious problems, likely a foreign key from products. | No action needed.|
| v2ProductName | No obvious problems. | No action needed.|
| v2ProductCategory | 757 entries are "(not set)" | No inherent problem, no action needed. Remain aware in relevant queries.| 
| productVariant | 15094 entries out of 15134 are "(not set)" | While there is a "single option only" option, there is no column in the products table that denotes variants and such no way to know if the entries there are not set have varients or not. As such, this column will be excluded from the view. In a professional setting, a solution could be to have tables dedicated to every product capable of variants with foreign keys linking them to this table and products.|
| currencyCode | Entirely either "USD" or null regardless of country | Excluding column from view. Alternatively, potentially could set all nulls to "USD"| 
| itemQuantity | Column entirely null | Excluding column from view. |
| itemRevenue | Column entirely null | Excluding column from view. |
| transactionRevenue| 15130 entries out of 15134 are null. | Excluding column from view. |
| transactionId | 15125 entries out of 15134 are null. | Excluding column from view. |
| pageTitle | Entries can be incredibly long. | Nothing inherently wrong, no action needed.|
| searchKeyword | Column entirely null | Excluding column from view. |
| pagePathLevel1| 14318 entries out of 15134 are "/google+redesign/" | Nothing inherently wrong, no action needed 
| eCommerceAction_type | 14785 entries out of 15134 are "0", the majority remaining being "1" or "2" and the rest "3" through "5" | Highly likely the number is code for something, no action needed.|
| eCommerceAction_step | 15116 entries out of 15134 are "1", the rest being "2" or "3".| Highly likely the number is code for something, no action needed.|
| eCommerceAction_option | 15103 entries out of 15134 are null | Excluding column from view |

Analytics:

|Name of Column | Observed Possible Problems | Solutions|
|---|---|---|
|visitNumber| No obvious issues. | No action needed.|
|visitId| No obvious issues. | No action needed.|
|visitStartTime| No obvious issues. | No action needed.|
|date|Stored as integer rather than datetime or formated string. | Will set format as "YYYY-MM-DD" when inputing to view.| 
|fullvisitorId | Nothing inherently wrong, but it feels like a foreign key from a table we do not have. | In a professional setting, a table might exist or need to be made for visitors. For our purposes, no action needed.| 
|userid | Column is completely null.| Column excluded from view.|
|channelGrouping|Purpose not immediatly clear, but no obvious issues. | No action needed.|
|socialEngagementType| Entire column set to "Not Socially Engaged". | Column provides no substance, excluding from view.|
|units_sold| 4205975 entries or ~98% are null. | Column excluded from view.|
|pageviews| 72 entries are null, not even 0.002%. | Rows with nulls can be exempt from view without consequence.|
|timeonsite| 477465 entries are null, ~11% of the rows. | Null entries replaced with zero. Remain aware on relevent  queries.|
|bounces | 3826283 or ~89% of entries are null.| Column excluded from view.| 
|revenue| 4285767 or ~99.6% of entries are null. | Column excluded from view.|
|unit_price| All prices are multiplied by one million. | Will cast column as double precision and divide by one million when inserting into view.|

Products:

|Name of Column | Observed Possible Problems | Solutions|
|---|---|---|
|SKU| All entries unique, no nulls. | Will use column as primary key.|
|name| Some names are repeated multiple times, but all have unique skus.|No action needed.
|orderedQuantity| No obvious problems.| No action needed.|
|stockLevel|No obvious problems. |No action needed.|
|restokingLeadTime | No obvious problems. |No action needed.|
|sentimentScore | Single Null. | Despite other products having 0 in orderedQuantity, this is the only row with null score. Row exempt from view, loss insignificant.|
|sentimentMagnitude| Single Null, same row as sentimentScore's.| See above.|

sales_by_sku:
|Name of Column | Observed Possible Problems | Solutions|
|---|---|---|
| productSKU | All unique, no nulls, likely foreign key from products.|
| total_ordered| no obvious problems. | No action needed|


sales_report:
|Name of Column | Observed Possible Problems | Solutions|
|---|---|---|
| productSKU | All unique, no nulls, likely foreign key from products.|
| total_ordered| no obvious problems. | No action needed|
|name | Nothing inherently wrong, redundant due to existing primary key from products. All names relate to sku exactly as in products table. | In a professional setting, I would exclude the column. For our purposes, no action needed. |
|stockingLevel | Nothing inherently wrong, redundant due to existing primary key from products. | In a professional setting, I would exclude the column after ensuring there was no differences. For our purposes, no action needed. |
|restockingLeadTime | Nothing inherently wrong, redundant due to existing primary key from products.| In a professional setting, I would exclude the column after ensuring there was no differences. For our purposes, no action needed. |
|sentimentScore | No obvious issues, aside from redundency as previously elaborated. | No action needed|
|sentimentMagnitude | No obvious issues, aside from redundency. | No action needed|
|ratio | Entries become null when total_ordered and stockLevel are both 0. | Replace null values with 0.0 |


Queries:
Below, provide the SQL queries you used to clean your data.



Create view all_sessions_view AS
Select "fullVisitorId",
	"channelGrouping",
	"time",
	"country",
	"city",
	coalesce("transactions",'0') as "transactions",
	coalesce("timeOnSite",'0') as "timeOnSite",
	"pageviews",
	date("date"::text) as date,
	"visitId",
	"type",
	cast("productPrice" as double precision)/1000000 as "productPrice",
	"productSKU",
	"v2ProductName",
	"v2ProductCategory",
	"pageTitle"
	"pagePathLevel1",
	"eCommerceAction_type",
	"eCommerceAction_step"
from all_sessions
where country <> '(not set)' and
	city <> '(not set)'

Create view analytics_view As
Select 
	"visitNumber",
	"visitId",
	"visitStartTime",
	date("date"::text) As "date",
	"fullvisitorId",
	"channelGrouping",
	"pageviews",
	coalesce("timeonsite",0) as "timeonsite",
	cast(unit_price as double precision)/1000000 as "unit_price"
From analytics
Where "pageviews" Is Not Null 


Create view products_view as
Select
	"SKU",
	"name",
	"orderedQuantity",
	"stockLevel",
	"restockingLeadTime",
	"sentimentScore",
	"sentimentMagnitude"
From products
Where 
	"sentimentScore" Is Not Null and
	"sentimentMagnitude" Is Not Null


Create view sales_by_sku_view As
Select "productSKU", "total_ordered"
From sales_by_sku

Create View sales_report_view As
Select
	"productSKU",
	"total_ordered",
	"name",
	"stockingLevel",
	"restockingLeadTime",
	"sentimentScore",
	"sentimentMagnitude",
	coalesce("ratio", 0.0) as "ratio"
From sales_report
