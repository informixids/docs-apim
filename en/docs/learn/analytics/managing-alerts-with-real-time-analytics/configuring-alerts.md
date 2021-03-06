# Configuring Alerts
    
WSO2 API Manager analytics-based alerts are disabled by default. Once you [enable alerts](../../../../learn/analytics/managing-alerts-with-real-time-analytics/configuring-alerts/#enable-alerts), you can customize and configure the analytics-based alerts using the features listed below.

-   Configure sender email alerts
-   Configure alerts as business rules
-   Configure alerts via the Publisher
-   Configure alerts via the Developer Portal

Once you have configured alerts, you can subscribe to alerts to receive email notifications. For instructions, see [Subscribing for Alerts](../../../../learn/analytics/managing-alerts-with-real-time-analytics/subscribing-for-alerts/) .

!!! note
     Before you begin, make sure that you have configured Analytics for API Manager. For instructions, see [Configuring APIM Analytics](../../../../learn/analytics/configuring-apim-analytics/).

### Enable alerts

WSO2 API Manager analytics-based alerts are disabled by default. Follow the instructions below to enable alerts.

1.  Shut down the analytics server if it is running.
2.  Open the `<API-M_ANALYTICS_HOME>/conf/worker/deployment.yaml` file.
3.  Uncomment the following configuration.
    ``` java
        analytics.solutions:
          APIM-alerts.enabled: true
    ```
4.  Go to the `<API-M_ANALYTICS_HOME>/bin` directory and run the following command to start the worker, to deploy the alerts siddhi applications.

    -   On Windows: `worker.bat --run` 
    -   On Linux: `sh worker.sh`

### Configure sender email alerts

!!! note
    If you do not require to subscribe to email notifications for alerts, you can skip this step for configuring sender email address. Admin users can view analytics in the admin portal(`https://<API-M_HOST>:<API-M_PORT>/admin`) without configuring the sender email address.
     
The users of your APIs can subscribe to analytics-related alerts from the API Publisher and the API Developer Portal. Follow the instructions below to configure an email address with API Manager to send email alerts to subscribers.

1.  Open the `<API-M_ANALYTICS_HOME>/conf/worker/deployment.yaml` file.
2.  Navigate to the `extensions` configuration under `siddhi` configurations.
3.  Add a new extension to configure the sender email address. The sample code is shown below.

    ``` java
        siddhi:
          extensions:
        ...
            -
              extension:
                name: email
                namespace: sink
                properties:
                  username: alex@gmail.com
                  address: alex@gmail.com
                  password: password 
        ...
    ```

    !!! warning
          Note that you might have to bypass a security warning to configure this with a private email address.


3.  Go to the `<API-M_ANALYTICS_HOME>/resources/apim-analytics/` directory. Copy the `APIM_ALERT_EMAIL_NOTIFICATION.siddhi` file and paste it in the `<API-M_ANALYTICS_HOME>/wso2/worker/deployment/siddhi-files` directory.
4.  Restart the servers.

### Configure alerts as business rules

You can configuring alerts as business rule by using the features listed below.

-   View deployment information of alerts
-   View alerts configuration
-   Edit alerts configuration
-   Delete alerts configuration
-   Undeploy alerts configuration

!!! note
    Before you begin, make sure that the worker instance of the Analytics server is running.

1.  Get a new distribution of API-M Analytics.
2.  Go to `<API-M_ANALYTICS_HOME>/wso2/dashboard/resources/dashboards` and remove the `apimdevportal.json` and `apimpublisher.json` files.
3.  Open the `<API-M_ANALYTICS_HOME>/conf/dashboard/deployment.yaml` file and do the following ;
       
    - Comment out the authentication configuration `auth.configs` as shown below.

    ``` java
        ## Authentication configuration
            #auth.configs:
            #  type: apim
            #  ssoEnabled: true
            #  properties:
            #    adminScope: apim_analytics:admin_carbon.super
            #    allScopes: apim_analytics:admin apim_analytics:product_manager apim_analytics:api_developer apim_analytics:app_developer apim_analytics:devops_engineer apim_analytics:analytics_viewer apim_analytics:everyone openid apim:api_view apim:subscribe
            #    adminServiceBaseUrl: https://localhost:9443
            #    adminUsername: admin
            #    adminPassword: admin
            #    kmDcrUrl: https://localhost:9443/client-registration/v0.15/register
            #    kmTokenUrlForRedirection: https://localhost:9443/oauth2
            #    kmTokenUrl: https://localhost:9443/oauth2
            #    kmUsername: admin
            #    kmPassword: admin
            #    portalAppContext: analytics-dashboard
            #    businessRulesAppContext : business-rules
            #    cacheTimeout: 900
            #    baseUrl: https://localhost:9643
            #    grantType: authorization_code
            #    publisherUrl: https://localhost:9443
            #    #storeUrl: https://localhost:9443
    ```
    
    - Point the business rules manager to the worker node by configuring the `deployment_configs` of `wso2.business.rules.manager` as shown below.
    
    ``` java
        wso2.business.rules.manager:
          datasource: BUSINESS_RULES_DB
          # rule template wise configuration for deploying business rules
          deployment_configs:
            -
             # <IP>:<HTTPS Port> of the Worker node
             <API-M_ANALYTICS_WORKER_HOST>:<API-M_ANALYTICS_WORKER_PORT>
          ...
    ```
4.  Go to the `<API-M_ANALYTICS_HOME>/bin` directory and run the following command to start the dashboard.

    -   On Windows: `dashboard.bat --run` 
    -   On Linux: `sh dashboard.sh`

5.  Go to the Business Rules and Status Dashboard. ( e.g., `https://<API-M_ANALYTICS_HOST>:9643/business-rules`)
6.  You can view the existing business rules that are applied for API Manager. Depending on your privileges, you can view, edit, and delete business rules.
    For more details on working with business rules, see [Managing Business Rules](https://ei.docs.wso2.com/en/latest/streaming-integrator/admin/creating-business-rules-templates/#managing-business-rules) .
    ![Alerts business rules](../../../assets/img/learn/alerts-business-rules.png)

### Configure alerts via the Publisher

Follow the instructions below to manage alerts via the Publisher:

- [Create an abnormal response time alert](../../../../learn/analytics/managing-alerts-with-real-time-analytics/configuring-alerts/#create-an-abnormal-response-time-alert/)
- [Create an abnormal backend time alert](../../../../learn/analytics/managing-alerts-with-real-time-analytics/configuring-alerts/#create-an-abnormal-backend-time-alert/)

!!! info
     You can not disable Health Availability related alerts, because they are enabled by default. However, you can enable and disable the email alerts that correspond to the Health Availability alerts.

#### Create an abnormal response time alert

1.  Log into the API Publisher with the username and password of a user with required permission.
2.  Click on the **SETTINGS** menu, to open the Manage Alert Subscriptions page.
![Publisher alerts settings](../../../assets/img/learn/alerts-settings-publisher.png)

3.  Click on the **Configuration** option that corresponds to the Abnormal Response Time option.
![Alerts configuration icon](../../../assets/img/learn/alerts-config-icon.png)

4.  Click **New Configuration** to add a new configuration
![Add new abnormal response time configuration](../../../assets/img/learn/alerts-abnormal-response-time-config-new.png)

5.  Select the API name and version for which you need to set up the alerts and define the time period (in milliseconds).
![Add new abnormal response time configuration](../../../assets/img/learn/alerts-abnormal-response-time-config.png)

6.  Click on **Add** to save the alert configuration <br />
    <html>
        <head />
        <body>
            <img src="../../../assets/img/learn/alerts-config-add.png" alt="Add alert configuration" title="Add alert configuration" width="60" />
        </body>
    </html>
    
Immediately after the response period of the API exceeds the above defined time period an alert gets triggered, such alerts could be treated as an indication of a slow WSO2 API Manager runtime or a slow backend.

#### Create an abnormal backend time alert

1.  Log into the API Publisher with the username and password of a user with required permission.
2.  Click on the **SETTINGS** menu, to open the Manage Alert Subscriptions page.
![Publisher alerts settings](../../../assets/img/learn/alerts-settings-publisher.png)

3.  Click on the **Configuration** option that corresponds to the Abnormal Backend Time option.
![Alerts configuration icon](../../../assets/img/learn/alerts-config-icon.png)

4.  Click **New Configuration** to add a new configuration
![Add new abnormal response time configuration](../../../assets/img/learn/alerts-abnormal-backend-time-config-new.png)

5.  Select the API name and version for which you need to set up the alerts and define the time period (in milliseconds).
![Add new abnormal response time configuration](../../../assets/img/learn/alerts-abnormal-backend-time-config.png)

6.  Click on **Add** to save the alert configuration <br />
    <html>
        <head />
        <body>
            <img src="../../../../assets/img/learn/alerts-config-add.png" alt="Add alert configuration" title="Add alert configuration" width="60" />
        </body>
    </html>
    
Immediately after the backend time of the API exceeds the above defined time period an alert gets triggered, such alerts could be treated as an indication of a slow backend. In technical terms, if the backend time of a particular API of a tenant lies outside the predefined value, an alert is sent.
### Configure alerts via the Developer Portal

Follow the instructions below to manage alert types via the Developer Portal:

- [Create an abnormal requests per minute alert](../../../../learn/analytics/managing-alerts-with-real-time-analytics/configuring-alerts/#create-an-abnormal-requests-per-minute-alert/)

!!! info
     You can not disable Abnormal Resource Access Alerts, Unseen Source IP Access Alerts and Tier Crossing Alerts, because they are enabled by default. However, you can enable and disable the email alerts that correspond to the latter mentioned alerts.
     
#### Create an abnormal requests per minute alert

1.  Log into the API Developer Portal with the username and password of a user with required permission.
2.  Click on the **SETTINGS** menu, to open the Manage Alert Subscriptions page.
![Publisher alerts settings](../../../assets/img/learn/alerts-settings-devportal.png)

3.  Click on the **Configuration** option that corresponds to the Abnormal Requests per Minute option. <br />
![Alerts configuration icon](../../../assets/img/learn/alerts-config-icon.png)

4.  Click **New Configuration** to add a new configuration
![Add new abnormal request count configuration](../../../assets/img/learn/alerts-abnormal-request-per-min-config-new.png)

5.  Select the API name and version for which you need to set up the alerts and define the request count per minute.
![Add new abnormal request count configuration](../../../assets/img/learn/alerts-abnormal-request-per-min-config.png)

6.  Click on **Add** to save the alert configuration <br />
    <html>
        <head />
        <body>
            <img src="../../../../assets/img/learn/alerts-config-add.png" alt="Add alert configuration" title="Add alert configuration" width="60" />
        </body>
    </html>

Immediately after the request count of the API exceeds the above defined request count per minute an alert gets triggered. These alerts could be treated as indications of possible high traffic, suspicious activity, possible malfunction of the client application etc.