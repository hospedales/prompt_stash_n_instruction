**Core Idea:**

The prompt is designed to be your "AI pair programmer's" instruction manual. You provide it at the beginning of each new task within a project. It guides the AI in understanding your project structure, workflow, and expectations.  Think of it like onboarding a new developer to your team – you wouldn't just throw them into the code; you'd give them an orientation. This prompt is the AI's orientation.

**Steps to Use the Prompt:**

1.  **Project Setup (One-Time Only):**
    *   **Create `.notes` directory:**
        *   At the root of your project (`[PROJECT_ROOT]`), create a directory named `.notes`.
        *   Inside `.notes`, create a file named `project_overview.md`. This will serve as your project's "memory bank."
        *   Populate `project_overview.md` with essential information:
            *   **Project Description:** A brief overview of the project's purpose.
            *   **Architecture:** How the project is structured (e.g., MVC, microservices).
            *   **Dependencies:** Key libraries and frameworks used.
            *   **Build/Run Instructions:** How to build, run, and test the project.
            *   **Coding Style:** Any specific coding conventions or style guides.
            *   **Important Modules/Components:** Brief descriptions of crucial parts of the codebase.
            *   **Example:**

                ```markdown
                # Project Overview: E-commerce Platform

                ## Description
                This project is a web-based e-commerce platform built using Python, Django, and React. It allows users to browse products, add them to a cart, and complete purchases.

                ## Architecture
                - **Backend:** Django REST Framework (Python)
                - **Frontend:** React with Redux for state management
                - **Database:** PostgreSQL

                ## Dependencies
                - Django 4.x
                - djangorestframework 3.x
                - React 18.x
                - Redux 4.x
                - ...

                ## Build/Run Instructions
                1. **Backend:**
                    - `cd backend`
                    - `pip install -r requirements.txt`
                    - `python manage.py migrate`
                    - `python manage.py runserver`
                2. **Frontend:**
                    - `cd frontend`
                    - `npm install`
                    - `npm start`

                ## Coding Style
                - Python: PEP 8
                - JavaScript: Airbnb JavaScript Style Guide

                ## Important Modules/Components
                - `backend/products`: Manages product data and APIs.
                - `backend/orders`: Handles order creation and processing.
                - `frontend/src/components/ProductList`: Displays a list of products.
                - `frontend/src/components/ShoppingCart`: Manages the shopping cart.
                ```
    *   **Create `.tasks` directory:** Also at the root of your project, create a directory named `.tasks`. This will store your task files.

2.  **Starting a New Task:**
    *   **Provide the Prompt:** Begin your conversation with the AI by pasting the entire "Task Implementation Protocol" prompt. This sets the stage for the task.
    *   **Provide the Task Description:** After the prompt, clearly describe the task you want the AI to perform. Be specific and include relevant details.
    *   **Example:**

        ```
        [Paste the entire "Task Implementation Protocol" prompt here]

        ---

        Task: Implement Product Search Functionality

        Description: I need to add a search bar to the product listing page that allows users to filter products by name and description. The search should be performed in real-time as the user types, updating the product list dynamically. The search should be case-insensitive.
        ```

3.  **AI Fills in Placeholders and Task Template:**
    *   The AI will process the prompt and your task description.
    *   It will create the task file in `.tasks` (e.g., `.tasks/2024-01-17_001_product-search.md`).
    *   It will ask you for the `[TASK_NUMBER]` if it cannot determine it automatically.
    *   It will execute the `date` commands to get `[CURRENT_DATE]` and `[TIME_STAMP]`.
    *   It will create and switch to the appropriate Git branch.
    *   It will then fill out the "Task Template" within the task file, drawing information from `project_overview.md` and its understanding of the task.
    *   **Example Task File (Partially Filled):**

        ```markdown
        # Context
        Created: 2024-01-17T10:30:00Z

        ## Original Prompt
        Task: Implement Product Search Functionality

        Description: I need to add a search bar to the product listing page that allows users to filter products by name and description. The search should be performed in real-time as the user types, updating the product list dynamically. The search should be case-insensitive.

        ## Project Overview
        ### Project Description
        This project is a web-based e-commerce platform built using Python, Django, and React. It allows users to browse products, add them to a cart, and complete purchases.

        ### Architecture
        - Backend: Django REST Framework (Python)
        - Frontend: React with Redux for state management
        - Database: PostgreSQL

        ... (rest of project overview)

        ## Current Branch
        feature/product-search

        ## Task Progress Below
        ---

        -----------------------------------
        # TASK TEMPLATE
        -----------------------------------
        Task: Implement Product Search Functionality

        ## Analysis
        - Current Implementation: Currently, there is no search functionality on the product listing page. All products are displayed.
        - Relevant Code/Modules:
            - `backend/products/views.py` (likely needs modification to handle search queries)
            - `frontend/src/components/ProductList.js` (needs to be updated to include a search bar and handle filtering)
        - Root Cause: N/A - This is a new feature.
        - Impact on System: Enhanced user experience by allowing quick filtering of products.
        - Dependencies: No new dependencies are anticipated.

        ## Solution
        - Proposed Changes:
            1. Add a search input field to `ProductList.js`.
            2. Implement filtering logic in `ProductList.js` to filter products based on the search input.
            3. Modify the `ProductListView` in `views.py` to accept a search query parameter and filter the queryset accordingly.
        - Potential Risks:
            - Performance issues if the filtering is not optimized (especially with a large number of products).
            - Potential UI/UX issues with the real-time updating of the product list.
        - Expected Outcome: Users should be able to type in the search bar and see the product list update in real-time to display only products matching their search query.
        - Error Handling and Rollback:
            - If there are errors in the backend API, display an appropriate error message to the user.
            - If the frontend filtering encounters issues, fall back to displaying all products.

        ## Implementation
        [Add specific debug logs or console messages if relevant, marking them clearly for later removal.]
        - [ ] Step 0: Write tests based on the "Verification" section below.
        - [ ] Step 1: Modify `backend/products/views.py` to handle search queries.
            ```python
            # backend/products/views.py
            # ... existing code ...
            class ProductListView(generics.ListAPIView):
                # ...
                def get_queryset(self):
                    queryset = Product.objects.all()
                    search_query = self.request.query_params.get('search', None)
                    if search_query:
                        queryset = queryset.filter(
                            Q(name__icontains=search_query) | Q(description__icontains=search_query)
                        )
                        print(f"DEBUG: [TIME_STAMP] - Search query: {search_query}") # Debug log - to be removed
                    return queryset
            ```
        - [ ] Step 2: Add a search input field to `frontend/src/components/ProductList.js`.
            ```javascript
            // frontend/src/components/ProductList.js
            import React, { useState, useEffect } from 'react';
            // ... other imports ...

            const ProductList = () => {
                // ... existing code ...
                const [searchQuery, setSearchQuery] = useState('');

                const handleSearchChange = (event) => {
                    setSearchQuery(event.target.value);
                    console.log(`DEBUG: [TIME_STAMP] - Search query changed: ${event.target.value}`); // Debug log - to be removed
                };

                useEffect(() => {
                    fetchProducts(`/api/products/?search=${searchQuery}`);
                }, [searchQuery]);

                return (
                    <div>
                        <input type="text" placeholder="Search products..." value={searchQuery} onChange={handleSearchChange} />
                        {/* ... rest of the component to display products ... */}
                    </div>
                );
            };
            ```
        - [ ] Step 3: Implement filtering logic in `frontend/src/components/ProductList.js`.
        - [ ] Additional Steps...
        - [ ] Clean up: Remove debug messages:
            - Remove `print(f"DEBUG: [TIME_STAMP] - Search query: {search_query}")` from `backend/products/views.py`
            - Remove `console.log(`DEBUG: [TIME_STAMP] - Search query changed: ${event.target.value}`);` from `frontend/src/components/ProductList.js`
        - [ ] Commit: `feat(products): add real-time search to product list`

        ## Verification (Test-Driven Development)
        - [ ] Step 1: Define or update tests to outline success criteria.
            - Write tests in `backend/products/tests.py` to ensure that the API correctly filters products based on the search query.
            - Write tests in `frontend/src/components/ProductList.test.js` to ensure that the search input updates the product list as expected.
        - [ ] Step 2: Ensure the implementation passes all tests.
        - [ ] Step 3: Perform additional manual checks (browser, CLI, logs, etc.).
            - Manually test the search functionality in the browser to ensure it works as expected.
        - [ ] Step 4: Validate end-to-end scenarios.
            - Test various search queries, including empty queries, queries with no matches, and queries with multiple matches.
        - [ ] Step 5: Confirm no regressions have been introduced.

        ## Documentation
        - Implementation Notes: The search is performed using a case-insensitive `icontains` lookup in the backend and by dynamically updating the API query in the frontend.
        - TDD Observations: Tests were written to cover various search scenarios, including edge cases. The TDD approach ensured that the implementation meets the requirements.
        - Updated/Created Documentation:
          - Updated `/backend/products/README.md` to include information about the new search functionality in the API.
          - Updated `/frontend/src/components/README.md` to include information about the new search bar in the `ProductList` component.

        ## Status
        - Current Status: Template Filled, Awaiting User Confirmation
        - Next Action: Wait for user review and confirmation to proceed with implementation.
        - Blockers (if any): None at this time.
        ```

4.  **User Review and Confirmation:**
    *   **Carefully review the AI-generated task file and filled-out template.** Pay close attention to:
        *   **Analysis:** Does the AI understand the current state and the impact of the task?
        *   **Solution:** Is the proposed solution correct and efficient? Are there any potential risks or better alternatives?
        *   **Implementation:** Are the steps clear and logical? Are there any missing steps?
        *   **Verification:** Are the testing steps comprehensive?
        *   **Documentation:** Are the documentation updates sufficient?
    *   **Provide Feedback or Confirmation:**
        *   If you are satisfied, tell the AI to proceed with implementation.
        *   If you have corrections or suggestions, provide them to the AI. The AI will update the task file accordingly.

5.  **Iterative Implementation:**
    *   The AI will work through the implementation steps, providing code, explanations, and asking clarifying questions as needed.
    *   Continue to review the AI's work and provide feedback.
    *   The AI will commit changes to the feature branch as it progresses.

**Example Scenarios:**

*   **Scenario 1: New Python/Django Project:**
    *   `project_overview.md` would describe your Django project structure, models, views, templates, etc.
    *   Tasks might involve creating new models, views, API endpoints, or templates.
*   **Scenario 2: Existing React/Node.js Project:**
    *   `project_overview.md` would describe your React components, state management (e.g., Redux, Context API), API interactions, Node.js backend setup, etc.
    *   Tasks might involve creating new components, updating existing ones, adding API routes, or fixing bugs.
*   **Scenario 3: Data Science Project (Python/Jupyter):**
    *   `project_overview.md` would describe your datasets, data cleaning steps, feature engineering, model training process, evaluation metrics, etc.
    *   Tasks might involve exploring new features, trying different models, optimizing hyperparameters, or generating visualizations.
*   **Scenario 4: DevOps/Infrastructure Project (Terraform/Ansible):**
    *   `project_overview.md` would describe your infrastructure setup, cloud provider, Terraform modules, Ansible playbooks, etc.
    *   Tasks might involve provisioning new resources, updating existing infrastructure, automating deployment processes, or configuring monitoring.

**Key Benefits:**

*   **Consistency:** Ensures a consistent approach to task management across all projects.
*   **Context Preservation:** The `.notes` and `.tasks` directories provide a valuable history and context for the AI.
*   **Reduced Errors:** The structured approach and user confirmation steps help prevent misunderstandings and errors.
*   **Improved Collaboration:** Facilitates a more effective collaboration between you and the AI.
*   **Onboarding New Projects:** This approach will help you onboard new projects into this workflow with ease.

**Important Considerations:**

*   **Security:** If you are allowing the AI to execute code (e.g., Git commands), be extremely cautious about security. Implement robust sandboxing and other security measures.
*   **Iteration:** Prompt engineering is an iterative process. You will likely need to refine the prompt and your instructions based on the AI's responses and your specific needs.
*   **Experimentation:** Try different variations of the prompt and task descriptions to see what works best for your workflow.

By following these instructions and adapting them to your specific projects, you can effectively use the "Task Implementation Protocol" prompt to enhance your development process with AI assistance. Remember that clear communication and careful review are crucial for a successful collaboration with your AI pair programmer.
