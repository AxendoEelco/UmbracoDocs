---
versionFrom: 7.0.0
---

# Umbraco Forms on Cloud

In this article you can learn about how Umbraco Forms are handled on Umbraco Cloud and read about the workflow and best practices.

Umbraco Forms is a package that is included with your Umbraco Cloud project. It gives you a nice integrated UI where you can create forms for your website. The package is built specifically for Umbraco and is maintained by Umbraco HQ.

Read more about the product in the [Umbraco Forms section](../../../Add-ons/UmbracoForms).

## How are Forms handled on Cloud?

Umbraco Forms are handled as content and media. This means that you can transfer your Forms between environments using the same workflow you use for content and media.

Entries submitted are not transferred to the next environment, as they are *environment specific*. If you need to move entries from one environment to another, you need to run an export/import script on the databases.

## Recommended workflow

You can work with Forms on the environment of your choice. 

Testing.

When you need to test or use your forms on another environment you can

* transfer the forms to the next environment using **Queue for transfer** or
* **Restore** the forms on an environment lower in the workflow

For more information on how to handle content transfer / restores on Umbraco Cloud, checkout the following articles:

* [Transfer content, media and Forms](../Content-Transfer)
* [Restoring content](../Restoring-content)

### Did you create your Cloud project before June 18th 2019?

Then your Forms data are handled as meta data. When you create a Form, a UDA file will be generated. This UDA file will contain all the metadata from your form, and be very similar to the `JSON` file that is also generated when you create a new form - this file can be found in `~/App_Data/UmbracoForms/Data/forms`.

This means that your Forms will be deployed along with the rest of your metadata and structure files, e.g. Document Types, Templates and Stylesheets. 

We strongly recommend that you work with the Forms on your local or Development environment, following the [left-to-right deployment model](../../Deployment).

You can configure your project to handle Umbraco Forms data as content by following these steps:

1. Make sure your Forms are in sync between all your Cloud environments
2. Clone down the project to your local machine
3. Find and open `~/Config/UmbracoDeploy.settings.config`
4. Update the `transferFormsAsContent` value to `true`
   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <settings xmlns="urn:umbracodeploy-settings">
      <forms transferFormsAsContent="true" />
   </settings>
   ```
5. Push the change back to the Cloud environment
   * If you have more than 1 Cloud environment, make sure to deploy the change through to all of them
6. Your Forms are now a part of content / media transfers!

:::tip
Do you want to test this new setting you've configured?

Make a change to a form on your Live environment, and then use the **Restore** option in the Forms section on another environment.

You will see that the changes you made on Live, is now reflected on your other environments.
:::

## Upgrades

Umbraco Forms is part of the [auto-upgrades on Umbraco Cloud](../../Upgrades). Whenever a new patch is ready for release, we will automatically apply it to your Cloud project. There will be a message in the Umbraco Cloud Portal at least 5 days before we roll out new versions.

To avoid that the auto-upgrades overwrite any of your custom settings, we strongly encourage that you use [config transforms](../../Set-Up/Config-Transforms) when you need custom configuration, and [Themes](../../../Add-ons/UmbracoForms/Developer/Themes) when you need to customize your forms.

## Common issues with Forms on Cloud

### The Forms tree is missing

Some times you might experience that you loose the tree in the Forms section in the backoffice after a deployment.

![Missing tree from Forms section](images/missing-forms-tree.png)

In order to get the tree back, all you need to do is **restart the environment** from the Umbraco Cloud Portal.
