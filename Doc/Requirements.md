#  Workflow Engine Microservice Design

## Overview

The **Workflow Engine Microservice** is designed to be a dynamic, modular, and extensible system that allows super administrators to create and manage any type of workflow logic. It is highly parameterizable, supporting various use cases such as authorizations, document generation, approvals, notifications, and more. The engine provides a flexible configuration interface, enabling super admins to design workflows that meet specific organizational needs.

---

## Key Features

1. **Dynamic Workflow Structure**: Supports complex workflow configurations with nodes, edges, and conditional transitions.
2. **Extensible Node Types**: Offers various node types like tasks, decisions, approvals, events, and sub-workflows.
3. **Dynamic Parameters and Fields**: Allows defining dynamic fields with validation rules for data collection and workflow decisions.
4. **Flexible Approval Logic**: Supports multiple approval processes, including single, multiple, conditional, and hierarchical approvals.
5. **Custom Workflow Outputs**: Enables workflows to generate documents, send notifications, or trigger webhooks.
6. **Comprehensive Workflow Settings**: Provides versioning, execution settings, and permissions for workflows.
7. **Intuitive Frontend Configuration**: Utilizes graphical interfaces (e.g., React Flow) for designing workflows and forms.

---

## Dynamic Workflow Features

### 1. Workflow Structure

- **Nodes**: Fundamental units representing steps or actions in a workflow.
  - **Start Node**: Initiates the workflow.
  - **Intermediate Nodes**: Actions, decisions, approvals, service calls.
  - **End Node**: Concludes the workflow.

- **Edges**: Define transitions between nodes, supporting:
  - **Conditional Transitions**: Based on rules or conditions (e.g., `if budget > 1000`).
  - **Parallel and Sequential Flows**: Allowing steps to execute simultaneously or in order.

- **Workflow Loops**: Support loops for retries or iterative processes.

### 2. Node Types

1. **Task Nodes**:
   - **Manual Task**: Requires user interaction or input.
   - **Automated Task**: Executes system actions (e.g., sending emails, API calls).
   - **Service Task**: Integrates with external microservices (e.g., document generation).

2. **Decision Nodes**:
   - **Conditional Logic**: Branches workflow paths based on conditions.
   - **Scripted Logic**: Uses scripts or expressions to determine the next path.

3. **Approval Nodes**:
   - Configurable approval processes (e.g., single, multiple, escalated).
   - Supports delegation, escalation, and time-based approvals.

4. **Event Nodes**:
   - **Timer Events**: Triggers based on time conditions (e.g., delays, deadlines).
   - **Message Events**: Waits for external events or messages (e.g., webhook callbacks).

5. **Sub-Workflow Nodes**:
   - Incorporates reusable workflows within other workflows for modularity.

6. **End Nodes**:
   - Marks the completion of a workflow or triggers final actions.

### 3. Dynamic Parameters

- **Dynamic Fields**:
  - Customizable fields for data collection (e.g., `budget`, `reason`).
  - Supports various data types: text, number, date, boolean, select lists.
  - Validation rules: required, patterns, length constraints.

- **Custom Actions**:
  - Define actions with dynamic parameters (e.g., API calls with placeholders).
  - Action types include validation, data transformation, and integrations.

- **Variables and Expressions**:
  - Use runtime variables (e.g., `${requestorName}`, `${approvalDate}`).
  - Support calculated fields using expressions.

### 4. Approval Logic

- **Parameterizable Approval Types**:
  - **Single Approval (SA)**: One approver required.
  - **Multiple Approvals (MA)**: Multiple approvers, can be sequential or parallel.
  - **Escalation Approval (EA)**: Escalates if not approved within a timeframe.
  - **Conditional Approval (CA)**: Approval required based on conditions.
  - **Parallel Approval (PA)**: Approvals happen simultaneously.
  - **Delegated Approval (DA)**: Approval delegated to another user.
  - **Multi-Level Approval (MLA)**: Hierarchical approvals.
  - **Auto Approval (AA)**: Automatically approved based on rules.
  - **Time-Based Approval (TBA)**: Approval required within a specific time.
  - **Batch Approval (BA)**: Group approvals processed together.

### 5. Workflow Outputs

- **Document Generation**:
  - Dynamically link document templates to workflows.
  - Populate documents with dynamic field data.

- **Notifications**:
  - Configurable recipients (e.g., approvers, requestors).
  - Multiple channels: email, SMS, push notifications.

- **Webhooks**:
  - Trigger external services upon certain workflow events.

### 6. Workflow Settings

- **Versioning**:
  - Maintain multiple versions of workflows.
  - Activate or archive specific versions.

- **Execution Settings**:
  - Choose between synchronous or asynchronous execution.
  - Define retry logic for failed steps.

- **Permissions**:
  - Control access to workflow creation, management, and execution.
  - Define roles and permissions for users.

### 7. Frontend Configuration Options

- **Graphical Workflow Builder**:
  - Visual interface for designing workflows using drag-and-drop.
  - Utilize tools like **React Flow** for an intuitive experience.

- **Dynamic Form Builder**:
  - Define data collection forms with dynamic fields and validations.
  - Support conditional visibility and dependencies between fields.

- **Template Library**:
  - Provide predefined workflow templates for common processes.
  - Allow cloning and customization of existing templates.

---

## Example Workflow Use Cases

### 1. **Employee Leave Request**

- **Steps**:
  1. **Submit Request**: Employee fills out a leave request form.
  2. **Manager Approval**: Single approval by the direct manager.
  3. **HR Approval**: Conditional approval by HR if leave exceeds a certain number of days.
  4. **Notification**: Employee receives notification of approval/rejection.
  5. **Document Generation**: Generate leave confirmation document if approved.

- **Dynamic Fields**:
  - `employeeId` (string)
  - `leaveType` (string, select list)
  - `startDate` (date)
  - `endDate` (date)
  - `reason` (string)

### 2. **Purchase Order Approval**

- **Steps**:
  1. **Submit Purchase Request**: Employee submits a purchase order.
  2. **Supervisor Approval**: First-level approval.
  3. **Department Head Approval**: Second-level approval if `totalCost > 1000`.
  4. **Finance Manager Approval**: Third-level approval if `totalCost > 5000`.
  5. **Escalation**: Escalate to Finance Director if approvals are delayed.
  6. **Notification**: Notify requestor and approvers at each step.

- **Dynamic Fields**:
  - `itemName` (string)
  - `quantity` (number)
  - `unitPrice` (number)
  - `totalCost` (number, calculated)

### 3. **Document Renewal Workflow**

- **Steps**:
  1. **Submit Renewal Request**: User submits a document renewal request.
  2. **Validation**: System validates the `expiryDate` and other criteria.
  3. **Approval**: Auto-approval if validation passes.
  4. **Document Generation**: Generate new document via Document Generation Microservice.
  5. **Notification**: Notify user of the new document availability.

- **Dynamic Fields**:
  - `documentType` (string)
  - `expiryDate` (date)

---

## Entities

### 1. WorkflowTemplate

Represents a workflow template created by a super admin.

**Fields**:

- `id` (UUID)
- `name` (string)
- `description` (string)
- `version` (string)
- `isActive` (boolean)
- `createdBy` (UUID)
- `createdAt` (timestamp)
- `updatedAt` (timestamp)
- `nodes` (array of `Node`)
- `edges` (array of `Edge`)
- `dynamicFields` (array of `DynamicField`)
- `permissions` (object)

### 2. Node

Represents a step or action in a workflow.

**Fields**:

- `id` (string)
- `type` (enum): `start`, `task`, `decision`, `approval`, `event`, `subWorkflow`, `end`
- `name` (string)
- `parameters` (object): Configuration specific to the node type
- `next` (array of strings): IDs of subsequent nodes

### 3. Edge

Defines the transition between nodes.

**Fields**:

- `fromNode` (string): ID of the starting node
- `toNode` (string): ID of the ending node
- `condition` (string): Expression determining if the transition should occur

### 4. DynamicField

Defines a field used within the workflow.

**Fields**:

- `name` (string)
- `type` (enum): `string`, `number`, `date`, `boolean`, `select`, `multiSelect`
- `validation` (object): Validation rules (e.g., required, pattern)
- `defaultValue` (any)
- `options` (array): For select fields

### 5. ApprovalRule

Defines approval logic within an approval node.

**Fields**:

- `approvalType` (enum): `SA`, `MA`, `EA`, `CA`, `PA`, `DA`, `MLA`, `AA`, `TBA`, `BA`
- `approvers` (array): User IDs or role IDs
- `conditions` (object): Conditions for approval
- `escalationPolicy` (object): Details for escalation
- `delegationPolicy` (object): Details for delegation
- `timeframe` (object): Time-based settings

---

## API Endpoints

### 1. **Create Workflow Template**

- **Endpoint**: `POST /workflow-templates`
- **Description**: Creates a new workflow template.

**Request Body**:

```json
{
  "name": "Leave Request Workflow",
  "description": "Workflow for approving leave requests",
  "version": "v1.0",
  "nodes": [
    {
      "id": "start",
      "type": "start",
      "next": ["submitRequest"]
    },
    {
      "id": "submitRequest",
      "type": "task",
      "name": "Submit Leave Request",
      "parameters": {
        "action": "collectData",
        "fields": ["employeeId", "leaveType", "startDate", "endDate", "reason"]
      },
      "next": ["managerApproval"]
    },
    {
      "id": "managerApproval",
      "type": "approval",
      "name": "Manager Approval",
      "parameters": {
        "approvalRule": {
          "approvalType": "SA",
          "approvers": ["manager-role-id"]
        }
      },
      "next": ["hrApproval"]
    },
    {
      "id": "hrApproval",
      "type": "approval",
      "name": "HR Approval",
      "parameters": {
        "approvalRule": {
          "approvalType": "CA",
          "conditions": {
            "leaveDays": ">5"
          },
          "approvers": ["hr-role-id"]
        }
      },
      "next": ["end"]
    },
    {
      "id": "end",
      "type": "end"
    }
  ],
  "edges": [],
  "dynamicFields": [
    {
      "name": "employeeId",
      "type": "string",
      "validation": {"required": true}
    },
    {
      "name": "leaveType",
      "type": "select",
      "options": ["Annual Leave", "Sick Leave", "Maternity Leave"],
      "validation": {"required": true}
    },
    {
      "name": "startDate",
      "type": "date",
      "validation": {"required": true}
    },
    {
      "name": "endDate",
      "type": "date",
      "validation": {"required": true}
    },
    {
      "name": "reason",
      "type": "string",
      "validation": {"required": false, "maxLength": 500}
    }
  ],
  "permissions": {
    "canExecute": ["employee-role-id"],
    "canManage": ["super-admin-role-id"]
  }
}
```

**Response**:

```json
{
  "id": "workflow-template-uuid",
  "name": "Leave Request Workflow",
  "version": "v1.0",
  "createdBy": "admin-uuid",
  "createdAt": "2023-10-01T12:00:00Z",
  "updatedAt": "2023-10-01T12:00:00Z",
  "isActive": false
}
```

### 2. **Validate Workflow Template**

- **Endpoint**: `POST /workflow-templates/validate`
- **Description**: Validates the workflow template for correctness.

**Request Body**:

```json
{
  "nodes": [...],
  "edges": [...],
  "dynamicFields": [...]
}
```

**Response**:

```json
{
  "isValid": true,
  "errors": []
}
```

### 3. **Activate Workflow Template**

- **Endpoint**: `POST /workflow-templates/{id}/activate`
- **Description**: Activates a workflow template for use.

**Response**:

```json
{
  "id": "workflow-template-uuid",
  "isActive": true,
  "updatedAt": "2023-10-01T13:00:00Z"
}
```

### 4. **Execute Workflow**

- **Endpoint**: `POST /workflows/execute`
- **Description**: Starts a new workflow instance based on a template.

**Request Body**:

```json
{
  "templateId": "workflow-template-uuid",
  "inputData": {
    "employeeId": "emp-123",
    "leaveType": "Annual Leave",
    "startDate": "2023-12-01",
    "endDate": "2023-12-10",
    "reason": "Family vacation"
  }
}
```

**Response**:

```json
{
  "workflowInstanceId": "workflow-instance-uuid",
  "status": "InProgress",
  "startedAt": "2023-10-01T14:00:00Z"
}
```

---

## Schemas

### WorkflowTemplate Schema

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "WorkflowTemplate",
  "type": "object",
  "required": ["name", "version", "nodes", "dynamicFields"],
  "properties": {
    "id": { "type": "string", "format": "uuid" },
    "name": { "type": "string" },
    "description": { "type": "string" },
    "version": { "type": "string" },
    "isActive": { "type": "boolean" },
    "createdBy": { "type": "string", "format": "uuid" },
    "createdAt": { "type": "string", "format": "date-time" },
    "updatedAt": { "type": "string", "format": "date-time" },
    "nodes": {
      "type": "array",
      "items": { "$ref": "#/definitions/Node" }
    },
    "edges": {
      "type": "array",
      "items": { "$ref": "#/definitions/Edge" }
    },
    "dynamicFields": {
      "type": "array",
      "items": { "$ref": "#/definitions/DynamicField" }
    },
    "permissions": {
      "type": "object"
    }
  },
  "definitions": {
    "Node": {
      "type": "object",
      "required": ["id", "type"],
      "properties": {
        "id": { "type": "string" },
        "type": { "type": "string" },
        "name": { "type": "string" },
        "parameters": { "type": "object" },
        "next": {
          "type": "array",
          "items": { "type": "string" }
        }
      }
    },
    "Edge": {
      "type": "object",
      "required": ["fromNode", "toNode"],
      "properties": {
        "fromNode": { "type": "string" },
        "toNode": { "type": "string" },
        "condition": { "type": "string" }
      }
    },
    "DynamicField": {
      "type": "object",
      "required": ["name", "type"],
      "properties": {
        "name": { "type": "string" },
        "type": { "type": "string" },
        "validation": { "type": "object" },
        "defaultValue": {},
        "options": {
          "type": "array",
          "items": {}
        }
      }
    }
  }
}
```

---

## Additional Considerations

### Integration with Other Microservices

- **Workflow Execution Microservice**: Executes workflows based on templates defined in the Workflow Engine.
- **Document Generation Microservice**: Generates documents when workflows require it.
- **Notification Service**: Sends notifications as specified in workflow actions.
- **User Data Microservice**: Provides user and role information for approvals and notifications.

### Security and Permissions

- Only authorized super admins can create or modify workflow templates.
- Role-based access control (RBAC) ensures that users have appropriate permissions.
- Sensitive data should be encrypted and handled securely.

### Error Handling and Validation

- Workflows are validated before activation to prevent runtime errors.
- Detailed error messages are provided for invalid configurations.
- Runtime exceptions are logged and managed gracefully.

### Performance and Scalability

- The engine should support high concurrency and scalability.
- Asynchronous processing for long-running tasks.
- Caching of frequently accessed data (e.g., templates).

---

## Conclusion

The refactored **Workflow Engine Microservice** offers a dynamic and flexible platform for super administrators to create customized workflows that can handle a wide range of business processes. By incorporating modular components, dynamic parameters, and extensible logic, the system can adapt to various organizational needs, enhancing automation and efficiency.

