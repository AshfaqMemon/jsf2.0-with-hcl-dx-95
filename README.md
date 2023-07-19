# JSF 2.0 with HCL DX 9.5 on WAS 9.0.5.x
HCL DX 9.5 running on WAS 9.0.5.x contains JSF bridge compatible with MyFaces 2.2 because WAS 9.0.x contains MyFaces 2.2 as JSF runtime by default.

If you want to run JSF 2.0 portlet on HCL DX 9.5 running over WAS 9.0.5.x, you should set below context parameters in web.xml file of your portlet. This context parameters allows your application to run on MyFaces 2.2 runtime with JSF 2.0 specifications.

        <!-- To allow composite components inside sub directories, as 2.2 has new way for composite components -->
        <context-param>
            <param-name>org.apache.myfaces.STRICT_JSF_2_ALLOW_SLASH_LIBRARY_NAME
            </param-name><param-value>true</param-value>
        </context-param>

        <!-- Run in compatibility mode with 2.0.
        This will enable JSF 2.0 features like inheritance of parent template parameters to child template and many more-->
        <context-param>
            <param-name>org.apache.myfaces.STRICT_JSF_2_FACELETS_COMPATIBILITY</param-name>
            <param-value>true</param-value>
        </context-param>

        <!-- Disable caching of JSF.JS as it can cause issue with HCL DX JSF bridge and MyFaces 2.2. Should only be used when PROJECT_STAGE is set to Production -->
        <context-param>
            <param-name>org.apache.myfaces.RESOURCE_HANDLER_CACHE_ENABLED</param-name>
            <param-value>false</param-value>
        </context-param>

# Troubleshooting JSF application on HCL DX 9.5 with WAS 9.0.5.x

1. **JSF.JS is not loading correctly, the returned response contains HTML.**  
Ans. You should try to set context parameter **javax.faces.PROJECT_STAGE** to **Development** and see you are able to load JSF.JS file on the page. If that's the case, its issue related to JSF.JS caching when PROJECT_STAGE is set to **Production**.

2. **My page is not rendering completely.  In the logs getting Serialization issue.**  
Ans. If you are using Session beans, Request beans make sure that they are marked with **Serializable** interface and are Serializable. In older version of Portal, JSF bridge hanles serialization issue but new JSF bridge is more conformant to JSF specification and hence it requires beans to be Serializable.

3. **My JSF 2.0 application is not working properly, facing many issues.**  
Ans. From my experience I recommend to use HCL DX 9.5 with at least CF206 or newer which contains most stable version of JSF bridge. Also, there are few fixes coming from IBM team, which will be incorporated in WAS 9.0.5.17. So, if you are on any fix pack less than 206, please consider for upgradation.

4. Is there any migration guide available for migrating JSF 2.0 running on portal 8.0 to 9.5?  
Ans. No, there is no migration guide. If you are already using JSF 2.0 specification, then there are not much changes required. Some changes that I know is listed below -
    - Compilation with new WAS libraries
    - Changes of Portlet classes listed in the [URL](https://opensource.hcltechsw.com/digital-experience/CF213/extend_dx/portlets_development/usage/jsf/)
    - Updating libraries to be compatible and supported with Java 8
    - Migrating to new View attribute parameter to **com.ibm.faces.portlet.VIEWID** from **com.ibm.faces.portlet.page.view**
    - Theme update to utilize new modules available in portal 9.5