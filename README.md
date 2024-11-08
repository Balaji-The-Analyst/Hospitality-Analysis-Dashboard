
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
**1.Domain (Hospitality)**

The hospitality domain encompasses industries and services that focus on providing comfortable, welcoming experiences for customers. This domain includes hotels, resorts, restaurants, tourism, event planning, and travel-related services. Hospitality businesses prioritize customer satisfaction, aiming to enhance guest experiences through quality service, convenience, and personal touches.

This project focuses on multiple five-star hotels offering luxury accommodations, high-end amenities, gourmet dining, and personalized experiences, ensuring guests enjoy unparalleled comfort and sophistication.

**2.Problem**

A company in the hospitality industry is losing market share and revenue in the luxury and business hotel sectors. Competitor strategies and ineffective management decisions have led to a decline in performance. Without an in-house data analytics team, they lack insights from historical data, hindering their ability to make data-driven decisions to improve operations.

**3.Goal of the project**

The goal is to analyze historical data to create a Power BI report and provide actionable insights that will help optimize pricing, improve room occupancy, and identify growth opportunities, helping the company regain market share and increase revenue.

**4.Dataset Information**

Luxury & Business Hotel’s “ historical room-booking “ data
- dim_date
- dim_hotels
- dim_rooms
- fact_aggregated_bookings
- fact_bookings
  
dim_date:
| Columns  | Data_Type  | 
| :-------- | --------: | 
| date | datetime | 
| month | String | 
| day_type | category |

dim_hotel:
| Columns | Data_Type | 
| :-------- | --------: | 
| property_id | String | 
| property_name | String | 
| category | String(category) | 
| city | String | 

dim_room:
| Columns | Data_Type | 
| :-------- | --------: | 
| room_id | String | 
| room_class | String | 

fact_aggregated_bookings:
| Columns | Data_Type | 
| -------- | --------: | 
| property_id | String | 
| check_in_date | date | 
| room_category | String(category) | 
| successful_bookings | Integer | 
| capacity | Integer | 

fact_bookings:
| Columns | Data_Type | 
| -------- | --------: | 
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
**1.Data Collection**

   *Data Source:* The data was downloaded in CSV format from the "codebasis" website, which contains relevant information for the project. 

  *Import Process:* The CSV file was imported into Power BI using the Get Data feature, selecting the Folder option to load the data.

*Import Mode:* The data was imported in Import Mode, where a copy of the data is loaded into Power BI's internal memory for faster querying and analysis.

**2.Data preparation**

In this phase, various data cleaning and transformation steps were applied to ensure data quality and consistency. Key steps include:

* *Data Type Conversion:* Adjusted column data types to ensure accurate representation and facilitate analysis.
* *String Corrections:* Standardized text data to maintain consistency across string columns
* *String Corrections:* Standardized text data to maintain consistency across string columns
* *Handling Missing Values:* Filled null values in the rating column with a default value of 0.
* *Date Extraction:* Extracted month and week from the main date column to enable time-based analysis.
* *Duplicate Removal:* Identified and removed duplicate records to maintain data accuracy.
* *Currency Formatting:* Converted revenue columns to currency format for clarity in financial analysis.

**3.Data Modeling**

In this project, a Star Schema data model was implemented to structure data efficiently for reporting and analysis. This model consists of fact and dimension tables, linked by key relationships to support analytical queries. Below is an overview of the data model.

![Star_Schema](https://github.com/user-attachments/assets/53f845d8-8879-48ef-badd-fc646ecec299)


These relationships enable quick and efficient querying of data across dimensions, supporting complex calculations and insights into hotel performance, booking patterns, and revenue.

**4.Data Analysis & Visualization**

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

***1.KPI Session:***

![KPI session](https://github.com/user-attachments/assets/b9539f81-d5f0-4ea5-9dc3-86f999c0ed55)

This KPI dashboard provides a quick overview of key hotel performance metrics. With revenue at 1709M and occupancy at 57.87%, there is room for growth as unutilized capacity is 42.13%. A high cancellation rate of 24.83% and no-show rate of 5.02% suggest opportunities to improve booking reliability. Total bookings of 135K show strong demand, though the average rating of 3.62 indicates a moderate level of customer satisfaction. The dashboard enables management to assess areas needing attention, such as occupancy and customer experience.

***2.Day Analysis:***

![Day](https://github.com/user-attachments/assets/e038c6ee-9aef-4e4d-a694-0d7d933bc145)

To analyze hotel performance differences between weekdays and weekends, focusing on booking volume, revenue, occupancy, and cancellations, supporting informed decisions for resource and strategy adjustments.

Used Power BI to create KPI cards for Weekday and Weekend metrics, including bookings, revenue, occupancy, cancellation, and rating, with filters isolating weekday vs. weekend data.

Weekends show higher occupancy (73.58%) but fewer bookings. Weekdays have more bookings and revenue (1070M) but lower occupancy. Consistent ratings indicate stable customer satisfaction across both periods.

***3.Revenue Contribution:***

![Revenue_contribution](https://github.com/user-attachments/assets/664059db-35d1-4950-ac64-b724b78118be)

This visual displays revenue contributions across cities, room types, booking status, and booking sources for a hotel chain.

I used Power BI’s bar, stacked bar, and donut chart visuals to break down revenue by city, hotel, room type, booking status, and source channels.

Mumbai contributes the most revenue (39.13%), Luxury rooms dominate (61.61%), and most bookings are checked out (82.46%). The largest booking source is Makeyourtrip at 19.95%.

***4.Weekly Trends For KPI’s:***

![Screenshot 2024-11-07 165902](https://github.com/user-attachments/assets/ee3843ee-977e-4def-8ff4-8ee7ebf2f8bf)

This visual tracks weekly trends in key metrics: revenue, occupancy, capacity, and cancellation rates.

Using Power BI’s line charts, I visualized weekly data to show fluctuations in revenue, occupancy percentage, capacity, and cancellations over time.

Occupancy rates and revenue remain stable until week 30, after which they drop significantly. Total bookings also decline from week 30, impacting overall performance.

***5.KPIs For Properties:***

![Screenshot 2024-11-07 174042](https://github.com/user-attachments/assets/1db9b049-f458-4e49-978e-c02269f5f550)

This table provides KPIs for each property, including bookings, revenue, occupancy, cancellation rates, and ratings.

I used a Power BI table visual to display property performance metrics, organized by property name and category for easy comparison.

Atliq Exotica has the highest revenue, while Atliq Blu has the highest occupancy (62.02%) and rating (3.96). Atliq Seasons has the lowest rating and occupancy rate.


***6.City & Rooms Class Analysis:***

![City   room analysis](https://github.com/user-attachments/assets/1d26ea5f-1f54-419f-ae81-60d7496f1d25)

*City:*

To analyze monthly revenue, occupancy, and cancellation rates across cities to optimize operational strategies based on location performance.

I used ribbon charts for revenue, occupancy, and cancellation rates, as well as metrics to highlight revenue per booking for each city.

Mumbai leads with the highest revenue per booking, while Bangalore shows the highest occupancy rates. Cancellation rates are fairly consistent across cities.

*Room Type:*

To evaluate revenue, occupancy, and cancellation trends by room class, aiding decisions on room pricing and inventory management.

Used ribbon charts for revenue, occupancy and cancellation rates, and metrics to highlight revenue per booking for each city.

Elite rooms contribute the most to revenue, while standard rooms have lower occupancy rates. Cancellation percentages remain stable across room types.

## Power BI Dashboard

![1](https://github.com/user-attachments/assets/c7fd91a9-f7c7-437f-8f74-3f69b6588d18)
![2](https://github.com/user-attachments/assets/1e2f52df-e7b4-4501-a166-2672f8486305)
![3](https://github.com/user-attachments/assets/e009d510-d2c5-47d0-8bcd-e0a8f95efaa0)

How this Power BI Report is helpful to stakeholders:

* Revenue Tracking: The dashboard provides a clear view of total revenue by property, room class, and city, helping stakeholders understand financial performance across different segments.
* Occupancy and Capacity Utilization: Stakeholders can monitor occupancy rates and unutilized capacity, enabling better decisions on resource allocation and pricing strategies.
* Cancellation and No-Show Analysis: With data on cancellation and no-show percentages, the dashboard helps identify areas for improvement in customer retention and booking policies.
* Booking Source Insights: By displaying booking percentages from various sources (e.g., direct online, third-party platforms), stakeholders can optimize marketing efforts based on channel performance.
* Performance by Room Class: The dashboard provides insights into booking percentages, occupancy, and revenue contribution by room type, supporting targeted pricing and inventory strategies.
* Property Comparison: Stakeholders can compare key metrics like revenue, occupancy, and ratings across different properties to identify high-performing and underperforming locations.
* Weekly Trends: The weekly trends for revenue, capacity, occupancy, and cancellations offer a dynamic view of performance, enabling timely interventions to address fluctuations.

**Interactive Dashboard:** https://www.novypro.com/profile_projects/novyprobalajipk?Popup=memberProject&Data=1699608698971x697001693791709800
