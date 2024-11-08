
# REVENUE INSIGHTS
**Session** 

    1.Project Details
        - Domain short description
        - Problem
        - Goal
        - Dataset Information
    2.Data work (What I  did)
        - Data collection
        - Data Cleaning
        - Data Modeling
        - Data Analysis & Visualization
    3.Power BI Dashboard
        - Dashboard 
        - InterActive Dashboard Link
## PROJECT DETAILS 
***1.Domain (Hospitality)***

The hospitality domain encompasses industries and services that focus on providing comfortable, welcoming experiences for customers. This domain includes hotels, resorts, restaurants, tourism, event planning, and travel-related services. Hospitality businesses prioritize customer satisfaction, aiming to enhance guest experiences through quality service, convenience, and personal touches.

This project focuses on multiple five-star hotels offering luxury accommodations, high-end amenities, gourmet dining, and personalized experiences, ensuring guests enjoy unparalleled comfort and sophistication.

***2.Problem***

A company in the hospitality industry is losing market share and revenue in the luxury and business hotel sectors. Competitor strategies and ineffective management decisions have led to a decline in performance. Without an in-house data analytics team, they lack insights from historical data, hindering their ability to make data-driven decisions to improve operations.

***3.Goal of the project***

The goal is to analyze historical data to create a Power BI report and provide actionable insights that will help optimize pricing, improve room occupancy, and identify growth opportunities, helping the company regain market share and increase revenue.

***4.Dataset Information***

Luxury & Business Hotel’s “ historical room-booking “ data
- dim_date
- dim_hotels
- dim_rooms
- fact_aggregated_bookings
- fact_bookings
  
dim_date:
| Columns  | Data_Type  | 
| -------- | -------- | 
| date | datetime | 
| month | String | 
| day_type | category |

dim_hotel:
| Columns | Data_Type | 
| -------- | -------- | 
| property_id | String | 
| property_name | String | 
| category | String(category) | 
| city | String | 

dim_room:
| Columns | Data_Type | 
| -------- | -------- | 
| room_id | String | 
| room_class | String | 

fact_aggregated_bookings:
| Columns | Data_Type | 
| -------- | -------- | 
| property_id | String | 
| check_in_date | date | 
| room_category | String(category) | 
| successful_bookings | Integer | 
| capacity | Integer | 

fact_bookings:
| Columns | Data_Type | 
| -------- | -------- | 
| booking_id | String | 
| property_id | String | 
| booking_date | date | 
| check_in_date | date | 
| check_out_date | date | 
| no_guests | Integer | 
| room_category | String(category) | 
| booking_platform | String(category) | 
| ratings_given | decimal | 
| booking_status | String |  
| revenue_generated | Integer | 
| revenue_realized | Integer | 


## Data/Project work (What I  did)
***1.Data Collection***

*Data Source:* The data was downloaded in CSV format from the "codebasis" website, which contains relevant information for the project. 

**Import Process:** The CSV file was imported into Power BI using the Get Data feature, selecting the Folder option to load the data.

**Import Mode:** The data was imported in Import Mode, where a copy of the data is loaded into Power BI's internal memory for faster querying and analysis.

***2.Data preparation***

In this phase, various data cleaning and transformation steps were applied to ensure data quality and consistency. Key steps include:

* **Data Type Conversion:** Adjusted column data types to ensure accurate representation and facilitate analysis.
* **String Corrections:** Standardized text data to maintain consistency across string columns
* **String Corrections:** Standardized text data to maintain consistency across string columns
* **Handling Missing Values:** Filled null values in the rating column with a default value of 0.
* **Date Extraction:** Extracted month and week from the main date column to enable time-based analysis.
* **Duplicate Removal:** Identified and removed duplicate records to maintain data accuracy.
* **Currency Formatting:** Converted revenue columns to currency format for clarity in financial analysis.

***3.Data Modeling***

In this project, a Star Schema data model was implemented to structure data efficiently for reporting and analysis. This model consists of fact and dimension tables, linked by key relationships to support analytical queries. Below is an overview of the data model and its components:

**Fact Table:**

***fact_bookings:***
Stores transactional data for individual bookings, including information such as booking ID, booking date, check-in and check-out dates, room category, booking platform, number of guests, ratings, booking status, and revenue-related details.

***fact_aggregated_bookings:*** Contains aggregated booking information by property, room category, and date, capturing total bookings and room capacity for each day.

**Dimension Tables:**

***dim_hotels:*** Describes hotel properties with attributes like property ID, name, category (Luxury or Business), and city location.

***dim_rooms:*** Defines room types and classifications, including room ID and room class (e.g., Standard, Elite, Premium, Presidential).

***dim_date:*** A date dimension table that breaks down dates into attributes like day, month, week number, day type (weekday or weekend), and formatted month names, to support time-based analysis.

**Relationships:**

Each fact table is linked to relevant dimension tables through foreign keys, establishing a one-to-many relationship. For example:
* fact_bookings and fact_aggregated_bookings tables are connected to dim_hotels via the property_id key.
* Both fact tables link to dim_rooms via room_category and to dim_date via check_in_date.

These relationships enable quick and efficient querying of data across dimensions, supporting complex calculations and insights into hotel performance, booking patterns, and revenue.

***3.Data Analysis & Visualization***

**KPIs :**  Revenue, Occupancy, No Show, Total Bookings, Cancellation, Capacity & Unutilized Capacity.

**DAX :**

``` 
1.Monthly Bookings Change % = 
VAR CurrentMonthRevenue = [Total Bookings]
VAR PreviousMonthRevenue =
    CALCULATE(
        [Total Bookings],
        PREVIOUSMONTH(dim_date[date])
    )
RETURN
    IF(
        ISBLANK(PreviousMonthRevenue),
        " ",
        DIVIDE(CurrentMonthRevenue - PreviousMonthRevenue, PreviousMonthRevenue) 
    )
```

```
2.Previous_Month_Revenue = 
    CALCULATE(
        [Revenue],
        DATEADD(dim_date[date], -1, MONTH)
    )
```

```
3.Cancellation for weekeday = 
CALCULATE([Cancellation%], 'dim_date'[day_type] IN { "weekeday" })
```

```
4.Cancellation% = DIVIDE(CALCULATE(
	COUNTA('fact_bookings'[booking_status]),
	'fact_bookings'[booking_status] IN { "Cancelled" }
),COUNT(fact_bookings[booking_status]))
```
```
5.Revenue_In_Millions = FORMAT([Revenue]/1000000,0.0) & "M"
```

```
6.Seelcted_City = 
IF(
    ISFILTERED(dim_hotels[city]),
    SELECTEDVALUE(dim_hotels[city]),
    "All Cities"  -- Default value when no filter is applied
)
```

```
7.Selected_month = 
VAR CurrentMonth = SELECTEDVALUE(dim_date[Month Name])
RETURN
    SWITCH(
        TRUE(),
        CurrentMonth = "May", "May",
        CurrentMonth = "June", "June",
        CurrentMonth = "July", "July",
        "All Months"
    )
```
![App Screenshot](https://drive.google.com/file/d/1rHMal-Mz_bzfMMZNB45DLJtoTqkESaqy/view?usp=drive_link)













