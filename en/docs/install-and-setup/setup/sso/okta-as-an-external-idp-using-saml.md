# Using Okta as an External IDP with SAML

Follow the instructions below to connect Okta as a third party Identity Provider to WSO2 API Manager.

## Pre-requisites

Before you begin, make sure you do the following.

1. Create an account in [https://developer.okta.com/](https://developer.okta.com/)
2. Download the latest API Manager distribution from [https://wso2.com/api-management/](https://wso2.com/api-management/)
3. Unzip the distribution and open the `deployment.toml` file located in `<APIM_HOME>/repository/conf/`. Add the following configuration
    ```
    [tenant_mgt]
    enable_email_domain= true
    ```
    You need to enable this because Okta uses the email as the username by default. To  use the email as the username in WSO2 API Manager you have to enable it as it is not enabled by default. Once enabled, you can use your email or a normal username as your username.
4. Start the WSO2 API Manager server.

## Step 1 - Configure Okta

1.  Login to the Okta developer console and switch to the classic UI. 
    [![]({{base_path}}/assets/img/learn/okta-classic-ui.png)]({{base_path}}/assets/img/learn/okta-classic-ui.png)

2.  Go to **Applications** -> **Add Application**.
    [![Add new Okta SAML application]({{base_path}}/assets/img/learn/okta-saml-add-app.png)]({{base_path}}/assets/img/learn/okta-saml-add-app.png)

3.  Click **Create New Application**.
    [![Create new Okta SAML application]({{base_path}}/assets/img/learn/okta-saml-create-new-app.png)]({{base_path}}/assets/img/learn/okta-saml-create-new-app.png)

4.  Select **Web** as the **Platform**. Select **SAML 2.0** as the sign-on method.
    [![Create a new application integration]({{base_path}}/assets/img/learn/okta-saml-create-saml-app.png)]({{base_path}}/assets/img/learn/okta-saml-create-saml-app.png)

5.  Enter the **General Settings** as shown in the images below.
    [![Enter application name]({{base_path}}/assets/img/learn/okta-saml-create-saml-app-name.png)]({{base_path}}/assets/img/learn/okta-saml-create-saml-app-name.png)
    
    [![Enter application details]({{base_path}}/assets/img/learn/okta-saml-create-saml-app-details.png)]({{base_path}}/assets/img/learn/okta-saml-create-saml-app-details.png)

    !!!warning
        **Audience URI** should be same as the identity provider entity id name that is created in WSO2 API Manager

6.  Inside the SAML app you created go to **Sign On** and click **View Setup Instructions**.

    [![View Setup Instructions]({{base_path}}/assets/img/learn/okta-saml-create-new-app-config1.png)]({{base_path}}/assets/img/learn/okta-saml-create-new-app-config1.png)

    1.  Scroll up to the **Provide the following IDP metadata to your SP provider** section. Copy and save the details given to a xml file.

        [![Copy and save xml]({{base_path}}/assets/img/learn/okta-saml-create-new-app-config2.png)]({{base_path}}/assets/img/learn/okta-saml-create-new-app-config2.png)

    2.  Go to **Assignments** -> **Assign**. Click **Assign to People** and assign your current user.

        [![Assign your current user]({{base_path}}/assets/img/learn/okta-saml-create-new-app-assign.png)]({{base_path}}/assets/img/learn/okta-saml-create-new-app-assign.png)
    
7.  Switch back to the Developer Console shown in step 1.

8.  Follow these steps to add a new attribute to the default user profile of Okta to represent the user role. 

    1.  Navigate to **Users** -> **Profile Editor** and click the pencil icon to edit the default profile.

        [![Edit the default profile in the Profile Editor]({{base_path}}/assets/img/learn/okta-add-new-attribute.png)]({{base_path}}/assets/img/learn/okta-add-new-attribute.png)

    2.  Click **Add Attribute** to add new user attributes.
    
        [![Add new attribute]({{base_path}}/assets/img/learn/okta-add-new-attribute-add.png)]({{base_path}}/assets/img/learn/okta-add-new-attribute-add.png) 

9.  Enter the user attributes shown in the image below. Click **Save**.

    [![Add new attributes]({{base_path}}/assets/img/learn/okta-add-new-attribute-details.png)]({{base_path}}/assets/img/learn/okta-add-new-attribute-details.png) 

10.   Follow the steps below to edit the user profile.

    1.  Go to **Users** -> **People** and click on your profile name.
        
        <a href="{{base_path}}/assets/img/learn/okta-profile-edit.png"><img src="{{base_path}}/assets/img/learn/okta-profile-edit.png"/></a>
    
    2.  Click **Edit** to change the profile details.
        
        <a href="{{base_path}}/assets/img/learn/okta-profile-edit2.png"><img src="{{base_path}}/assets/img/learn/okta-profile-edit2.png" width="600" height="400"/></a>

    3.  Add the **Role**. This will be used in the API Manager to map an internal role to the provisioned user.
        
        <a href="{{base_path}}/assets/img/learn/okta-profile-edit3.png"><img src="{{base_path}}/assets/img/learn/okta-profile-edit3.png"/></a>

## Step 2 - Configure API Manager
1.  Login in to `https://localhost:9443/carbon`.

2.  Create a role that needs to be assigned to users that will be provisioned from Okta. 

    1.  Go to **Users and Roles**.
        <a href="{{base_path}}/assets/img/learn/okta-apim-add-role.png"><img src="{{base_path}}/assets/img/learn/okta-apim-add-role.png" width="400" height="200"/></a>

    2.  Add a new role.
        <a href="{{base_path}}/assets/img/learn/okta-apim-add-role-name.png"><img src="{{base_path}}/assets/img/learn/okta-apim-add-role-name.png" width="400" height="200"/></a>

    3.  Assign the following permissions to the role and click **Save**.

    <a href="{{base_path}}/assets/img/learn/okta-apim-add-role-permissions3.png"><img src="{{base_path}}/assets/img/learn/okta-apim-add-role-permissions3.png" width="300" height="300"/></a>
    <br/>
    <br/>
    <a href="{{base_path}}/assets/img/learn/okta-apim-add-role-permissions2.png"><img src="{{base_path}}/assets/img/learn/okta-apim-add-role-permissions2.png" width="300" height="350"/></a>
    <br/>
    <br/>
    <a href="{{base_path}}/assets/img/learn/okta-apim-add-role-permissions1.png"><img src="{{base_path}}/assets/img/learn/okta-apim-add-role-permissions1.png" width="300" height="300"/></a>

3. Add role permissions via the WSO2 API Manager Admin Portal.

    1. Sign in to the WSO2 API Manager Admin Portal.
    
         `https://localhost:9443/admin`
         
    2. Click **Settings** and then click **Role Permissions**.

         [![Okta API-M role pemission mapping]({{base_path}}/assets/img/learn/okta-apim-role-pemission-mapping-admin-ui.png)]({{base_path}}/assets/img/learn/okta-apim-role-pemission-mapping-admin-ui.png) 

    3. Click **Add role permission**.
    
    4. Enter `okta_role` in the **Provide role name** field and click **Next**.

         [![Edit Okta API-M role pemission mapping]({{base_path}}/assets/img/learn/okta-apim-role-pemission-mapping-admin-ui-edit1.png)]({{base_path}}/assets/img/learn/okta-apim-role-pemission-mapping-admin-ui-edit1.png) 

    5. Go to **Select permissions**, click  **Custom permissions**, and start assigning the permissions as shown below. 
    
         These permissions will allow a user having the `okta_role` to login to Publisher and Developer Portals.

        [![Okta API-M role pemission mapping]({{base_path}}/assets/img/learn/okta-apim-role-pemission-mapping-admin-ui-edit2.png)]({{base_path}}/assets/img/learn/okta-apim-role-pemission-mapping-admin-ui-edit2.png)

        [![Okta API-M role pemission mapping]({{base_path}}/assets/img/learn/okta-apim-role-pemission-mapping-admin-ui-edit3.png)]({{base_path}}/assets/img/learn/okta-apim-role-pemission-mapping-admin-ui-edit3.png)

        [![Okta API-M role pemission mapping]({{base_path}}/assets/img/learn/okta-apim-role-pemission-mapping-admin-ui-edit4.png)]({{base_path}}/assets/img/learn/okta-apim-role-pemission-mapping-admin-ui-edit4.png)

        [![Okta API-M role pemission mapping]({{base_path}}/assets/img/learn/okta-apim-role-pemission-mapping-admin-ui-edit5.png)]({{base_path}}/assets/img/learn/okta-apim-role-pemission-mapping-admin-ui-edit5.png)

        [![Okta API-M role pemission mapping]({{base_path}}/assets/img/learn/okta-apim-role-pemission-mapping-admin-ui-edit6.png)]({{base_path}}/assets/img/learn/okta-apim-role-pemission-mapping-admin-ui-edit6.png)

        [![Okta API-M role pemission mapping]({{base_path}}/assets/img/learn/okta-apim-role-pemission-mapping-admin-ui-edit7.png)]({{base_path}}/assets/img/learn/okta-apim-role-pemission-mapping-admin-ui-edit7.png)

    6. Click **Save** to save your changes.

    !!! note
        If you want your user to perform analytics-based tasks, you should add the `okta_role` to the required analytics scopes according to your preference. The steps below are given as an example.

        1. Sign in to the API-M Management Console.
             `https://localhost:9443/carbon`

        2. Navigate to **Main > Resources > Browse**.

        3. Enter `/_system/config/apimgt/applicationdata/tenant-conf.json` as the location and click **Go** to browse the registry and locate the required resource.

        4. Update the `RESTAPIScopes` JSON field by adding `okta_role` to the `Roles` field under the corresponding `Name` fields as shown below for the analytics related scopes.
            ```bash
            {
                "Name": "apim_analytics:api_analytics:view",
                "Roles": "admin,Internal/creator,Internal/publisher,okta_role"
            },
            {
                "Name": "apim_analytics:application_analytics:view",
                "Roles": "admin,Internal/subscriber,okta_role"
            },
            ```
        5. Click **Save Content**.

4. Add an Identity Provider.

     1. Sign in to the WSO2 API-M Management Console.
     
         `https://localhost:9443/carbon`. 
     
     2. Click **Main** and then click **Add** under  **Identity Providers**. 
     
     3. Enter the Identity Provider's Name.  

         [![Add an IDP for Okta SAML]({{base_path}}/assets/img/learn/okta-saml-add-idp.png)]({{base_path}}/assets/img/learn/okta-saml-add-idp.png) 

     4. Expand **Federated authenticators** -> **OAuth2/OpenID Connect Configuration** and add the following details.
        
        [![API-M IDP OIDC details]({{base_path}}/assets/img/learn/okta-apim-idp-odic-details.png)]({{base_path}}/assets/img/learn/okta-apim-idp-odic-details.png)

        <table>
        <colgroup>
            <col />
            <col />
            <col />
        </colgroup>
        <tbody>
            <tr>
                <th colspan="2"><b>Field</b></th>
                <th><b>Sample value</b></th>
            </tr>
            <tr>
                <td colspan="2" class="confluenceTd">Enable OAuth2/OpenIDConnect</td>
                <td class="confluenceTd">True</td>
            </tr>
            <tr>
                <td colspan="2" class="confluenceTd">Client id</td>
                <td class="confluenceTd">You can find this value from the Okta application that you created.</td>
            </tr>
            <tr>
                <td colspan="2" class="confluenceTd">Client secret</td>
                <td class="confluenceTd">You can find this value from the Okta application that you created.</td>
            </tr>
            <tr>
                <td colspan="2" class="confluenceTd">Authorization Endpoint URL</td>
                <td class="confluenceTd">https://your_okta_url/oauth2/default/v1/authorize</td>
            </tr>
            <tr>
                <td colspan="2" class="confluenceTd">Token Endpoint URL</td>
                <td colspan="1" class="confluenceTd">https://your_okta_url/oauth2/default/v1/token</td>
            </tr>
            <tr>
                <td colspan="2" class="confluenceTd">callback url</td>
                <td class="confluenceTd">
                    https://localhost:9443/commonauth
                </td>
            </tr>
            <tr>
                <td colspan="2" class="confluenceTd">Userinfo Endpoint URL</td>
                <td colspan="1" class="confluenceTd">
                    https://your_okta_url/oauth2/default/v1/userinfo
                </td>
            </tr>
            <tr>
                <td colspan="2" class="confluenceTd">Logout Endpoint URL</td>
                <td colspan="1" class="confluenceTd">
                    https://your_okta_url/oauth2/default/v1/logout
                </td>
            </tr>
            <tr>
                <td colspan="2" class="confluenceTd">Additional Query Parameters</td>
                <td colspan="1" class="confluenceTd">
                    scope=openid%20profile
                </td>
            </tr>
        </tbody>
        </table>

     5. Expand **Claim Configuration** -> **Basic Claim Configuration**. Add the claim configurations as shown in the image below.
         
         [![Okta API-M IDP claims details]({{base_path}}/assets/img/learn/okta-apim-idp-claims-details.png)]({{base_path}}/assets/img/learn/okta-apim-idp-claims-details.png) 

     6. Expand **Role configuration** and add `okta_role` as shown below. 
     
         You can check if the user logged in has the role `any` and assign the local `okta_role`.

        <a href="{{base_path}}/assets/img/learn/okta-apim-role-oidc-role-mapping.png"><img src="{{base_path}}/assets/img/learn/okta-apim-role-oidc-role-mapping.png"/></a>

     7. Enable **Just-in-Time Provisioning** for the user to be saved in the API Manager user store.

         <a href="{{base_path}}/assets/img/learn/okta-apim-role-oidc-jit.png"><img src="{{base_path}}/assets/img/learn/okta-apim-role-oidc-jit.png" width="600"/></a>

    !!! info
        When Just-In-Time Provisioning is enabled, the user details will be saved in the API Manager user store. User profile details will be updated via the federation following each login event. To preserve the user profile details without any changes you need to enable `SystemRolesRetainedProvisionHandler`.
        
        Add the following to the `<API-M_HOME>/repository/conf/deployment.toml` file and restart the server.

        ```
        [authentication.framework.extensions]
        provisioning_handler = "org.wso2.carbon.identity.application.authentication.framework.handler.provisioning.impl.SystemRolesRetainedProvisionHandler"
        ```

8. Update the Service Providers.

    1. Click **Service Providers** -> **List** in the WSO2 API-M Management Console.
        
        There are two service providers available by default; `apim_publisher` and `apim_devportal`. 
        
    2. Click **Edit** to edit `apim_publisher`.

        !!! warning
            You need to have signed in to the Developer Portal and Publisher at least once for the two service providers to appear, as it is created during the first sign in.

        [![Okta API-M role OIDC SP]({{base_path}}/assets/img/learn/okta-apim-role-oidc-sp.png)]({{base_path}}/assets/img/learn/okta-apim-role-oidc-sp.png)

    3. Expand **Local & Outbound Authentication Configuration** under **Federated Authentication** and select the identity provider you created.

         [![Okta API-M role OIDC SP outbound]({{base_path}}/assets/img/learn/okta-apim-role-oidc-sp-outbound.png)]({{base_path}}/assets/img/learn/okta-apim-role-oidc-sp-outbound.png)
    
    4. Repeat the latter mentioned two steps for `apim_devportal`.

Now you will be able to Sign in to the Publisher and Developer Portal using Okta.
