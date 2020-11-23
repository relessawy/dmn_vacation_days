## Decision Model & Notation - Employee Vacation Days

In this lab we will create a decision that determines the number of vacation days assigned to an employee using Decision Model & Notation (DMN). The number of vacation days depends on age and years of service

* Every employee receives at least 22 days.
* Additional days are provided according to the following criteria:
1. Only employees younger than 18 or at least 60 years, or employees with at least 30 years of service will receive 5 extra days
2. Employees with at least 30 years of service and also employees of age 60 or more, receive 3 extra days, on top of possible additional days already given
3. If an employee has at least 15 but less than 30 years of service, 2 extra days are given. These 2 days are also provided for employees of age 45 or more. These 2 extra days can not be combined with the 5 extra days.

## Create a Decision Project
To define and deploy a DMN decision model, we first need to create a new project in which we can store the model. To create a new project 

- Click on the **home button -> Design -> MySpace**
- Click on the blue **Add project** button at the top right of the page
- Give the project the name `vacation-days-decisions`, and the description `Vacation Days Decisions` and click the **Add Button**.

![Create Project](https://github.com/relessawy/dm_lab_instructions/blob/master/images/add-project-vacation-days-decisions.png)


With the project created, we can now create our DMN model. Click on the blue **Add Asset** button.
In the **Add Asset** page, select **Decision** in the dropdown filter selector.

![New Asset](https://github.com/relessawy/dm_lab_instructions/blob/master/images/new-asset-decisions-filter.png)

Click on the **DMN** tile to create a new DMN model. Give it the name `vacation-days`. This will create the asset and open the DMN editor.

![Vacation Days](https://github.com/relessawy/dm_lab_instructions/blob/master/images/add-dmn-vacation-days.png)


## DMN Editor

The DMN Editor consists of a number of components:

* **Decision Navigator**: shows the nodes used in the Decision Requirements Diagram (DRD, the diagram), and the decisions behind the nodes. Allows for quick navigation through the model.
* **Decision Requirements Diagram Editor**: the canvas in which the model can be created.
* **Palette**: Contains all the DMN constructs that can be used in a DRD, e.g. Input Node, Decision Node, etc.
* **Expression Editor**: Editor in which DMN boxed expressions, like decision tables and literal expressions, can be created.
* **Property Panel**: provides access to the properties of the model (name, namespace, etc), nodes, etc.
* **Data Types**: allows the user to define (complex) datatypes.

![DRD](https://github.com/relessawy/dm_lab_instructions/blob/master/images/dmn-editor-components.jpg)

![Boxed Expressions](https://github.com/relessawy/dm_lab_instructions/blob/master/images/dmn-editor-decision-table.jpg)

![Data Types](https://github.com/relessawy/dm_lab_instructions/blob/master/images/dmn-editor-datatypes.jpg)


## Solution

Follow this step-by-step guide which will guide you through the implementation.

**Input Nodes**

The problem statement describes a number of different inputs to our decision:

* **Age** of the employee
* **Years of Service** of the employee

Therefore, we create 2 input nodes, one for each input:

Add an **Input** node to the diagram by clicking on the **Input** node icon and placing it in the DRD.

![Input](https://github.com/relessawy/dm_lab_instructions/blob/master/images/add-drd-input-node.png)

Double-click on the node to set the name. We will name this node `Age`.
With the `Age` node selected, open the property panel. Set the **Output data type** to `number`.

![Age_Input](https://github.com/relessawy/dm_lab_instructions/blob/master/images/Age_Input.png)

In the same way, create an **Input** node for `Years of Service`. This node should also have its **Output data type** set to `number`.

![Output Data Type](https://github.com/relessawy/dm_lab_instructions/blob/master/images/drd-input-nodes-complete.png)

Save the model.


**Constants**

The problem statement describes that every employee receives at least 22 days. So, if no other decisions apply, an employee receives 22 days. This is can be seen as a constant input value into our decision model.
In DMN we can model such constant inputs with a **Decision** node with a **Literal** boxed expression that defines the constant value:

Add a **Decision** node to the DRD

![Decision Node](https://github.com/relessawy/dm_lab_instructions/blob/master/images/add-drd-decision-node.png)

- Give the node the name `Base Vacation Days`.
- Click on the node to select it and open the property panel. Set the node's **Output data type** to `number`.
- Click on the node and click on the **Edit** icon to open the expression editor.

![Edit Decision Node](https://github.com/relessawy/dm_lab_instructions/blob/master/images/drd-decision-node-edit.png)

In the expression editor, click on the box that says **Select expression** and select **Literal expression**.

![Select Expression](https://github.com/relessawy/dm_lab_instructions/blob/master/images/select-expression.png)


Simply set the **Literal Expression** to `22`, the number of base vacation days defined in the problem statement.

![Select Expression](https://github.com/relessawy/dm_lab_instructions/blob/master/images/base-vacation-days-literal-expression.png)


Save the model.

**Decisions**

The problem statement defines 3 decisions which can cause extra days to be given to employees based on various criteria. Let's simply call these decision:

* Extra days case 1
* Extra days case 2
* Extra days case 3

Although these decisions could be implemented in a single decision node, we've decided, in order to improve maintainability of the solution, to define these decisions in 3 separate decision nodes.

In your DRD, create 3 decision nodes with these given names. Set their **Output data types** to `number`.
We need to attach both input nodes, **Age** and **Years of Service** to all 3 decision nodes. We can do this by clicking on an Input node, clicking on its arrow icon, and attaching the arrow to the Decision node.

![three_decision_nodes](https://github.com/relessawy/dm_lab_instructions/blob/master/images/add-drd-three-decision-nodes.png)


Select the **Extra days case 1** node and open its expression editor by clicking on the **Edit** button.
Select the expression **Decision Table** to create a boxed expression implemented as a decision table.
The first case defines 2 decisions which can be modelled with 2 rows in our decision table as such:
employees younger than 18 or at least 60 years will receive 5 extra days, or ...
employees with at least 30 years of service will receive 5 extra days

![case1](https://github.com/relessawy/dm_lab_instructions/blob/master/images/decision-table-case-1.png)


Note that the **hit-policy** of the decision table is by default set to `U`, which means `Unique`. This implies that only one rule is expected to fire for a given input. In this case however, we would like to set it to `Collect Max`, as, for a given input, multiple decisions might match, but we would like to collect the output from the rule with the highest number of additional vacation days. To do this, click on the `U` in the upper-left corner of the decision table. Now, set the **Hit Policy** to `Collect` and the **Builtin Aggregator** to `MAX`.

![Decision Table Hit Policy](https://github.com/relessawy/dm_lab_instructions/blob/master/images/decision-table-hit-policy.png)
 
Finally, we need to set the default result of the decision. This is the result that will be returned when none of the rules match the given input. This is done as follows:
- Select the output/result column of the decision table. In this case this is the column `Extra days case 1`
- Open the properties panel on the right-side of the editor.
- Expand the **Default output** section.
- Set the `Default output property` to `0`.

![Decision Table Default Output](https://github.com/relessawy/dm_lab_instructions/blob/master/images/decision-table-default-output.png)

Save the model

The other two decisions can be implemented in the same way. Simply implement the following two decision tables:

![case2](https://github.com/relessawy/dm_lab_instructions/blob/master/images/decision-table-case-2.png)

![case3](https://github.com/relessawy/dm_lab_instructions/blob/master/images/decision-table-case-3.png)

**Total Vacation Days**

The total vacation days needs to be determined from the base vacation days and the decisions taken by our 3 decision nodes. As such, we need to create a new Decision node, which takes the output of our 4 Decision nodes (3 decision tables and a literal expression) as input and determines the final output. To do this, we need to:

- Create a new Decision node in the model. Give the node the name `Total Vacation Days` and set its **Output data type** to `number`.
- Connect the 4 existing Decision nodes to the node. This defines that the output of these nodes will be the input of the next node.

![DRD Complete](https://github.com/relessawy/dm_lab_instructions/blob/master/images/drd-complete.png)


Click on the `Total Vacation Days` node and click on **Edit** to open the expression editor. Configure the expression as a literal exprssion.
- We need to configure the following logic:
- Everyone gets the Base Vacation Days.
- If both case 1 and case 3 add extra days, only the extra days of one of this decision is added. So, in that case we take the maximum.
- If case 2 adds extra days, add them to the total.
- The above logic  can be implemented with the following FEEL expression:

![Total Vacation Days Expression](https://github.com/relessawy/dm_lab_instructions/blob/master/images/total-vacation-days-expression.png)


Save the completed model.


**Deploying the Decision Service**

With our decision model completed, we can now package our DMN model in a Deployment Unit (KJAR) and deploy it on the Execution Server. To do this:

- In the bread-crumb navigation in the upper-left corner, click on `vacation-days-decisions` to go back to the project's Library View.
- Click on the **Deploy** button in the upper-right corner of the screen. This will package our DMN mode in a Deployment Unit (KJAR) and deploy it onto the Execution Server (KIE-Server).
- Go to the **Execution Servers** perspective by clicking on "Menu -> Deploy -> Execution Servers". You will see the **Deployment Unit** deployed on the Execution Server.

