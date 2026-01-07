# Creating a Catalog and sharing it in a Developer Portal

_Hands-on lab guide_

${toc}

## Developer Portal Overview

The Developer Portal is a way for API providers to share their APIs with consumers and make it easy for them to not only learn about your API products, but also to subscribe to your API products, monitor their usage of APIs, and securely aquire API credentials.

With API Connect, there are three choices for how to share a catalog. Each of these options are selected per catalog and different catalogs can be shared in a different manner.

1. A Developer Portal site. Each one Developer Portal service can efficiently manage many Developer Portal sites to share many catalogs.
1. A Consumer Catalog. This is selected by default. A Consumer Catalog is a lightweight platform that provides the key functionality necessary to share a catalog:
    - Create accounts and applications
    - View available products
    - View analytics
    - Subscribe to API.
1. Don't share at all. An API Provider can manage consumer apps from within the API Manager UI.

For more information about the Developer Portal and the Consumer Catalog, see the [IBM Documentation](https://www.ibm.com/docs/en/api-connect/software/10.0.8_lts?topic=catalogs-consumer-catalog-developer-portal-considerations)

The most power option is the first one, the Developer Portal. In this lab we will create a new catalog and share it with a Developer Portal site. It assumes a Developer Portal server is deployed but may or may not be configured.

## Configuring a Developer Portal server

This chapter of the lab is to enable a new Developer Portal to work with an API Connect instance. This only needs to be done once per API Connect cloud, and some installation automations, including the Jam-in-a-box environment, may have already completed part of all of this configuration.

If there is already a working Developer Portal site on your server, you may skip this entire chapter and move on to [Creating a catalog](#creating-a-catalog).

### Register the Developer Portal service

Log in to the Platform Navigator using the username and password provided.

![Platform Navigator home page showing various integration capabilities including API Connect](images/PD01.png)

Click on the `apim-demo` Cloud Manager.

![Cloud Manager instance tile labeled apim-demo](images/PD02.png)

Log in. If using the Jam-in-a-box environment, clicking "Cloud Pak User Registry" should be enough as it already is configured to have an SSO with the Platform Navigator.

![Cloud Manager login screen with Cloud Pak User Registry option](images/PD03.png)

Click the `Configure topology` tile.

![Cloud Manager home page with Configure topology tile highlighted](images/PD04.png)

If the `Portal Service` is already available in your availability zone, you may skip the rest of this chapter and move on to [Configuring your SMTP service](#configuring-your-smtp-service).

![Topology configuration page showing availability zones and services](images/PD05.png)

Click `Register service`

![Register service button on topology page](images/PD06.png)

Select the `Portal` tile

![Service type selection dialog with Portal tile highlighted](images/PD07.png)

Enter the information about the portal server.

- The `Title` (usually `Portal Server` will suffice unless there are multiple Developer Portal services)
- The `Management endpoint` is the URL that the API Manager uses to communicate with the portal
- The `Portal endpoint` is the URL of the Developer Portal web sites. By default, a portal site will have a base URL of the form below, but this can be customized with vanity URLs per site.

Click save when done.

    ```text
    https://[portal-endpoint]/[porg-name]/[catalog-name]
    ```

![Portal service registration form with title, management endpoint, and portal endpoint fields](images/PD08.png)

### Configuring your SMTP service

The Developer Portal site needs an SMTP service to work. Navigate to the home page of the Cloud Manager and click either the `Manage resources` tile or the filing cabinet icon on the side nav.

![Cloud Manager home page with Manage resources tile highlighted](images/PD09.png)

Click `Notifications`

![Resources page with Notifications option in the sidebar](images/PD10.png)

If there is already a mail server available, you may skip to the next chapter, [Creating a Catalog](#creating-a-catalog). If not, click `Create`.

![Notifications page with Create button to add new mail server](images/PD11.png)

Enter the details of the email server in this screen, then click `Test email`

![Email server configuration form with fields for title, host, port, and credentials](images/PD12.png)

Enter your email address and clock `Send test email`. The Jam-in-a-box environment provides a simple email that catches all emails and allows you to view it in a web mail client.

![Test email dialog with email address field and Send test email button](images/PD13.png)

A successful email looks like the screenshot below

![Success notification showing test email was sent successfully](images/PD14.png)

Click save

![Save button on email server configuration page](images/PD15.png)

You now have everything you need to deploy a Developer Portal site

## Creating a Catalog

Log into Platform Navigator with the username and password provided

![Platform Navigator home page showing various integration capabilities including API Connect](images/PD01.png)

Click on the API Manager `apim-demo-mgmt` and, if necessary log in. If using the Jam-in-a-box environment, clicking "Cloud Pak User Registry" should be enough as it already is configured to have an SSO with the Platform Navigator.

![API Manager instance tile labeled apim-demo-mgmt](images/PD16.png)

Click on the `Manage catalogs` tile.

![API Manager home page with Manage catalogs tile highlighted](images/PD17.png)

There is already a Sandbox catalog. Every provider org has a Sandbox catalog by default. We're going to create a new catalog.

The `Add` menu gives you two options:

1. `Create catalog` creates a catalog and assigns it to a known user, and
1. `Invite catalog owner` creates a catalog for a new user and sends the user an email inviting them to create an account.

In this case, we'll `Create catalog` for ourselves.

![Add menu dropdown showing Create catalog and Invite catalog owner options](images/PD18.png)

Under `Select user`, select yourself. Then give your catalog a title.

Then click `Create`

![Create catalog form with user selection dropdown and catalog title field](images/PD19.png)

You'll see a `Catalog [your catalog] create` toast notification and a new tile with your catalog's name on it.

![Catalogs page showing Sandbox catalog and newly created catalog tile](images/PD20.png)

## Deloying a Developer Portal

Click into your new catalog

![New catalog tile with catalog name](images/PD21.png)

Click the `Catalog settings` tab. If your screen is too narrow, you may have to click the right chevron to reveal the Catalog settings tab.

![Catalog view with Catalog settings tab in the navigation](images/PD22.png)

Click `Portal`

![Catalog settings page with Portal option in the sidebar](images/PD23.png)

By default, your catalog has `Consumer catalog` is enabled, and the URL is provided on this screen. We want to use a Developer Portal, so let's click the `Create` button.

![Portal settings showing Consumer catalog URL and Create button for Developer Portal](images/PD24.png)

Select the portal service to deploy your portal site onto. Most API Connect installs, like the Jam-in-a-box environment, have only one portal service. Select it and click `Create`

![Create developer portal dialog with portal service selection dropdown](images/PD25.png)

It takes some time to provision the developer portal. You will receive an email once the provisioning is complete.

![Portal settings page showing provisioning status message](images/PD26.png)

When your portal is ready, you'll receive an email that looks like this. We need to set the admin password. To do that, click the link. It may take a minute for the link to respond.

![Email notification with portal ready message and activation link](images/PD27.png)

Click the blue `Sign in` button.

![Developer portal welcome page with Sign in button](images/PD28.png)

Create a strong admin password and click `Submit`

![Password creation form with password and confirm password fields](images/PD29.png)

You now have a Developer Portal site for your catalog and you are logged in as an admin. Click your avatar (the A for Admin) in the top right corner to pull down the user menu and click `Sign out`.

![Developer portal admin view with user menu dropdown showing Sign out option](images/PD30.png)

You are now ready to sign up as a consumer and browse your catalog in the Developer Portal.
