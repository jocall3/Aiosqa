openapi: 3.0.4
info:
  title: AI-as-OS Concepts API
  version: "1.0.0"
  description: |
    This API provides access to a catalog of 70 AI-as-OS (or No-OS) concepts.
    Each concept outlines a fundamental aspect of an AI-driven operating system,
    along with guiding questions for design/implementation and a broad palette
    of suggested programming languages and targets.

    The intent is to convert these concepts into a structured format for
    broader programmatic access and understanding, demonstrating their applicability
    across various programming paradigms, from high-level languages to
    low-level assembly, bytecode, and hardware description languages.

    ---

    Original context from the source document:

    "You want the 70 AI-as-OS concepts converted into a table where each concept includes four guiding questions (the original question plus three new, deeper/design/programming questions) and a list of programming / implementation languages and targets that could be used to build or prototype that concept. You specifically want to include COBOL (for business-backend logic), assembly and bytecode-level options, pixel/shader languages (for 'pixel frame / hi-code'), and many other languages so the table proves the idea to the broader 'type world' (programming community)."

    "Each row has (1) ID, (2) Concept, (3) Four concise guiding questions to answer (Q1..Q4), and (4) Suggested languages/targets (a broad multilingual palette — high-level, backend, systems, bytecode, assembly, shader, hardware description). I kept each question short and actionable so you can use them as design prompts or to assign implementation tasks."
servers:
  - url: https://api.example.com/ai-os
    description: AI-as-OS Concepts API Server
tags:
  - name: Concepts
    description: AI-as-OS foundational concepts

paths:
  /concepts:
    get:
      summary: Retrieve all AI-as-OS concepts
      operationId: getAllConcepts
      tags:
        - Concepts
      responses:
        "200":
          description: A list of AI-as-OS concepts.
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Concept'
        "500":
          $ref: '#/components/responses/InternalServerError'

  /concepts/{conceptId}:
    get:
      summary: Retrieve a specific AI-as-OS concept by ID
      operationId: getConceptById
      tags:
        - Concepts
      parameters:
        - in: path
          name: conceptId
          schema:
            type: integer
            format: int32
            minimum: 1
            maximum: 70
          required: true
          description: Numeric ID of the concept to retrieve (1-70).
      responses:
        "200":
          description: Details of the requested AI-as-OS concept.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Concept'
        "404":
          $ref: '#/components/responses/NotFound'
        "500":
          $ref: '#/components/responses/InternalServerError'

components:
  schemas:
    Concept:
      type: object
      required:
        - id
        - name
        - guidingQuestions
        - suggestedLanguages
      properties:
        id:
          type: integer
          format: int32
          description: Unique identifier for the concept.
          example: 1
        name:
          type: string
          description: The name of the AI-as-OS concept.
          example: Kernelless resource arbitration
        guidingQuestions:
          type: array
          description: Four concise guiding questions for design, implementation, and testing of the concept.
          items:
            type: string
          example:
            - "Q1: How can AI arbitrate CPU/GPU cycles without a central scheduler?"
            - "Q2: How to express arbitration policies as learnable reward signals?"
            - "Q3: How to measure fairness across tenants?"
            - "Q4: How to trace decisions to source inputs?"
        suggestedLanguages:
          type: array
          description: A broad multilingual palette of programming languages and targets suitable for building or prototyping the concept. Includes high-level, backend, systems, bytecode, assembly, shader, and hardware description languages.
          items:
            type: string
          example:
            - C
            - Rust
            - C++
            - Go
            - Assembly (x86/ARM)
            - WebAssembly
            - RISC-V asm
            - Java
            - C#
            - Python (prototyping)
            - eBPF
            - VHDL/Verilog
    Error:
      type: object
      properties:
        code:
          type: string
          example: "NOT_FOUND"
        message:
          type: string
          example: "Concept with ID 99 not found."
  responses:
    NotFound:
      description: The specified resource was not found.
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
            example:
              code: "NOT_FOUND"
              message: "The concept with the given ID was not found."
    InternalServerError:
      description: An unexpected error occurred on the server.
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
            example:
              code: "SERVER_ERROR"
              message: "An internal server error occurred."