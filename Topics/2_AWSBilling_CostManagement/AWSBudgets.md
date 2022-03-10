## AWS Budgets

- Allows us to set custom budgets that will alert us when our costs or usage exceed or are forecasted to exceed your budgeted amount
- With Budgets, we can view the following information:
    - How close our plan is to our budgeted amount or to the free tier limits
    - Our usage to date, including how much you have used of your Reserved Instances and purchased Savings Plans
    - Our current estimated charges from AWS and how much your predicted usage will incur in charges by the end of the month
    - How much of our budget has been used
- Budget information is updated up to three times a day
- Types of Budgets:
    - **Cost budgets**: plan how much we want to spend on a service
    - **Usage budgets**: plan how much we want to use one or more services
    - **RI utilization budgets**: define a utilization threshold and receive alerts when your RI usage falls below that threshold
    - **RI coverage budgets**: define a coverage threshold and receive alerts when the number of your instance hours that are covered by RIs fall below that threshold
- Budgets can be tracked at the daily, monthly, quarterly, or yearly level, and we can customize the start and end dates
- Budget alerts can be sent via email and/or Amazon SNS topic
- First two budgets created are free of charge