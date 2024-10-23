# Emergency-Room-Visits
**This repository demonstrates how Data Analysis Expressions (DAX) were utilized to create a comprehensive and interactive Power BI dashboard. The dashboard provides insightful analysis of Emergency Room Visits at a hospital over a two-year period, showcasing key metrics and trends to support data-driven decision-making.**

![Er Visits](https://github.com/Herola007/ER-Visits/blob/main/Er%20Illustration.jpg?raw=true)

## DAX Formulas Created
The hospital's objective was clear and concise, but to meet their goals effectively, I went the extra mile by creating custom DAX formulas. These formulas enabled the detailed and insightful analysis needed to fulfill the hospital's requirements.

- **Patient Visits**: The hospital wanted to know the total number of Emergency Room (ER) visits they had over the past two years.
   ```dax
   Patient Visits = COUNTROWS('Hospital ER')
   ```
- **Patient Visits Prevoius Month**: This formula calculates the total number of patient visits for the previous month based on the selected date context in the hospital's data. It allows the hospital to compare the current month's data with the previous month's data over the two-year period.
   ```dax
   Patient Visits PM = CALCULATE([Patient Visits],PREVIOUSMONTH('Calendar'[Date]))
   ```
- **Patient Visits Variance**:  It allows the hospital to track changes in ER visit volumes month to month, helping them identify trends, anticipate staffing or resource needs, and respond to fluctuations in patient visits over time.
   ```dax
   Patient Visits Variance = [Patient Visits] - [Patient Visits PM]
   ```
- **Patient Visits Growth**: It helps the hospital assess how visit volumes are changing over time, and also provides the hospital with the growth rate of patient visits, expressed as a percentage.
   ```dax
   Patient Visit Growth = DIVIDE([Patient Visits Variance],[Patient Visits PM],0)
   ```
- **Patient Visits Max Value**: This was created to allows the hospital to identify whether the current month's patient visits match the maximum recorded visits for any day in the dataset. 
  ```dax
   Patient Visits Max Value = 
    VAR _value = 
    MAXX(
        ALL('Calendar'[Day]),
        [Patient Visits])

    
    VAR _check=
        IF(_value = [Patient Visits],_value,BLANK()
        )
  RETURN
  _check
   ```
- **Patient Visits MIN Value**: This was created just like the maximum to allow the hospital to identify whether the current month's patient visits match the minimum recorded visits for any day in the dataset.
  ```dax
  Patient Visits Min Value = 
    VAR _value = 
    MINX(
        ALL('Calendar'[Day]),
        [Patient Visits])

    
    VAR _check=
        IF(_value = [Patient Visits],_value,BLANK()
        )
  RETURN
  _check
   ```
- **Male Patient**: This calculation provides the hospital with the total count of male patients who visited the ER, enabling them to analyze gender distribution among patients. This information can assist in understanding patient demographics, tailoring services to specific needs.
    ```dax
    Male Patient = CALCULATE(COUNT('Hospital ER'[Gender]),'Hospital ER'[Gender]="Male")
    ```
- **Female Patient**: Similar to the previous formula, this one provides the hospital with the total count of female patients who visited the ER.
   ```dax
    Female Patient = CALCULATE(COUNT('Hospital ER'[Gender]),'Hospital ER'[Gender]="Female")
    ```
- **Average Wait Time**: This insight helps identify trends over time. By calculating the average wait time, the hospital can assess the efficiency of its ER services.
   ```dax
    Avg Wait Time = AVERAGE('Hospital ER'[Wait Time])
    ```
- **Average Wait Time Previous Month**: This calculation allows the hospital to analyze the average wait time specifically for the previous month, facilitating comparisons with current month data.
   ```dax
    Avg. Waiting Time PM = CALCULATE([Avg Wait Time],PREVIOUSMONTH('Calendar'[Date]))
    ```
- **Average Wait Time Variance**: This calculation helps the hospital track changes in average wait time from month to month. By understanding these fluctuations, the hospital can assess the effectiveness of any changes implemented in their processes.
  ```dax
    Avg. Waiting Time Variance = [Avg Wait Time] - [Avg. Waiting Time PM]
    ```
- **Average Wait Time Growth**: The hospital can assess whether wait times are increasing or decreasing relative to the previous month. This insight helps in evaluating the effectiveness of operational changes, and identifying areas for improvement.
  ```dax
    Avg. Waiting Time Growth = DIVIDE([Avg. Waiting Time Variance],[Avg. Waiting Time PM],0)
    ```
- **Busiest Day**: This formula identifies the busiest day in the Emergency Room (ER) by determining which day of the week had the highest number of patient visits. This insight allows them to optimize staffing, manage resources effectively, and improve patient care during peak times, ultimately enhancing operational efficiency.
  ```dax
  Busiest Day = 
    CALCULATE(
        MAXX(
            TOPN(
                1,
                SUMMARIZE(
                    'Hospital ER',
                    'Calendar'[DayName],
                    "Total Patients", [Patient Visits]
                ),
                [Total Patients],
                DESC
            ),
            'Calendar'[DayName]
        )
    )
   ```
- **Busiest Time**: This insight enables the hospital to optimize staffing levels, allocate resources effectively, and improve patient care during high-demand hours. Ultimately, this can lead to reduced wait times and enhanced operational efficiency.
    ```dax
    Busiest Time = 
    CALCULATE(
        MAXX(
            TOPN(
                1,
                SUMMARIZE(
                    'Hospital ER',
                    'Hospital ER'[Hours],
                    "Total Patients", [Patient Visits]
                ),
                [Total Patients],
                DESC
            ),
            FORMAT('Hospital ER'[Hours], "h:mm:AM/PM")
        )
    )
    ```
- **Average Satisfaction Score**: This formula calculates the average satisfaction score of patients who visited the Emergency Room (ER). By determining this average, the hospital can evaluate overall patient satisfaction with its ER services. This insight enables the hospital to identify areas for improvement, track trends over time, and make data-driven decisions aimed at enhancing patient experience and care quality.
   ```dax
    Avg. Satisfaction Score = AVERAGE('Hospital ER'[Satisfaction Score])
    ```
- **Average Satisfaction Score Previous Month**: This formula calculates the average satisfaction score of patients who visited the Emergency Room (ER) in the previous month.
This helps in identifying trends in patient satisfaction over time, allowing the hospital to assess the effectiveness of any changes made in service delivery. Monitoring this metric can also guide strategic initiatives aimed at improving patient care and enhancing the overall experience.
   ```dax
    Avg. Satisfaction Score PM = CALCULATE([Avg. Satisfaction Score],PREVIOUSMONTH('Calendar'[Date]))
    ```
- **Average Satisfaction Score Variance**: This formula calculates the variance in the average satisfaction score of patients visiting the Emergency Room (ER) by comparing the current month's average satisfaction score to that of the previous month. A positive variance indicates an improvement in patient satisfaction, while a negative variance suggests a decline. This insight allows the hospital to identify trends, assess the impact of any changes made to services or processes, and make informed decisions to enhance patient experience and care quality. Tracking this variance can help the hospital ensure consistent high levels of patient satisfaction over time.
  ```dax
    Avg Satisfaction Score Variance = [Avg. Satisfaction Score] - [Avg. Satisfaction Score PM]
    ```
- **Average Satisfaction Score Growth**: By calculating the growth rate of the average satisfaction score, the hospital can assess the percentage change in patient satisfaction over the month. This metric provides valuable insight into how changes in services or processes are impacting patient perceptions and experiences.
  ```dax
    Avg. Satisfaction Score Growth = DIVIDE([Avg Satisfaction Score Variance],[Avg. Satisfaction Score PM],0)
    ```

With the aid of these carefully crafted DAX measures, I can now proceed to create a comprehensive and insightful dashboard. This dashboard will effectively visualize key metrics related to Emergency Room visits, such as average wait times, patient satisfaction scores, and trends over time.


## Dashboard
![Dashboard](https://github.com/Herola007/ER-Visits/blob/main/Dashboard.png?raw=true)

![Summary](https://github.com/Herola007/ER-Visits/blob/main/Summary.png?raw=true)

The interactive nature of the dashboard will allow hospital administrators to filter data by specific time periods, patient demographics, and other relevant factors, fostering a deeper understanding of patient experiences and needs. Ultimately, this comprehensive dashboard will serve as a vital tool for the hospital to monitor its performance, drive continuous improvement, and ensure high-quality patient care.

**Link to the interactive dashboard**: https://app.powerbi.com/groups/me/reports/5bb85799-72c2-4465-9852-7f98d7b36512/f5c8728527aa5904d809?experience=power-bi
