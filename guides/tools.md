# Tools
## Looker Data Store
The Looker Data Store is a collection of LookML files, Looker documentation, and best practices for building, validating, and troubleshooting LookML models and Looker dashboards. It is used to provide syntax validation, error correction, and guidance on configuring Looker models. The data store contains flattened repositories of LookML files and Looker-related documentation in plain text, making it efficient for querying and validation tasks.
## Looker Studio Data Store
The Looker Studio Data Store contains resources for optimizing and troubleshooting LSP views and dashboards, including error messages, best practices, and data connections.
## BigQuery Data Store
The BigQuery Data Store contains resources for optimizing and troubleshooting SQL queries generated by LookML, including error messages, best practices, and query optimization techniques. It serves as a reference for diagnosing SQL issues, improving query performance, and ensuring efficient resource utilization in BigQuery. The datastore is structured to facilitate quick retrieval of error logs, performance recommendations, and best practices.
## OpenAPI
The OpenAPI tool allows for interaction with external APIs, enabling users to query and retrieve data from various external services. It is designed to handle API requests, process external data queries, and serve as a fallback for general-purpose tasks that don’t fall under the scope of Looker or BigQuery-specific operations. The OpenAPI integration ensures smooth external data interactions and is capable of making requests to other services as needed. This OpenAPI specification is used to interact with the Gemini 1.5 Flash within the Looker and BigQuery ecosystem. It enables querying external services, processing data, and handling general-purpose tasks outside the scope of Looker or BigQuery-specific operations. The Gemini API integration allows for advanced AI-driven interactions, such as natural language processing, code generation, and data insights.
### Schema
```
openapi: 3.0.0
info:
  title: Gemini 1.5 Flash
  version: 1.0.0
  description: >
    This OpenAPI specification is used to interact with the Gemini 1.0 Pro API within the Looker and BigQuery ecosystem. It enables querying external services, processing data, and handling general-purpose tasks outside the scope of Looker or BigQuery-specific operations. The Gemini API integration allows for advanced AI-driven interactions, such as natural language processing, code generation, and data insights.

servers:
  - url: 'https://gemini.googleapis.com/v1/flash'
paths:
  /generate:
    post:
      summary: Perform an AI task using Gemini 1.0 Pro
      operationId: performGeminiTask
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/TaskRequest'
      responses:
        '200':
          description: AI task completed successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/TaskResponse'
        '400':
          description: Bad request, invalid parameters provided
        '429':
          description: Rate limit exceeded for the free tier (15 RPM, 32,000 TPM, 1,500 RPD)
  /status:
    get:
      summary: Retrieve the status of a task
      operationId: getTaskStatus
      parameters:
        - in: query
          name: taskId
          required: true
          schema:
            type: string
          description: The ID of the task to retrieve the status for
      responses:
        '200':
          description: Task status retrieved successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/TaskStatusResponse'
        '404':
          description: Task not found with the given ID
components:
  schemas:
    TaskRequest:
      type: object
      properties:
        input_text:
          type: string
          description: The input text or query for the Gemini model to process
        task_type:
          type: string
          description: The type of task to perform
          enum:
            - nlp
            - code_generation
            - data_analysis
        max_tokens:
          type: integer
          description: The maximum number of tokens for the response
          default: 256
    TaskResponse:
      type: object
      properties:
        result:
          type: string
          description: The result of the processed task
        tokens_used:
          type: integer
          description: Number of tokens used in the response
        task_id:
          type: string
          description: The ID of the generated task for status retrieval
    TaskStatusResponse:
      type: object
      properties:
        task_id:
          type: string
          description: The ID of the task
        status:
          type: string
          description: The current status of the task (e.g., "completed", "in_progress", "failed")
        result:
          type: string
          description: The result of the task if completed
```
