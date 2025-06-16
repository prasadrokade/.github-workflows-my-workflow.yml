openapi: 3.0.3
info:
  title: StrideGPT API
  description: API for interacting with StrideGPT – a conversational AI service.
  version: 1.0.0
servers:
  - url: https://api.stridegpt.com/api/v1
    description: Production server

paths:
  /chat:
    post:
      summary: Send a prompt to StrideGPT
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                prompt:
                  type: string
                  description: The user prompt to send to StrideGPT
                session_id:
                  type: string
                  description: Optional session ID for context continuation
              required:
                - prompt
      responses:
        '200':
          description: A successful response with the AI's reply
          content:
            application/json:
              schema:
                type: object
                properties:
                  reply:
                    type: string
                    description: AI-generated response
                  session_id:
                    type: string
                    description: Session ID for context continuation
        '400':
          description: Invalid input
        '500':
          description: Server error

  /sessions:
    get:
      summary: Retrieve a list of active sessions
      responses:
        '200':
          description: List of sessions
          content:
            application/json:
              schema:
                type: array
                items:
                  type: object
                  properties:
                    session_id:
                      type: string
                    created_at:
                      type: string
                      format: date-time

    delete:
      summary: Delete all sessions
      responses:
        '204':
          description: Sessions deleted

  /sessions/{session_id}:
    delete:
      summary: Delete a specific session
      parameters:
        - name: session_id
          in: path
          required: true
          schema:
            type: string
      responses:
        '204':
          description: Session deleted
        '404':
          description: Session not found

components:
  securitySchemes:
    ApiKeyAuth:
      type: apiKey
      in: header
      name: X-API-Key

security:
  - ApiKeyAuth: []
