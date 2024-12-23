# Implementing cookie consent with Google Tag Manager and Cookiebot CMP
> [!NOTE]
> This article is based on information as of January 2021. Please take a look at the official website for the latest updates.

With GDPR compliance becoming mandatory, many people might wonder, "What exactly should I do?" It's challenging to determine when to stop tags like Google Analytics, Google Ads, or Facebook Ads from firing, and under what conditions it's acceptable for them to fire. Implementing everything from scratch can be quite daunting.

In this article, we'll simplify the process and introduce a method to handle the bare minimum requirements using **Google Tag Manager** and **[Cookiebot](https://www.cookiebot.com/)** to obtain cookie consent and manage actions based on user responses.

## What is Cookiebot?

[Cookiebot](https://www.cookiebot.com/) is a service designed to comply with GDPR and CCPA regulations. It displays a popup to visitors, allowing them to control which cookies can be collected and automatically manages what can be captured based on their preferences.

The [pricing](https://www.cookiebot.com/en/pricing/) varies based on the size of your site, starting with a free plan for up to 100 pages and €9/month for up to 500 pages (as of April 2021). Choose a plan that suits your website's needs.

## Getting Started with an Account

First, register on the [Cookiebot](https://manage.cookiebot.com/en/signup) website and obtain a key. This key will be used later, so make sure to save it.

## Creating the Cookiebot CMP Tag

Next, we’ll set up the Cookie Consent popup tag, "Cookiebot CMP," in Google Tag Manager.

1. In Google Tag Manager, navigate to the **Tags** menu and select **Create New**.
2. Import the tag template and select "Cookiebot CMP."

   ![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3939399/5eeb2d59-be7a-59da-f54d-d3067c00e58c.png)
3. Enter the key you obtained from Cookiebot into the "Cookiebot ID" field.
4. Set the trigger to "All Pages" and save the tag.

   ![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3939399/19af62fd-3378-54dd-8e55-03a8e1dcdf8b.png)

## Creating the Cookie Consent Variable

Now, let’s create a variable. 

1. In Google Tag Manager, go to the **Variables** menu and select **Create New**.
2. Import the "Cookiebot Consent State" template (be careful not to select similarly named templates).
   
   ![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3939399/083a9b24-859c-3c3c-93d3-342bb04122a6.png)
4. Name the variable "Cookie Consent" and save it.

   ![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3939399/490c6251-bf58-6990-cb24-5f6cc8bf246f.png)

## Creating Triggers to Prevent Tag Firing

Next, triggers should be created to control whether marketing and analytics tags fire.

### Creating a Trigger for Blocking Marketing Tags

Create a trigger named "Cookie Consent Marketing – blocking":

- **Trigger Name**: Cookie Consent Marketing – blocking
- **Trigger Type**: Custom Event
- **Event Name**: `.*` (check "Use regex matching")
- **Condition**: Cookie Consent does not contain "marketing"

  ![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3939399/fd93ead5-6e1b-4b2c-ea40-7cea621ff554.png)

### Creating a Trigger for Blocking Analytics Tags

Create a trigger named "Cookie Consent Statistics – blocking":

- **Trigger Name**: Cookie Consent Statistics – blocking
- **Trigger Type**: Custom Event
- **Event Name**: `.*` (check "Use regex matching")
- **Condition**: Cookie Consent does not contain "statistics"

  ![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3939399/d3717430-8e3e-b2f4-84fd-6c4913206ec0.png)

## Updating Marketing Tags

Modify the triggers for tags like Google Analytics, Google Ads, and Facebook Ads.

1. For all Google Analytics tags, add the "Cookie Consent Statistics – blocking" trigger under the **Exception** section.

   ![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3939399/d3b41dee-fb75-16d5-999e-5cccb28fa2b5.png)
2. Similarly, for Google Ads, Facebook Ads, and other marketing tags, add the "Cookie Consent Marketing – blocking" trigger as an exception.

Finally, publish the workspace in Tag Manager.

When you visit the target site, a popup will appear, and tags will not fire based on the user's selections.

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3939399/26dbc51d-de35-29f0-eb4c-9891b6422bb3.png)

For instance, if you deselect all checkboxes except for "Necessary" and click OK, the "CookieConsent" value for cookies will reflect as `false`:

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3939399/bace1087-c266-e5eb-d8ef-912881aada96.png)

While it’s possible to develop or outsource a custom solution for GDPR and CCPA compliance, this can be expensive. Moreover, future changes in regulations might require additional updates. Using a service like Cookiebot is a practical way to mitigate risks effectively.
