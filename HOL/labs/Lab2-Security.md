# Lab: Security

In this lab you will secure access to the management site, and optionally
enable users to log in to the main site and mobile API. (The main site and mobile
API can be used without authentication, so it is only useful if you want to store
persistent information for the user. The [SQL Server](Lab5-SqlServer.md "SQL Server lab") lab later on requires
authentication on the main site.)

In both cases, you will be using Azure Active Directory (Azure AD for short), but in
slightly different ways. The management site will use an ordinary Azure AD directory,
and will only allow in users that a directory administrator has added to that directory.
The main site and mobile API will use an Azure AD B2C (Business to Consumer)
directory, which enables users to authenticate through various popular social media
sites, or with a Microsoft Account. With a B2C directory, users do not need to be
added in advance - you can define a policy that enables, say, any user who can
authenticate with a Microsoft Account to join.

## Part 1: Create Azure AD Directory for Management Site

First, you need to create a new directory in Azure AD. (The nomenclature gets a little
unwieldy: you're about to create a new Azure Active Directory directory - so good, they
named it twice. You can shorten this to 'Azure AD directory' but it's often abbreviated
further to just 'AD'. That doesn't help much because now 'AD' means two different
things, which is why some people prefer to use the other name for an Azure AD directory:
**tenant**. Azure AD is a multi-tenant service, meaning it can support multiple
customers, and each directory is a tenant of the service.)

A tenant (or directory) defines a set of users, and is able to authenticate them.
(Azure AD offers a few different mechanisms for this. It defaults to username/password,
but it also supports two-factor authentication, or it can use smartcard-based login,
or, in it can defer to external authorities such as the Microsoft Account
infrastructure.) You can also optionally define groups of users. And a tenant also
defines one or more **applications** which are allowed to use the tenant to log users
in. (If you want your web site to be able to authenticate users in a particular
tenant, you must define a corresponding application in the directory that lets
Azure AD know that the web site is authorized to do this. Likewise, if you want
to enable mobile or desktop clients to log users in through Azure AD, you need to
define an application in the tenant that permits this.)

You will already have at least one directory: any Azure subscription is associated
with one. If you signed up for your Azure subscription using a Microsoft account,
the directory will typically have a name based on your email address. For example,
if you are `jdoe@example.com` the tenant will be called something like
`jdoeexample.onmicrosoft.com`. If you use one of the business editions of Office 365
you will belong to a directory associated with your company's Office 365 subscription.
(It's possible that your Azure subscription is associated with the same tenant as your
company's Office 365 subscription—companies that supply multiple employees with
MSDN subscriptions often control access to these through an Office 365 tenant, in which
case the Azure subscription included with each MSDN subscription will typically be
associated with that same tenant.)

In general, you don't want to use the same directory for your Azure subscription and
applications that you host in Azure. You might not even have a choice. If the tenant
in question is your company's Office 365 tenant, you might not have the necessary
security access to define applications. In any case, the set of users who should
have access to your Azure subscription is very often going to be quite different from
the set of users who will be using some application you're hosting in Azure, and it
can simplify matters to define a tenant specifically for your application. Thus that's
what you will be doing. By creating a dedicated tenant just for your management site,
you can use a very simple policy: users can use the management site if and only if they
belong to its tenant.


1.  Find Azure Active Directory

    ![Active Directory icon and label](media/ad1.png)

2.  Select it 
    ![Active Directory icon and label](media/ad2.png)

3.  Type an organizacion name and AD domain name

    ![Active Directory icon and label](media/ad3.png)

4.  On the users blade you can see the users and add new ones if you want.
    ![Active Directory icon and label](media/ad4.png)

> **Note:** There are now **two** ways to proceed. In either case, you will create an
Application in your Azure AD tenant, and configure your Azure Kit management web
site to use it. You can either get Visual Studio to do the work for you,
or you can do it by hand. It's easiest to let Visual Studio do it, and if that's
what you want, proceed to **Part 2**. However, that will hide some important details, and
if you'd like to see exactly what's going on, skip *Part 2*, and go straight to **Part 3**.

## Part 2: Configure the Management Web App

Now that you have a tenant, you can configure the management app to use it. In fact
Visual Studio can do most of the work - it can create the application in the tenant
for you.

1.  Assuming you have Visual Studio 2015 still open from the last lab, return to Visual Studio, 
    and right click on the **AzureKit.Management** project and select
    **Publish**. Use the **Prev** or **Next** buttons to move to the **Settings** page.
    Last time you published, you unchecked the **Enable Organizational Authentication**
    checkbox. Now you need to check it:

    ![Publish settings](media/PublishSettingsEnableOrgAuth.png)

    In the **Domain** text field, type in the domain name you used earlier when
    creating the tenant, and then add `.onmicrosoft.com` to the end.

2.  Click **Publish**. Visual Studio will ask you to sign in:

    ![Sign in](media/SignInDialog.png)

    It needs you to log in as a Global Administrator of the tenant. There will be just
    one such adminstrator (unless you added more), and that will be whatever user
    account you logged into the Azure management site with. Log in using that same
    account.

3.  Once Visual Studio has finished deploying your app, it will open it in a web
    browser. Click the **Sign in** link. Log in if prompted. The first time you use
    the site you will see a consent prompt:

    ![Login consent prompt](media/ManagementAppLogin.png)

    Click **Accept**

    The main page of the Azure Kit management site will show, and this time it won't
    ask you to log in - it will show a series of links for managing various aspects
    of the site.

    **Note:** none of these will work yet! You have not yet created a DocumentDB
    instance to hold the content, so the management site will have nowhere to put
    it. You will need to complete the next lab before you will be able to use this.

Since *Part 3* is an alternative to *Part 2*, you can now skip directly to **Part 4**.



## Part 3: Create Azure AD B2C Directory for the Public Site and Mobile API

The directory you created in the preceding parts secures access to the 
management site. If a new user is to gain access to the site, someone with
administrator privileges for the tenant must add them. This is exactly what you want
for the management site - it shouldn't be open to anyone who discovers the URL. But
this is not a suitable approach for the main site.

The main site is designed to be open to all. In fact, it doesn't need authentication, 
it can be used entirely anonymously. However, in one of the later labs 
(the [SQL Server](Lab5-SqlServer.md "SQL Server lab") lab), you will add a feature 
in which the user can opt-in to (and back out of) receiving email notifications 
related to site updates. For that to work, you need some way of authenticating users.
You can't just let someone type in any old email address and trust that it's valid. 
So although you won't force all users to log in, you will need them to authenticate 
if they want to receive emails, and manage the settings for that feature.

Fortunately, Azure AD can still help you. You can create a different kind of tenant: a
*B2C* (Business to Consumer) tenant. You may recall there was a checkbox for this when
you created the tenant earlier. If you select this option, the tenant behaves quite
differently. Instead of allowing in only those users explicitly added by a tenant
administrator, you can configure a B2C tenant to allow in any user at all. AAD still
requires them to authenticate, but there are a few ways it can do this. You can configure
it to allow anyone with a Microsoft Account (formerly called a Live ID) to log in.
You can also enable log in through Google, Facebook, LinkedIn, or Amazon accounts.
Furthermore, Azure AD can authenticate users directly through their email address, 
if you enable this. Users will be able to enter an email address, and it will send
an email containing a verification link to allow them to prove that they own the
email address.

1.  Go to portal click create and find azure active directory b2c

    ![Active Directory icon and label](media/adbc1.png)

2.  Click the **+ Create Azure AD B2C Tenant**:

    ![New button](media/adbc2.png)

3.  Oncre created, select it.

    ![Directory creation](media/adbc3.png)

4.  Go to the old management site http://management.windowsazure.com, find your AD b2c tenant and get the ID from the     URL, copy it somewhere, you need it a few times later, this is referred as the Tenant ID.
    ![Custom Create](media/adbc5.png)

5.  At the top right of the portal is a button showing your user name and the name
    of the tenant associated with your subscription. If you click this, it shows a
    list of tenants that you belong to:

    ![Tenant list](media/AzurePortalDomainPicker.png)

    This will include the new B2C tenant that you just created. Select that.

7.  Once you have selected the B2C tenant, you will see a fresh dashboard - none of
    the resources you created earlier will be visible. This is because you can only
    access resources in Azure subscriptions associated with the selected tenant, and
    there are no Azure Subscriptions associated with this new tenant. (Each Azure
    subscription is controlled by exactly one Azure AD tenant, and creating a new
    tenant doesn't change that. You can move subscriptions between tenants if you
    want to, but it doesn't happen automatically. So a freshly-created tenant will
    never have any associated Azure subscriptions to begin with.) This is why we
    recommended that you use a new browser tab for this work - by switching tenants,
    you cause all your Azure resources to become inaccessible. (Obviously you can
    switch back, but then you won't be able to configure your new B2C tenant.)
    
    On the left hand side, click the **More services** label:

    ![More services](media/AzurePortalMoreServices.png)

    Find your AD B2C directory, and create and application for the manaagement site.

    ![Custom Create](media/adbc4.png)  


8. You must first tell Azure AD which identity providers to use, and supply it with
    the details it requires to work with them. In the **Settings** column, click the
    **Identity providers** entry. It will initially show that no social identity
    providers have been configured. Click the **+ Add** button at the top of the blade.

    In the **Add social identity provider** blade that opens, enter *MSA* in the
    **Name** field, then click on **Identity provider type**. In the list that
    appears, select **Microsoft Account**.

    ![Select Microsoft Account](media/B2cConfigureMsa.png)

    Click **OK**.

11. Back in the **Add social identity provider** blade, click **Set up this
    identity provider**. This will show a blade asking for the **Client ID** and
    **Client Secret**. These are values that you need to obtain from the Microsoft
    Account system, so you will need to go to
    [http://go.microsoft.com/fwlink/p/?LinkId=262039](http://go.microsoft.com/fwlink/p/?LinkId=262039)
    and log in with a *Microsoft Account*.

12. In the **My applications** page, find the **Live SDK applications** section, and
    click its **Add an app** button. (There will be more than one such button—make
    sure to click the one in the right section.) Enter a name for the application:

    ![New application name](media/MsaNewApplication.png)

    Click **Create application**


13. In the page that opens representing the new application, you will see some details:

    ![Application details](media/MsaAppDetails.png)

    This contains the two values you need. Copy the **Application Id** in the
    **Properties** section and past it into the **Client ID** field in the browser
    tab in which you're setting  your B2C tenant. Then in the **Application Secrets**
    section, copy the string of random looking characters in that section and
    paste it in as the **Client secret** for the B2C settings.

    You can click **OK** in the B2C settings now, and then click **Create**.

    You're not yet done with the Microsoft Account settings though. You need to give
    it the URL that Azure AD will supply as a redirect URL during log in. This URL
    will have the following form:

    `https://login.microsoftonline.com/te/YOUR-TENANT-ID/oauth2/authresp`

    You will need to replace `YOUR-TENANT-ID` with your tenant's unique GUID.
    To discover your B2C tenant's ID, you'll need to go back to the Azure management
    site. If you're not already there, go to the page for your B2C tenant, and then
    look in the address bar - it will have a URL with a `#` followed by the text
    `Workspaces/ActiveDirectoryExtension/Directory` followed by a GUID. That GUID is
    your tenant id.

    Enter this in the **Redirect URIs** section of the **Web** block under
    **Platforms** in the Microsoft Account application page:

    ![MSA web platform](media/MsaAppPlatform.png)

    Click **Add Url**. Then at the bottom of the page click **Save**

    You have now completed the Microsoft Account configuration.
    ![Custom Create](media/adbc6.png)
    ![Custom Create](media/adbc7.png)

14. Back in the Azure portal, you will also need to configure a *Sign In/Sign Up*
    policy for your B2C tenant, to tell it who should be allowed to log into the
    site. Close the **Identity provider** blade if it is still open, then in the
    **Settings** blade, under **POLICIES** select **Sign-up or sign-in policies**

    ![Sign-up or sign-in](media/B2cSignUpOrIn.png)

    In the **Sign-up or sign-in policies** blade that opens click the **+ Add** button.

    ![Add](media/AzurePortalAddButton.png)

15. In the **Add policy** dialog set the name to **SignUpOrIn**. Click on **Identity
    providers** and then check the **MSA** checkbox.

    ![Id provider list for policy](media/B2cPolicyIdProviders.png)

    Click **OK**.

    Select **Sign-up attributes**. In the **Select sign-up attributes** blade,
    check **Display Name** and **Email Address**
    
    ![Policy sign-up attributes](media/B2cPolicySignUpAttributes.png)

    Click **OK**.

    Select **Application claims**, and again check **Display Name** and **Email Address**
    then click **OK**.

    Click **Create** in the **Add policy** blade.

## Part 5: Configure the Main Site to use your B2C Tenant

First, you will need to create an application, remember Azure AD will only allow a web 
site to authenticate users through a tenant if you define a corresponding application.

1.  Still in the Azure portal on the B2C settings, on the **Settings** blade, 
    select **Applications**. In the blade that opens, click **+ Add**.

    In the **New application** blade, set the **Name** to **Azure Kit Web**. Under
    **Include web app/web API** click **Yes**. Leave **Allow implicit flow** at **Yes**
    then in the **Reply URL** enter the URL for your Azure Kit main site, specifying `https`.

    ![](media/B2cAppSettings.png)

    Click **Create**

    After a few seconds, the application will appear in the **Applications** blade. Note
    down the **APPLICATION ID** because you will need this.

2.  In Visual Studio, open the **AzureKit** project's **Web.config** file, and find the
    `appSettings` section. Set the **ClientId** to the ID of the application you just
    created in the Azure portal. Set the **TenantId** to the ID for your B2C tenant that you discovered
    earlier when setting up the Microsoft Account reply URL.

    For the **B2cSignInOrUpPolicy** you need the name of the policy you created, but
    it won't have the exact name you chose when you created it. In the **Settings**
    blade for your B2C tenant, select **Sign-up or sign-in policies**. The list
    may take a few seconds to populate, but when it does it will show the name Azure AD
    has chosen for the policy. It will be something like **B2C_1_SignUpOrSignIn**.
    Whatever value is shown, use that as the **B2cSignInOrUpPolicy** in your `Web.config`.

3.  Publish the main site again. (Right click the **AzureKit** project in Solution Explorer,
    select **Publish** then click **Publish**.)

4.  When the web site opens in a browser, click the **Sign in** link at the top right. You
    might find that it goes straight to a page in which it shows your email address and a
    default display name, and offers a **Continue** button. If you see this, what has happened
    is that Azure AD remembers your identity and has not asked you to sign in again. In this case,
    to see the whole login flow, try opening a browser in a 'private' mode. (E.g., if you're using
    Chrome, select **New incognito window** from its menu at the top right; if using Edge,
    select **New InPrivate Window** from its menu at the top right.) You should see a normal
    Microsoft Account prompt:

    ![Microsoft Account Login](media/MsaLoginPrompt.png)

    Log in with your Microsoft Account, and once you have, you will then be redirected back
    to the Azure Kit site as an authenticated user.

## Part 6: Configure the Mobile API to use your B2C Tenant

You will need to create another application for the mobile API because in B2C tenants, 
applications can only redirect to one domain.
 
1.  In the **Settings** blade select **Applications**. In the blade that opens, click **+ Add**.

    In the **New application** blade, set the **Name** to **Azure Kit API**. Under
    **Include web app/web API** click **Yes**. Leave **Allow implicit flow** at **Yes**.

    For the **Reply URL**, start with the URL for your Azure Kit API site, specify `https`,
    and then on the end, add `.auth/login/aad/callback`.

    Click **Create**

    After a few seconds, the application will appear in the **Applications** blade. Note
    down the **APPLICATION ID** because you will need this.

2.  While the main site happens to use the ASP.NET's support for working with AAD, the mobile
    site is using the Azure Mobile Services server SDK, and when using that, you don't normally
    write our own code for authentication. Instead, we're going to configure the Azure App Service
    to authenticate for us.

    Back in the browser tab that has the Azure portal open showing with the tenant for your
    subscription selected (i.e., the one in which you can see all your web apps, not the
    one you're using to work with the B2C tenant), go to your resource group. (You can click
    on the **Microsoft Azure** text at the top left, and then click on the tile on your
    dashboard that represents your resource group.) Click on your **API site**.

    In the **Settings** blade, select **Authentication/Authorization**. Set
    **App Service Authentication** to **On**.

    Leave the **Action to take when request is not authenticated** as the default:
    **Allow Anonymous requests (take no action)**

    In the list of **Authentication Providers** select **Azure Active Directory**

    In the **Azure Active Directory Settings** blade, click **Advanced**.

    In **Client ID** enter the ID of the application you created in the B2C tenant for your
    mobile API.

    For the **Issuer Url** you need a URL of this form:

    `https://login.microsoftonline.com/YOUR-TENANT-ID/v2.0/.well-known/openid-configuration?p=B2C_1_SignUpOrSignIn`

    You will need to substitute your tenant's ID for `YOUR-TENANT-ID`. You should also check
    whether the profile name at the end of the URL shown above (`B2C_1_SignUpOrSignIn`) matches
    the name of the profile you created. If not, use the right one for your profile.

    In the end it should look something like this:

    ![](media/ApiAppAuthConfig.png)

    Click **OK**.

    Click the **Save** button at the top of the blade.

3.  To test this, in Visual Studio, open the **AzureKit - Mobile UWP Only** solution. Right click
    on the **AZKitMobile.UWP** project and select **Set as StartuUp Project**. Select the
    **Debug | Start Debugging** menu item to run the app.

    It will show a **Push registration** error which you can dismiss because you haven't
    yet told it which server to use. You should see the **Settings** page. (If you've already
    run this app, you might not, in which case click the cog icon to show the settings.)

    Under **Web/Mobile Address** there is a textbox enter the URL of your API web site,
    specifying `https`. Click the save icon at the top right. A progress UI will appear, and
    then you will see an error reporting an **Internal Server Error**. This is because you
    haven't yet set up DocumentDB on the server, so you can ignore and dismiss this error.
    (Our goal at this stage is simply to verify that login works.)

    Click on the padlock icon at the top right. You should see a window labelled
    **Connecting to a service** appear. You may see a progress animation, but then you
    should see a Microsoft Account login prompt.

    ![](media/UwpMsaLogin.png)

    Log in with your Microsoft Account. If this completes without error, you have successfully
    configured authentication. (Since there is more work to do on the back end, there's nothing
    to see at this point in the app - you just want to verify that you can complete the login.)
    You can close the UWP app.
