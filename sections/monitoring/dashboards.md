# Monitoring and logs dashboards

APEX Cloud provides the Elastic ELK using the [StackOps](https://docs.developer.tech.gov.sg/docs/stackops-documentation) monitoring tool. ELK includes pre-made dashboards for reporting and debugging issues.

You can use these dashboards to:

- Monitor API request metrics over different time ranges.
- Track and monitor trace logs
- Monitor application performance and
- Provide operational transparency for effective incident response

## View dashboards

To view the dashboards, follow these steps:

1. Access your StackOps account.

   - Production: [go.gov.sg/apex-report](https://go.gov.sg/apex-report)
   - Staging: [go.gov.sg/apex-report-stg](https://go.gov.sg/apex-report-stg)

2. Log in with [TechPass](sections/onboarding/techpass). The Elastic Cloud dashboard is displayed.

3. From the Spaces menu, select your project space.

4. From the main menu, go to the Analytics category and click **Dashboard**. A list of available dashboards are displayed in the Dashboards page.

   ![Viewing dashboards](./_assets/dashboards_intro.gif)

5. From the list of dashboards, select any of the following monitoring dashboards.

   - API-Requests Real-Time
   - API-Requests Quarterly
   - API-Requests Yearly

   ?> **Note:** You can use the search functionality to filter the list of dashboards.

See the sections below to know more about these dashboards.

- [API-Requests Real-Time](sections/monitoring/real-time-dashboards)
- [API-Requests Quarterly and Yearly](sections/monitoring/quarterly-and-yearly-dashboards)

Aside from the monitoring dashboards, you can also view and use the Traffic Trace dashboard to troubleshoot API issues. For more information, refer to the [APEX Cloud Troubleshooting Guide](sections/troubleshooting/introduction).
