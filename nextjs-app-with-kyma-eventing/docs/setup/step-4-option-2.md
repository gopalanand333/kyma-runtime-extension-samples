# Setup SAP Event Mesh as your eventing backend for Kyma Runtime

## Setup steps

### 1. Add the required entitlements to your SAP BTP subaccount

1. In your BTP subaccount, select `Entitlements` -> `Configure Entitlements`.

   ![Configure Entitlements](../assets/setup-step-4/1.png)

2. Select `Add Service Plans`.

   ![Add Service Plans](../assets/setup-step-4/2.png)

3. Search for `Event Mesh`. Then, select its `default` and `standard (Application)` plans and select `Add 2 Service Plans`.

   ![Add Service Plans](../assets/setup-step-4/3.png)

4. Save your changes by selecting `Save`.

   ![Save your changes](../assets/setup-step-4/4.png)

### 2. Create an instance of SAP Event Mesh service in your Kyma Runtime (default plan)

1. Go to your Kyma workspace and select `Namespaces`. Then, select the `conference-registration` namespace.

   > **Note:** If you're following the example from the [previous step](step-4.md), then select the `conference-registration` namespace.. Otherwise, select your namespace instead.

   ![Select your namespace](../assets/setup-step-4/5.png)

2. Select `Service Management` -> `Catalog` -> Search for `Event Mesh` and select `Event Mesh`.

   ![Create service instance](../assets/setup-step-4/6.png)

3. Select `Add`.

   ![Create service instance](../assets/setup-step-4/7.png)

4. Enter the name of the instance as `kyma-enterprise-messaging-client`. Select the `default` plan and select `Add parameters`.

   ![Create service instance](../assets/setup-step-4/8.png)

5. Copy and paste the following JSON code snippet into the parameters field.

   ```shell
   {
   "options": {
      "management": true,
      "messagingrest": true
   },
   "rules": {
      "topicRules": {
         "publishFilter": [
         "${namespace}/*"
         ],
         "subscribeFilter": [
         "${namespace}/*"
         ]
      },
      "queueRules": {
         "publishFilter": [
         "${namespace}/*"
         ],
         "subscribeFilter": [
         "${namespace}/*"
         ]
      }
   },
   "resources": {
      "units": "10"
   },
   "version": "1.1.0",
   "emname": "kyma-enterprise-messaging-client",
   "namespace": "eu/kyma-enterprise-messaging-client/dev"
   }
   ```

6. Review the JSON code snippet to ensure that the value of `emname` is the same as the instance name. Then, select Create.

   ![Create service instance](../assets/setup-step-4/9.png)

7. Wait for the status of the service instance to change to `PROVISIONED`. Then, select `Add Service Binding`.

   ![Create binding](../assets/setup-step-4/10.png)

8. For `Name` enter `kyma-enterprise-messaging-client-binding` and for `Secret Name` enter `kyma-enterprise-messaging-client-secret`. Then, select `Create`.

   ![Create binding](../assets/setup-step-4/11.png)

### 3. Switch the default eventing of Kyma Runtime from NATS to SAP Event Mesh

1. After the previous step, wait for the status of the service instance binding to change to `READY`. Then, select the Secret name. For example, `kyma-enterprise-messaging-client-secret`.

   ![Create binding](../assets/setup-step-4/12.png)

2. Select `Edit`.

   ![Create binding](../assets/setup-step-4/13.png)

3. Select the `YAML` tab.

   ![Create binding](../assets/setup-step-4/14.png)

4. Enter the following YAML code snippet to add the `kyma-project.io/eventing-backend: beb` label to the Secret. Then, select `Update`.

   ```shell
   labels:
      kyma-project.io/eventing-backend: beb
   ```

   ![Create binding](../assets/setup-step-4/15.png)

## Optional steps to setup SAP Event Mesh Enterprise Messaging application in your SAP BTP cockpit

> **Note:** The following steps are optional and allow you to view, manage and monitor the SAP Event Mesh Enterprise Messaging application within your SAP BTP cockpit.

## 1. Create a subscription for SAP Event Mesh service in your SAP BTP account (standard plan)

1. Within your BTP subaccount, go to **Services** > **Service Marketplace**, search for **Event Mesh** and click **Create**.

   ![Create Event Mesh instance](../assets/setup-step-4/16.png)

2. Select the **standard** plan and click **create**.

   ![Create Event Mesh instance](../assets/setup-step-4/17.png)

## 2. Assign the required Role Collections to the admin user

1. Assign the required Role Collections to the admin user. In your BTP subaccount, select `Security` -> `Users`. Search for your user. Then, select the right arrow below the `Actions' column.

   ![Select Actions](../assets/setup-step-4/18.png)

2. Scroll down and click on the three dots below `Role Collections`. Then select `Assign Role Collection`.

   ![Select Actions](../assets/setup-step-4/19.png)

3. Select all the following options and select `Assign Role Collection`.

   * Enterprise Messaging Administrator
   * Enterprise Messaging Developer
   * Enterprise Messaging Display
   * Enterprise Messaging Subscription Administrator
   * Event Mesh Integration Administrator

   ![Select Actions](../assets/setup-step-4/20.png)

## 3. Navigate to the SAP Event Mesh Enterprise Messaging application

1. Go to `Services` -> `Instances and Subscriptions` and select the `Event Mesh` application.

   ![Select Actions](../assets/setup-step-4/21.png)

2. Select `kyma-enterprise-messaging-client`.

   ![Select Actions](../assets/setup-step-4/22.png)

3. Navigate through the various tabs and explore the user interface of the SAP Event Mesh Enterprise Messaging application.

   ![Select Actions](../assets/setup-step-4/23.png)

## Refer to the following documentation page for more information :arrow_lower_right&#58;

### [Use Kyma Eventing with SAP Event Mesh](https://help.sap.com/products/BTP/65de2977205c403bbc107264b8eccf4b/407d1266017f4b529b61665fa7408c41.html)

## Navigation

| [:house:](../../README.md) | :arrow_backward: [Setup : Step 4 - Apply the Event Registration Subscription)](step-4.md) | :arrow_forward: [Setup : Step 5 - Create an instance of SAP HANA Cloud](step-5.md) |
| -------------------------- | --------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
