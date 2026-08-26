# Goals

- Predict if the customer will subscribe to a term deposit
- Determine the segment(s) of customers who are more likely to buy the investment product.
- Determine most important feature that makes the customers buy

# Target Variable

- Term Deposit

Y => has the client subscribed to a term deposit?

# Features

| Name      | Type        | Description                                                             |
| --------- | ----------- | ----------------------------------------------------------------------- |
| age       | numeric     | age of customer                                                         |
| job       | categorical | type of job                                                             |
| marital   | categorical | marital status                                                          |
| education | categorical | level of education                                                      |
| default   | binary      | whether the customer has credit in default                              |
| balance   | numeric     | average yearly balance, in euros                                        |
| housing   | binary      | whether the customer has a housing loan                                 |
| loan      | binary      | whether the customer has a personal loan                                |
| contact   | categorical | type of contact communication                                           |
| day       | numeric     | last contact day of the month                                           |
| month     | categorical | last contact month of the year                                          |
| duration  | numeric     | duration of the last contact, in seconds                                |
| campaign  | numeric     | number of contacts performed during this campaign includes last contact |
