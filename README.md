This repository contains Big Query code using Google Analytics raw data monitoring the performance of Google Analytics tags via Google Tag Manager. 

I have developed a looker studio dashboard (https://lookerstudio.google.com/reporting/2a2b938e-f964-4f3d-90aa-f4ec33eec7d5) that brings the insights to life. 

The steps required:

1. To be able to generate the data to build the dashboard it requires implementing this GTM container > https://github.com/GTMRecipeContainers/Google-Analytics-4-Enhanced-E-commerce it will require configuring the triggers + GA4 event tag that works best for the website
2. Google Analytics is connected to Big Query
3. The Big Query code provided, create and save the views in Big Query
4. Make a copy of the looker studio dashboard and connect it to the saved views

Watch-Outs: 

1. To be able to monitor the performance of Google Analytics tags the most important is the architecture of each tag that is shared in the GitHub GTM link provided above. Each GA-GTM event tag requires adding the following event parameters: tag_name, tag_catgeory and tag_date_creation. These are additional info that will be available in Big Query for each event/tag. To be able to monitor the performance correctly having these event parameters correclty inputted will provide far greater insights
2. The Event Variable Settings shared in the GitHub GTM link provided above should be a default for every GA4 event tag in Google Tag Manager. The Event Variable Settings has the following event parameters: container_id, container_version and timestamp, container_id + container_version are coming from built-in variables whwre timestamp is coming from a user defined variable which can be found in the GitHub GTM container. These provide addtional rich information in Big Query for each event.tag. Once the Event Variable Settings are set it should not be changed unless addtional event parameters are being added  
3. Health Status is used in the detailed log, it will require Add Field > Add Calculated Field > Label Health Status > Copy the below and tweak where required

 CASE 
  WHEN tag_name = 'live google analytics 4 pageview' AND event_count < 1000 THEN '⚠️ Low Traffic'
  WHEN tag_name = 'live google analytics 4 - ecommerce purchase' AND event_count < 50 THEN '⚠️ Low Sales'
  WHEN tag_name = 'live google analytics 4 ee - add to cart' AND event_count < 200 THEN '⚠️ Low Cart Adds'
  ELSE '✅ Healthy'
END
  
