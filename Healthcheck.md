
#### What is Health check 
App Service Health Check feature will help you to improve the availability of your Azure App Service.

#### Why to configure Health check on app services
Being PAAS offering and considering teh [architecture of app services](https://docs.microsoft.com/en-us/archive/msdn-magazine/2017/february/azure-inside-the-azure-app-service-architecture) you dont know if there is any issues which might cause application working unexpected or not working there it comes health check using which you can get real time availability of the application server if Health check is configured.

#### Pre-requisites:
Your App Service plan should be scaled to two or more instances to fully utilize Health check. THe reason we need to have 2 server is if one server by any means is not responding or having issues then azure infrastructure try to replace that server.

#### Overview
The Health Check feature allows you to specify a path on your application for App Service to ping. If an instance fails to respond to the ping, the system determines it is unhealthy and removes it from the load balancer rotation so that next requests would not go to that instance.
What if ping continues to fail:
When the instance is unhealthy and removed from the load balancer, the service continues to ping it. If it begins responding with successful response codes then the instance is returned to the load balancer. If it continues to respond unsuccessfully, App Service will restart the underlying VM in an effort to return the instance to a healthy state.
What if web app is auth is enabled:
Health Check integrates with App Service’s authentication and authorization features, so the system will reach the endpoint even if these security features are enabled. If you are using your own authentication system, the health check path must allow anonymous access.

Once you configure your app service health check, you can monitor the health of your app service using Azure Monitor. From the Health check blade in the Portal, click Metrics. This will open a new blade where you can see the app service health status history and if you want you can create a new alert rule for next action based on business impact of this application.

What if web app has traffic manager/application gateway/azure front door already configured
If these components are already configured then its recommended to revisit them and configured health check as it may cause redundant load if path is different and may misbehave.

#### Recommended Practices:
Health check path should check critical components of your application.
For example, if your application depends on a database and a messaging system.

#### How Azure platform improves availability of App Service:
The azure infrastructure used the health check feature configured path to ping and if an instance fails to respond to the ping the system determines its unhealthy ( non 200-299 response) and removes it from the azure infrastructure load balancer (Front end of app services) rotation.
If an instance remains unhealthy for one hour, it will be replaced with new instance by azure infrastructure.
Check the other apps in the plan with health check feature enabled - the health check is not marking an instance as unhealthy if returning non 2xx status code for one site inside the ASP, but for the others(with health check configured) it is healthy;

#### What is limit of workers/instance to be replaced:
WEBSITE_HEALTHCHECK_MAXUNHEALTYWORKERPERCENT variable is set to 50% by default which means that half of the total number of workers would be replaced - if you want to have more than 50% of the instances replaced then you should increase the variable value ( 0->100 - 0 means no instances replaced due to health check - 100 means all the instances could be replaced in case of being unhealthy)

*Good To know: *
Health check doesn't follow 302 redirects. At most one instance will be replaced per hour, with a maximum of three instances per day per App Service Plan.
Note, if your health check is giving the status Waiting for health check response then the check is likely failing due to an HTTP status code of 307, which can happen if you have HTTPS redirect enabled but have HTTPS Only disabled.
Health check path has a length limit of 64 characters: The portal has a requirement that the health check path be at most 64 characters.

#### Can health check configuration cause Restart of web app:
yes, Health check configuration changes cause restart your app. To minimize impact to production apps, we recommend configuring staging slots and swapping to production.

#### App settings related to Heath Check:
- WEBSITE_HEALTHCHECK_MAXPINGFAILURES
- WEBSITE_HEALTHCHECK_MAXUNHEALTHYWORKERPERCENT

#### Good to validate points:
Make sure you set up a default startup page correctly otherwise please use full path.
If you experience 404 which means either there is no default page or path you have configured is no more present. It could be possible that you’re using “/” and application don’t have any default page.
If its DotNet core application stack then Call the UseDefaultFiles method in Configure method of Startup.cs

##### References:
404 response code caused by App Services – AlwaysOn feature - Developer Support (microsoft.com)

Troubleshoot backend health issues in Azure Application Gateway | Microsoft Docs

#### How to automate Heath Check feature:
To enable the feature with ARM templates, set the healthcheckpath property of the Microsoft.Web/sites resource to the health check path on your site, for example: "/api/health/". To disable the feature, set the property back to the empty string, "".
$resourceGroupName = "resourcegroupname"
$appName = "webappname"
$webApp = Get-AzWebApp -ResourceGroupName $resourceGroupName -Name $appName -ErrorAction Stop
enable healthcheck feature. Update healthcheckpath
$webApp.SiteConfig.healthCheckPath = "/healthcheckpath"
Saving new configuration
$webApp = Set-AzWebApp -WebApp $webApp -ErrorAction Stop

Reference: https://docs.microsoft.com/en-us/rest/api/appservice/web-apps/create-or-update-configuration
Health Check is now Generally Available - Azure App Service

#### Elastic Function App & Health check
Please note that function app hosted inside Elastic Premium already has its own, built-in version of the HealthCheck functionality. We ping a health endpoint on all workers site deployed on elastic premium plan is running on every ~10 seconds or so. These health checks are factored into function scaling decisions, and function app do not route requests to unhealthy instances. 
Elastic Premium tier has some special scaling logic for HTTP traffic, and periodic health pings can cause unnecessary fluctuation. The built-in health checks for Elastic Premium should be sufficient and 
#### no need to configure the health check explicitly
