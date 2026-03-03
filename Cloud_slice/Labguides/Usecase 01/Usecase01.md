# Use Case 1 - Build connected data models with Fabric IQ Ontology​

**Introduction**

In modern data platforms, enterprises often need a **business-centric
semantic layer** that unifies meaning across diverse data sources and
analytical models. The *Ontology (preview)* feature in Microsoft Fabric
IQ enables you to build this layer by defining **enterprise concepts**
(like products, stores, and events) and **their relationships**, then
binding these definitions to real data across your lakehouse, semantic
models, and event streams.

In the scenario, a fictional company called **Lakeshore Retail**, which
sells ice cream at multiple locations. Using sample data, the tutorial
shows how to set up your environment and begin building an ontology that
captures business concepts such as *Store*, *Products*, and *SaleEvent*.
You’ll also connect streaming data (like freezer temperatures from
Eventhouse) to these concepts so the ontology can support **cross-domain
reasoning and queries**, for instance: *“Which stores have lower ice
cream sales when freezer temperature rises above –18 °C?”*

**Objectives**

- Prepare a Microsoft Fabric workspace with required services, including
  Lakehouse, Eventhouse, and Ontology (preview).

- Build a business-centric ontology by defining core entity types such
  as Store, Products, SaleEvent, and Freezer.

- ind static data from OneLake tables and time-series data from
  Eventhouse to ontology entities.

- Create meaningful relationships between entities to represent real
  business processes (for example, Store has SaleEvent and Store
  operates Freezer).

- Explore and validate the ontology using entity instances, relationship
  graphs, and query builder filters.

- Enable natural language querying by integrating the ontology with a
  Fabric Data Agent (preview).

# Exercise 1: Environment Setup

## Task 1: Create a Fabric workspace

In this task, you create a Fabric workspace. The workspace contains all
the items needed for this lakehouse tutorial, which includes lakehouse,
dataflows, Data Factory pipelines, the notebooks, Power BI datasets, and
reports.

1.  Open your browser, navigate to the address bar, and type or paste
    the following URL: +++https://app.fabric.microsoft.com/+++
    then press the **Enter** button and sign in with your credentials

    |  |   |
    |---|----|
    |Username	|+++@lab.CloudPortalCredential(User1).Username+++|
    |TAP	|+++@lab.CloudPortalCredential(User1).AccessToken+++|


2.  In the Workspaces pane, click on **+New workspace** tile

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image1.png)

3.  In the **Create a workspace** pane that appears on the right side,
    enter the following details, and click on the **Apply** button.
	
    |  |  |
    |---|----|
    |Name	|+++Fabric IQ Ontology@lab.LabInstance.Id+++|
    |License Mode	| **Fabric capacity**|
    |Model Storage Format	|**Small dataset storage format**|

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image2.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image3.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image4.png)

## Task 2: Create a lakehouse

1.  Create a new lakehouse by clicking on the **+New item** button in
    the navigation bar.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image5.png)

2.  Filter by, and select, the +++Lakehouse+++ tile.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image6.png)

3.  In the **New lakehouse** dialog box, enter +++IQ_Lakehouse@lab.LabInstance.Id+++ in the **Name** field and **unselect** the lakehouses schemas.Click on the **Create** button and open the new lakehouse.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image7.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image8.png)

4.  You will see a notification stating **Successfully created SQL
    endpoint**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image9.png)

## Task 3: Ingest sample data

1.  In the **IQ_Lakehouse** page, navigate to **Get data in your
    lakehouse** section, and click on **Upload files as shown in the
    below image.**

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image10.png)

2.  On the Upload files tab, click on the folder under the Files

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image11.png)

3.  Browse to **C:\Lab Files\Cloud_slice\LabFiles\Lab1** on your VM, then
    select **DimProducts.csv, DimStore.csv, FactSale.csv** and
    **Freezer.csv** file and click on **Open** button.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image12.png)

4.  Then, click on the **Upload** button and close the **Upload
    files** dialog by selecting the **X** icon for the dialog.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image13.png)

    ![A screenshot of a upload box AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image14.png)

5.  Click and select refresh on the **Files**. The file appear.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image15.png)

6.  In the **Lakehouse** page, Under the Explorer pane select **Files**.
    Now, hover your mouse over the **DimProducts.csv** file. Click on
    the horizontal ellipses **(…)** beside **DimProducts.csv** .
    Navigate and click on **Load Table**, then select **New table**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image16.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image17.png)

7.  In the **Load file to new table** dialog box, click on
    the **Load** button.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image18.png)

8.  Now successfully created **DimProducts** table

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image19.png)

9.  Select the **DimProducts** table to preview the data.

    >[!Note]: You may need to select the **Refresh** button more
than once to preview the data.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image20.png)

10. Repeat Steps 7 through 9 to push the remaining files into the
    tables.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image21.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image22.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image23.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image24.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image25.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image26.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image27.png)

11. From the left navigation bar, select **Fabric IQ Ontology**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image28.png)

## Task 4: Prepare the eventhouse

Follow these steps to upload the device streaming data file to a KQL
database in Eventhouse.

1.  On the **Fabric IQ Ontology** home page, select **+New item** and
    select **Eventhouse**. 

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image29.png)

2.  Name the Eventhouse +++TelemetryDataEH+++ and click on
    the **Create** button.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image30.png)

3.  The eventhouse opens when it's ready    ![A screenshot of a computer
    AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image31.png)

4.  Open the KQL database by selecting its name.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image32.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image33.png)

5.  On the lower ribbon of your **KQL database**, click on **Get data**,
    then select **Local file** to upload files from your local system
    into the database.
    ![A screenshot of a computer AI-generated content
    may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image34.png)

7.  Select the target option to ingest data into a new table, click +
    New table, and enter a table name as +++**FreezerTelemetry**+++.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image35.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image36.png)

8.  Select the destination table, then drag and drop the files or click
    *Browse for files* to upload the data.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image37.png)

9.  Browse to **C:\Lab Files\Cloud_slice\LabFiles\Lab1** on your VM, then
    select ***FreezerTelemetry*.csv** file and click on **Open** button.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image38.png)

10.  Click on **Next** button

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image39.png)

11. Then click on the **Finish** button.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image40.png)

12. Wait for the Data ingestion to be completed, and click **Close**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image41.png)

13. The KQL database shows the **FreezerTelemetry** table when you're
    done:

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image42.png)

14. Select **Fabric IQ Ontology** in the left navigation pane.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image43.png)

# Exercise 2: Building an ontology from OneLake

## Task 1: Create ontology (preview) item

1.  In your Fabric workspace, select **+ New item**. Search for and
    select the **Ontology (preview)** item.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image44.png)

2.  Enter +++**RetailSalesOntology**+++ for the **Name** of your
    ontology and select **Create**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image45.png)

    >[!Tip]: Ontology names can include numbers, letters, and
underscores. Don't use spaces or dashes.

3.  The ontology opens when it's ready.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image46.png)

Next, create entity types, data bindings, and relationships based on
data from your lakehouse tables.

## Task 2: Create entity types and data bindings

First, create entity types. Entity types represent types of objects in
a business. This step has three entity types: **Store**, **Products**,
and *SaleEvent*. After you create the entity types, create their
properties by binding source data columns in
the ***IQ_Lakehouse*** lakehouse tables.

### Add first entity type (Store)

1.  From the top ribbon or the center of the configuration canvas,
    select **Add entity type**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image47.png)

2.  Enter +++**Store**+++for the name of your entity type and
    select **Add Entity Type**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image48.png)

3.  The **Store** entity type is added to the configuration canvas, and
    the **Entity type configuration** pane is visible.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image49.png)

4.  To create entities from existing source data, switch to
    the **Bindings** tab. Select **Add data to entity type**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image50.png)

5.  Next, choose your data source.Select the **IQ_Lakehouse** lakehouse and select **Connect**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image51.png)

12. Select the **dimstore** table and select **Next**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image52.png)

6.  Configure a static data binding for the following properties.

    a)  For **Binding type**, don't change the default selection
        of **Static**.

    b)  Under **Bind your properties**, the columns from
        the **dimstore** table populate automatically. The **Source
        column** side lists their names in the source data, and
        the **Property name** side lists their corresponding property
        names on the *Store* entity type within ontology. Don't change
        the default property names, which match the source column names.

    c)  Select **Save**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image53.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image54.png)

7.  Back in the **Entity type configuration** pane, the data binding is
    visible. Next, select **Add entity type key**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image55.png)

8.  Select **StoreId** as the key property and select **Save**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image56.png)

9.  Now the **Store** entity type is ready. Continue to the next section
    to create the remaining entity types.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image57.png)

	### Add other entity types (Products, SaleEvent)

10. Follow the same steps that you used for the **Store** entity type to
    create the entity types described in the following table. Each
    entity has a static data binding with the default columns from its
    source table.

	|  |   |   |
	|---|---|---|
	|Entity type name|	Source table in IQ_Lakehouse	|Entity type key|
	|+++Products+++|  |dimproducts	|ProductId|
	|+++SaleEvent+++	|factsales	|SaleId|


    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image58.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image59.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image60.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image61.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image62.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image63.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image64.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image65.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image66.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image67.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image68.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image69.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image70.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image71.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image72.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image73.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image74.png)

11. When you're done, you see these entity types listed in the **Entity
    Types** pane.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image75.png)

## Task 3: Create relationship types

Next, create relationship types between the entity types to represent
contextual connections in your data.

**Store has SaleEvent**

1.  Select **Add relationship** from the menu ribbon.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image76.png)

2.  Enter the following relationship type details and select **Add
    relationship type**.

	- **Relationship type name**: +++has+++

	- **Source entity type**: Store

	- **Target entity type**: SaleEvent

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image77.png)

3.  The **Relationship configuration** pane opens, where you can
    configure additional information. Enter the following details (some
    fields become visible based on other selections) and
    select **Create**.

    1.  **Source data**: Select your tutorial workspace,
        the **IQ_Lakehouse**  lakehouse, and the **factsales** table.
        This table in the source data can
        link **Store **and **SaleEvent** entities together, because it
        contains identifying information for both entity types. Each row
        in this table references a store and a sale event by ID.

    1.  **Source entity type \Source column**: Select **StoreId**.
        This setting specifies the column in the relationship source
        data table (*factsales \* StoreId) whose values match the key
        property defined on the *Store* entity (*dimstore \* StoreId).
        In the tutorial data, the column name is the same in both
        tables.

    1.  **Target entity type \Source column**: Select SaleId. This
        setting specifies the column in the relationship source data
        table whose values match the key property defined on
        the **SaleEvent** entity. In this case, the relationship data
        source and the entity data source both use
        the **factsales** table, so you're selecting the same column.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image78.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image79.png)

Now the first relationship is created, and bound to data in your
source table. Continue to the next section to create another
relationship type.

### Products soldIn SaleEvent

4.  Follow the same steps that you used for the first relationship type
    to create the relationship type described in the following table.

	|   |   |   |
	|----|----|----|
	|Relationship type name|Source data table|	Source entity type	|Target entity type|
	|+++soldIn+++	|Tutorial workspace IQ_Lakehosefactsales|	Products For Source column, select ProductId.	SaleEvent|For Source column, select SaleId.|


    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image80.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image81.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image82.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image83.png)

	When you're done, you have two relationships targeting
	the **SaleEvent** entity type. To see the relationships, select
	the **SaleEvent** entity type from the **Entity Types** pane. You see
	its relationships on the configuration canvas.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image84.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image85.png)

# Exercise 3: Enrich the ontology with additional data

In this exercise, you enrich your ontology by adding a
new ***Freezer* **entity type. This entity type adds more domain context
and introduces properties for time series data, which reflects live
operational information.

>[!Note]: For both static and time series data, you can create properties without
binding data and bind data later, or create properties and bind data to
them in a single step. This article demonstrates both approaches.

Finally, you create a new relationship type to represent the connection
between a store and its freezers.

## Task 1: Create Freezer entity type and add properties

Follow these steps to create the *Freezer* entity type and add
properties to it. The properties aren't bound to data yet.

1.  Select **Add entity type** from the top ribbon.
    Enter +++Freezer+++ for the name of your entity type and
    select **Add Entity Type**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image86.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image87.png)

2.  In the **Entity type configuration** pane, go to
    the **Properties** tab. Select **Add properties**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image88.png)

3.  Add the following properties and select **Save**.

    |    |  |   |
    |----|----|---|
    |Name	|Value type	|Property type|
    |+++FreezerId+++	|String	|Static|
    |+++Model+++	|String	|Static|
    |+++minSafeTempC+++|	Double|	Static|
    |+++StoreId+++	|String|Static|


	>[!Note]: Property names must be unique across all entity types.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image89.png)

4.  Here's what it looks like before saving:

5.  Select **Add entity type key**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image90.png)

6.  Select **FreezerId** as the key value and click on **Save**

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image91.png)

## Task 2: Bind static data to properties

Next, bind static data to the properties you created on
the *Freezer* entity type.

1.  In the **Entity type configuration** pane, go to
    the **Bindings** tab. Select **Add data to entity type**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image92.png)

2.  From **OneLake catalog**, select the **IQ_Lakehouse** Lakehouse
    and click **Connect** to establish the data connection.
     ![A screenshot of a computer AI-generated content may be
    incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image93.png)

4.  Select the **freezer** table from the **IQ_Lakehouse** data source,
    then click **Next** to proceed to the data binding step

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image94.png)

5.  Configure a static data binding for the properties.

    a.  For **Binding type**, use the default selection of **Static**.

    b.  Under **Bind your properties**, the properties you created
        populate automatically with links to matching columns from
        the *freezer* table.

    c.  Select **Save**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image95.png)

6.  Now the **Freezer** entity has static data bound to it.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image96.png)

## Task 3: Bind time series data to additional properties

Next, add time series data on the **Freezer **entity, by creating new
properties and binding time series data to them in a single data binding
operation.

1.  In the **Entity type configuration** pane's **Bindings** tab,
    select **Add data to entity type**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image97.png)

2.  From **OneLake catalog**, select the **TelemetryDataEH** Eventhouse
    and click **Connect** to establish the data connection.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image98.png)

3.  Select the **Freezer Telemetry** table from the **TelemetryDataEH data source** data source,
    then click **Next** to proceed to the data binding step.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image99.png)

4.  Configure a time series data binding.

    1.  For **Binding type**, keep the default selection
        of **Timeseries**. For **Source data timestamp column**,
        select timestamp.

    2.  Under **Bind your properties \Static**, two source data
        columns populate that match static properties already defined on
        the entity. Keep them as they are.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image100.png)

3.  Under **Bind your properties \Timeseries**, the time series
    columns from the **FreezerTelemetry**table populate automatically
    with matching property names for the *Freezer* entity type. Keep the
    default selections.

4.  Select **Save**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image101.png)

5.  Now the ***Freezer* entity** has two data bindings: one with
    **static** data from the ***freezer* lakehouse** table and one with
    streaming data from the **FreezerTelemetry** eventhouse table.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image102.png)

## Task 4: Add relationship type

Finally, create a new relationship type to represent the connection
between a store and its freezers.

**Create Store operates Freezer**

1.  Select **Add relationship** from the menu ribbon.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image103.png)

2.  Enter the following relationship type details and select **Add
    relationship type**.

    1.  **Relationship type name**: +++operates+++

    2.  **Source entity type**: Store

    3.  **Target entity type**: Freezer

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image104.png)

3.  The **Relationship configuration** pane opens, where you can
    configure additional information. Enter the following details (some
    fields become visible based on other selections) and
    select **Create**.

1.  **Source data**: Select your workspace, the **IQ_Lakehouse**
	lakehouse, and the **freezer** table. This table in the source
	data can link **Store** and **Freezer** entities together,
	because it contains identifying information for both entity
	types. Each row in this table references a store and a freezer
	by ID.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image105.png)

2.  **Source entity type \Source column**: Select **StoreId**. This
    setting specifies the column in the relationship source data table
    (*freezer \* StoreId) whose values match the key property defined
    on the *Store* entity (*dimstore \* StoreId). In the tutorial data,
    the column name is the same in both tables.

3.  **Target entity type \Source column**: Select FreezerId. This
    setting specifies the column in the relationship source data table
    whose values match the key property defined on the **Freezer** entity.
    In this case, the relationship data source and the entity data
    source both use the **freezer** table, so you're selecting the same
    column.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image106.png)

	Here's what the relationship configuration looks like:

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image107.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image108.png)

# Exercise 4: Preview the ontology

In this exercise, explore your ontology by using the preview experience.
Inspect entity instances that instantiate your entity types with data,
and explore graph-shaped context across sales and device streaming data.

## Task 1: Preview entity instances

When you bound data to your entity types in previous tutorial steps,
ontology automatically created instances of those entities that are tied
to the source data rows. In this section, you use the preview experience
to view those entity instances.

1.  Select the **SaleEvent** entity type, and select **Entity type
    overview** from the top ribbon.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image109.png)

2.  It might take a few minutes for the ontology overview to load the
    first time.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image110.png)

    ![A screenshot of a graph AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image111.png)

3.  Scroll down to the **Entity instances** section. Verify that it
    shows entity instances, with unit counts and revenue populated from
    the **factsales** lakehouse table.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image112.png)

	>[!Tip]:  If data bindings don't load, confirm that the source data
	tables exist with matching column names, and that your Fabric identity
	has data access.

4.  Open the **Freezer** entity type in the preview experience, by
    selecting it in the **Entity Types** pane and selecting **Entity
    type overview** from the top ribbon.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image113.png)

5.  Update the time range from the default of *Last 30 minutes* to a
    custom date range that begins on *Fri Jan 01 2025 at 12:00 AM*, ends
    on *Mon Aug 04 2025 at 12:00 AM*, and has a **Time
    granularity** of *1 minute*.

    ![Screenshot of the time selector.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image114.png)

6.  Observe the time series data that's visible from
    different *Freezer* entity instances.

    ![Screenshot of the time series tiles.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image115.png)

## Task 2: Preview ontology graph

The preview experience also contains a **Relationship graph**, which you
use to visualize your ontology in a graph of nodes and edges.

1.  Use the tabs across the top of the preview experience to reopen
    the **SaleEvent** entity type. In the **Relationship graph** tile,
    select **Expand**.

    ![A screenshot of a graph AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image116.png)

2.  In the **graph**, observe the details of the relationships to
    the **SaleEvent** entity type from **Store **and ***Products*.**

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image117.png)

3.  Open the preview experience for the **Store** entity type,
    and **Expand** its relationship graph.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image118.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image119.png)

4.  In the graph, observe the relationship
    between **Store** and ***SaleEvent**, and the relationship
    between **Store** and ***Freezer**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image120.png)

5.  Then, select **Run query** in the query builder ribbon to run the
    default query and see a graph of entity instances and their
    connections.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image121.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image122.png)

## Task 3: Query graph instances

1.  In the relationship graph view, you can query your ontology for
    entity instances that meet certain criteria. Use the **Query
    builder** filters in the top ribbon to craft queries.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image123.png)

	First, craft this query: *Show all freezers that are operated in the
	Paris store.

2.  In the **Store** entity's relationship graph, select **Add filter \
    Store \StoreId** from the query builder ribbon.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image124.png)

3.  Set the filter for StoreId = S-PAR-01 and select Apply. This value
    is the store ID for the Paris store.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image125.png)

4.  In the **Components** pane, uncheck **SaleEvent** so that the only
    checked fields are **Nodes \Store**, **Nodes \Freezer**,
    and **Edges \operates**.

    ![Screenshot of filtering the components.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image126.png)

5.  Select **Run query** and verify that the instance graph shows two
    freezers connected to the *Paris* store.

    ![Screenshot of the freezers that are connected to the filtered
store.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image127.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image128.png)

6.  Select **Clear query** to clear the query results, and use
    the **Remove filter** options to remove the store filter.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image129.png)

	Next, craft this query: **Show all stores that have made a sale with a
	revenue greater than 150.**

7.  Select **Add a node** and add a node for **SaleEvent.**

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image130.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image131.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image132.png)

8.  In the **Components** pane, check the boxes next to **Nodes \
    Store** and **Edges \has** to add them to the graph.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image133.png)

9.  From the query builder ribbon, select **Add filter \SaleEvent \
    RevenueUSD**. Set the filter for **RevenueUSD** \150.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image134.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image135.png)

10. Select **Run query** and verify that the instance graph shows two
    stores that meet the filter for their connected sale events. You can
    also select the nodes in the graph to get details of the specific
    sale events.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image136.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image137.png)

This process allows you to inspect the paths that connect operational
issues (like rising freezer temperature at certain stores) to business
outcomes (sales).

# Exercise 5: Create data agent

Ontology (preview) integrates with [Fabric data agent
(preview)](https://learn.microsoft.com/en-us/fabric/data-science/concept-data-agent) to
let you ask questions in natural language, and get answers grounded in
the ontology's definitions and bindings.

## Task 1:Create data agent with ontology (preview) source

Follow these steps to create a new data agent that connects to your
ontology (preview) item.

1.  Now, click on **Fabric IQ Ontology XX** on the left-sided navigation
    pane.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image138.png)

2.  In the **Fabric** home page, select **+New item.** In the Filter by
    item type search box, enter +++data agent+++ and select the Data
    agent

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image139.png)

3.  Enter +++RetailOntologyAgent+++ as the Data agent name and
    select **Create**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image140.png)

4.  In **RetailOntologyAgent** page, select **Add a data source**

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image141.png)

5.  In the OneLake catalog tab, select the **RetailSalesOntology**
    Ontology and select **Add.**
         ![A screenshot of a computer
    AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image142.png)

	When the agent is ready, it opens.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image143.png)

## Task 2: Provide agent instructions

>[!Note]: This step is added in response to a known issue affecting aggregation in queries.

Next, add a custom instruction to the agent.

1.  Select **Agent instructions** from the menu ribbon.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image144.png)

2.  At the bottom of the input box, add Support group by in GQL. This
    instruction enables better aggregation across ontology data.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image145.png)

3.  The instruction is applied automatically. Optionally, close
    the **Agent instructions** tab.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image146.png)

## Task 3: Query agent with natural language

Next, explore your ontology with natural language questions.

1.  Enter the following text and click on the **Submit icon** as shown
    in the below image.

	+++For each store, show any freezers operated by that store that
	ever had a humidity lower than 46 percent.+++

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image147.png)

    ![A screenshot of a chat AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image148.png)

2.  Enter the following text and click on the **Submit icon** as shown
    in the below image.

	+++What is the top product by revenue across all stores?+++

    ![A screenshot of a chat AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image149.png)

    ![A screenshot of a chat AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image150.png)

	Notice that the responses reference entity types
	(**Store**, **Products**, **Freezer**) and their relationships, not just raw
	tables.

    ![Screenshot of the result of a query.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image151.png)

>[!Tip]:  If you see errors that say there's no data while running the
example queries, wait a few minutes to give the agent more time to
initialize. Then, run the queries again.

Continue exploring the data agent by trying out some prompts of your
own.

**Task 7: Clean up resources**

1.  Select your workspace, the **Fabric IQ OntologyXX** from the
    left-hand navigation menu. It opens the workspace item view.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image152.png)

2.  Select the ... option under the workspace name and select
    **Workspace settings**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image153.png)

3.  Navigate to the bottom of the General tab and select **Remove this
    workspace**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image154.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrciqdepth/refs/heads/main/Cloud_slice/Labguides/Usecase%2001/media/image155.png)

**Summary**

This use case demonstrates how Microsoft Fabric IQ Ontology (preview)
can be used to create a connected, semantic data model that represents
real-world business concepts and their relationships. By combining
structured lakehouse data with streaming telemetry data, the ontology
provides a unified, business-friendly view of enterprise data.

Through entity definitions, data bindings, and relationship modeling,
users can analyze how operational signals—such as freezer temperature or
humidity—relate to business outcomes like sales and revenue. The use
case also highlights how ontologies power graph exploration and natural
language queries through Fabric data agents, enabling deeper insights
without requiring users to understand underlying tables or schemas.

Overall, this use case shows how Fabric IQ Ontology helps bridge
operational data and analytics, supporting smarter decision-making
across domains.


