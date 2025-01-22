# Automations Tab

The **Automations** tab is one of the most essential features in SyncEngine, enabling you to create and manage powerful workflows. This tab allows you to design automations tailored to your needs by specifying inputs, data sources, and tasks.

___

## Creating a New Automation

When creating a new automation, you will need to provide the following details:

- **Name**: Used for your reference to track and identify the automation’s purpose.
- **Description**: An optional field to provide additional context or notes about the automation.
- **Endpoint**: A required field that serves as the trigger point for the automation. You will need to call this endpoint to start the automation.

## Configuring the Automation

Once the automation is created, you’ll be presented with various options. While the interface offers immense flexibility, it can initially seem overwhelming. Here's how to get started:

### 1. Select the Data Source
Choose the source of data for the automation by selecting one of the following options:

- **Request**: Use this option if you want to send all required data to the automation's endpoint when it is triggered.
- **Retrieve**: Use this option if the automation should fetch data from an external source. After selecting this option, a default **retrieve** task will appear. You can configure this task to define the external source.

### 2. Add Tasks and Flows
After defining the data source, you can:
- Add individual tasks that define the specific actions or operations to be performed.
- Create flows to organize tasks logically and control the execution order.

Tasks and flows can include data processing, conditional branching, integrations with external services, and more.

### 3. Test and Preview
At any point, you can use SyncEngine’s **Previewer** to test or inspect the automation. The Previewer provides insights into how the automation functions and what outputs it produces. To access the Previewer, press the play button as shown in the interface.

## Benefits of the Automations Tab
- **Flexibility**: Build custom workflows that adapt to your needs.
- **Scalability**: Handle complex integrations and large-scale tasks with ease.
- **User-Friendly Testing**: The Previewer simplifies debugging and ensures your automation works as expected.

## Tips for Effective Automation
- Use meaningful names and descriptions to keep your automations organized.
- Start small: Begin with basic tasks before incorporating advanced flows and integrations.
- Leverage the Previewer frequently to test changes incrementally.
- In the Previewer you can use the "Dry run" function, this will NOT send data to external sources.

## Additional Resources
For detailed guides on specific tasks or troubleshooting common issues, visit the [SyncEngine Documentation](https://docs.syncengine.io) or reach out to our [Support Team](mailto:info@syncengine.io).

---

Start exploring the **Automations** tab today and unlock the potential of SyncEngine’s workflow automation capabilities!