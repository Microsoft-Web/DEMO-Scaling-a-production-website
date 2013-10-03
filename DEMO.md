<a name="title" />
# Scaling a production web site #

---
<a name="Overview" />
## Overview ##


<a id="goals" />
### Goals ###
In this demo, you will see how to:


<a name="technologies" />
### Key Technologies ###

<a name="setup" />
### Setup and Configuration ###
Follow these steps to setup your environment for the demo.

1. Follow the steps detailed in [this link](http://docs.nuget.org/docs/creating-packages/hosting-your-own-nuget-feeds) to setup local sources for the following directories:

	1. **C:\Program Files (x86)\Microsoft Web Tools\Packages**
	1. **C:\Program Files (x86)\Microsoft ASP.NET\ASP.NET Web Stack 5\Packages**

	![NuGet Sources](images/nuget-sources.png?raw=true)

1. Create a (free) web site in Windows Azure and deploy the web site that is part of the **GeekQuiz.sln** solution located under the **source\end** folder.

1. Register a new user account.

1. Open the **StressGeekQuiz.sln** solution located under **source\end**.

1. In the **Solution Explorer**, double-click **WebTest1.webtest**.

1. Select the **http://geekquizdemo.azurewebsites.net** node, as shown in the following figure.

	![Node Selection](images/node-selection.png?raw=true)

1. In the **Properties** window, update the **Url** field to point to the site you just created.

	![Url Change](images/url-change.png?raw=true)

1. Save all files and close the solution.

1. Create a Windows Azure storage account (e.g. _geekquizstorage_), create a blob container named _images_ and upload the **logo-big.png** image located inside the **source\assets** folder.

<a name="Demo" />
## Demo ##
This demo is composed of the following segments:

1. [Configuring auto-scaling](#segment1)
1. [Load testing with Visual Studio](#segment2)
1. [Scaling GeekQuiz using Azure storage](#segment3)
1. [Auto-scaling result](#segment4)

<a name="segment1" />
### Configuring auto-scaling ###

1. Open the [Azure Management portal](https://manage.windowsazure.com/) and log in with your credentials.

1. Select the web sites tab.

	![Web Sites](images/web-sites.png?raw=true)

1. Click the website where you deployed GeekQuiz during the setup steps.

1. Open the scaling configuration page.

	![Scale](images/scale.png?raw=true)

1. Change the web site's mode to **Standard**.

	![Web Site Mode](images/web-site-mode.png?raw=true)

1. Clear all other web sites from the list of sites to be updated.

	![Clear Web Sites](images/clear-web-sites.png?raw=true)

1. Show that there is only one instance.

	![One Instance](images/one-instance.png?raw=true)

1. Select the **CPU** metric for scaling.

	![CPU scaling](images/cpu-scaling.png?raw=true)

1. Change the target CPU to 20-40.

	![Target CPU](images/target-cpu.png?raw=true)

	> **Speaking point:** Explain that this is done as we cannot ensure that a bigger load is generated with VS.

1. Save the changes. 
	
	> **Note:** Don't close the management portal.


<a name="segment2" />
### Load testing with Visual Studio ###

1. Open the **StressGeekQuiz.sln** solution located under **source\end**.

1. In the **Solution Explorer**, double-click **LoadTest1.loadtest**.

1. Run the load test.

	![Run Load Test](images/run-load-test.png?raw=true)

1. Open a new instance of Visual Studio.

	> **Speaking point:** Let's take a look at how that solution can be built.

1. Open the **New Project** dialog.

1. Select **Test** in the templates tree, and select **Web Performance and Load Test project**.

	![Test Project](images/test-project.png?raw=true)

1. Click **OK**.

1. Right-click **WebTest1** and select **Add Request**.

	![Add Request](images/add-request.png?raw=true)

1. Select the new node.

1. In the **Properties** window, update the **Url** field to point to the Azure web site.

	![Url Change](images/url-change.png?raw=true)

1. Right-click **WebTest1** and select **Add Loop...**.

	![Add loop](images/add-loop.png?raw=true)

1. Select the **For Loop** rule.

	![For Loop](images/for-loop.png?raw=true)

1. Update the following values:
	
	1. **Terminating value:** 1000.
	1. **Context Parameter Name:** Iterator.
	1. **Increment Value:** 1.

	![Values](images/values.png?raw=true)

1. Select the GeekQuiz request as the first and last item of the loop.

	![Items](images/items.png?raw=true)

1. Click **OK**.

1. In the **Solution Explorer**, right-click the **WebAndLoadTestProject1** project, expand the **Add** menu and select **Load Test...**. A wizard will start.

	![Load Test](images/load-test.png?raw=true)

1. Click **Next**.

1. Select **Do not use think times** and click **Next**.

	![Think times](images/think-times.png?raw=true)

1. Change the **User Count** to **250** users and click **Next**.

	![User Count](images/user-count.png?raw=true)

1. Select **Based on sequential test order** and click **Next**.

	![Text Mix](images/text-mix.png?raw=true)

1. Click **Add...**.

1. Double-click **Web Test 1** and click **OK**.

	![Add Tests](images/add-tests.png?raw=true)

1. Click **Next**.

1. In the **Network Mix** page, click **Next**.

1. Select **Internet Explorer 10.0** as the browser type and click **Next**.

1. In the **Counter Sets** page, click **Next**.

1. Set the load test duration to 5 minutes and click **Finish**.

	![Load test duration](images/load-test-duration.png?raw=true)

1. Close the current instance of **Visual Studio**.

<a name="segment3" />
### Scaling GeekQuiz using Azure storage ###

1. Open Internet Explorer.

1. Navigate to the image that you uploaded to Azure storage during setup. Foe example, if the name of the storage account is _geekquizstorage_ the URL for the image will be _http://geekquizstorage.blob.core.windows.net/images/logo-big.png_.

	![Logo big](images/logo-big.png?raw=true)

1. Open the **GeekQuilz.sln** solution located under **source\end**.

1. Open the site's **web.config** file for edition.

1. Find the `<system.webServer>` element.

1. Highlight the URL rewrite rule as shown in the following figure.

	![Highlighting Rewrite Rule](images/highlighting-rewrite-rule.png?raw=true)

1. Back in Internet Explorer, open the deployed GeekQuiz site (log in if necessary).

	![Geek Quiz with Image](images/geek-quiz-with-image.png?raw=true)

1. Press **F12** to launch the development tools, select the **Network** tab and start recording.

	![Start recording](images/start-recording.png?raw=true)

1. Press **CTRL + F5** to refresh the web page.

1. Once the page has finished loading, switch back to the development tools and show that the request for the image was redirected to Azure storage.

	![Redirect in Dev Tools](images/redirect-in-dev-tools.png?raw=true)

<a name="segment4" />
### Auto-scaling result ###

1. Back in the managemtn portal, press **CTRL + F5** to refresh the page.

1. Show that a new instance was automatically deployed.

	![New Instance](images/new-instance.png?raw=true)
