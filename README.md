# Test for Analytics

Design tests for Analytics functionality on a Battery Monitoring System.

## Analysis-functionality to be tested

Battery Telemetrics are collected and stored on a server.
Data for a month is stored in a [csv file](https://en.wikipedia.org/wiki/Comma-separated_values).

Analysis must be done on this csv file to find the following:
- minimum
- maximum
- count of breaches (how many times did it cross the threshold in a month?)
- record trends (date & time when the reading was continuously increasing for 30 minutes)

A PDF report of the analysis must be stored every week.
Notification must be sent when a new report is available.

## Tasks

### List Dependencies

List the dependencies of the Analysis-functionality.

1. Access to the Server containing the telemetrics in a csv file
2. Read access of the csv file.
3. A PDF generator
4. Write access to the server to store the generated PDF file
5. The csv file contains date, time and battery properties.
6. Threshold value to be measured against.
7. A third party notification utility


### Mark the System Boundary

What is included in the software unit-test? What is not? Fill this table.

| Item                      | Included?     | Reasoning / Assumption
|---------------------------|---------------|---
Battery Data-accuracy       | No            | We do not test the accuracy of data
Computation of maximum      | Yes           | This is part of the software being developed
Off-the-shelf PDF converter | No            | We do not test the pdf converter, we can test if report generation is invoked weekly
Counting the breaches       | Yes           | Counting of breaches is a major requirement of the software
Detecting trends            | Yes           | Trend detection is a major requirement of the software
Notification utility        | No| Whenever a new report is generated, a notification has to be sent. Whether a notification is generated on report generation has to be tested. But since notification utility is considered a third party, it need not be tested.

##### Operations Involved

​		Reading data from csv

​		Calculation of minimum and maximum from the data read

​		Breach Evaluation 

​		Detecting trends whenever there is a breach

​		PDF generation, every week

​		Notification has to be sent when PDF is generated

### List the Test Cases

Write tests in the form of `<expected output or action>` from `<input>` / when `<event>`

Add to these tests:

1. Write minimum and maximum to the PDF from a csv containing positive and negative readings

   ##### Reading data from csv

1. Values are expected as output given a csv file and a server.

   ##### Calculation of minimum and maximum from the data read

1. Minimum value calculation has to be tested from the csv values

1. Maximum value calculation has to be tested from the csv values

1. Calculated maximum and minimum value has to be written to PDF and success is expected

1. Write "Invalid input" to the PDF when the csv doesn't contain expected data during minimum / maximum calculation

   ##### Breach Evaluation 

1. Breach evaluation has to be tested whenever the value in the csv file crosses the threshold

1. Write "Invalid input" to the PDF when the csv doesn't contain expected data for breach evaluation.

1. Count of breaches must be returned from a list of csv values with breaches.

   ##### Detecting trends whenever there is a breach

1. Date and Time has to be returned whenever there is a breach for 30 mins continuously

1. Determined date and time value has to be written to PDF and success message is expected

1. Write "Invalid input" to the PDF when the csv doesn't contain expected data during trend evaluation.

   ##### PDF generation, every week

1. PDF generation invocation is to be tested for a particular number of times, given a timeframe.

   ##### Notification has to be sent when PDF is generated

1. Whenever a new pdf is generated, check if notification is invoked.

1. When the notification is invoked, check if the notification is received.

### Recognize Fakes and Reality

Consider the tests for each functionality below.
In those tests, identify inputs and outputs.
Enter one part that's real and another part that's faked/mocked.

| Functionality            | Input        | Output                      | Faked/mocked part
|--------------------------|--------------|-----------------------------|---
Read input from server     | csv file     | internal data-structure     | Fake the server store
Validate input             | csv data     | valid / invalid             | None - it's a pure function
Notify report availability | Generated pdf file | Notification to targets | Fake the notification utility  - only notification, message can be printed that report is available.
Report inaccessible server | server IP | error message | Fake an inaccessible server store
Find minimum and maximum   | csv data | calculated value  | None - The function just calculates the min / max and there is no state change
Detect trend               | csv data | date and time of trend occurrence  | None - The function just calculates trend and there is no state change
Write to PDF               | Minimum, maximum values, trend data, breach count | pdf file  | Mock the pdf service - PDF service has to be invoked weekly once with the required set of data. We will validate the data written to PDF, so we are using mock.

