```markdown
# Detailed Implementation Plan for Village Data Monitoring Dashboard

## Overview
This plan outlines the implementation of a Next.js–based dashboard prototype for monitoring the identification and reporting of village basic data needs. The system will enable village officials and stakeholders to input data, validate entries, visualize key metrics, and generate reports. External integrations will be handled via Supabase for both database and authentication functionality.

## Environment Setup
1. **.env.local**  
   - Create a new file (not checked into VCS) with:
     - `NEXT_PUBLIC_SUPABASE_URL="your-supabase-url"`
     - `NEXT_PUBLIC_SUPABASE_ANON_KEY="your-supabase-anon-key"`
2. **package.json**  
   - Add dependencies:
     - `"@supabase/supabase-js"` for Supabase integration.
     - `"next-auth"` if additional auth flows are desired.
     - `"react-hook-form"` for form state management.
     - `"jspdf"` if PDF report generation is needed.
   - Confirm scripts for development and production are intact.

## File Modifications and New File Additions

### 1. next.config.ts
- **Changes:**
  - Ensure environment variables are correctly loaded.
  - No major changes apart from verifying Next.js configuration supports our API routes and app directory.

### 2. src/app/globals.css
- **Changes:**
  - Add custom global styles emphasizing modern typography, consistent spacing, and a responsive grid or flex layout.
  - Define CSS classes for headers, sidebars, and content sections to be reused in dashboard pages.

### 3. src/app/dashboard/page.tsx
- **New File:** Create `src/app/dashboard/page.tsx`
- **Changes:**
  - Import and render UI components: a header (or navbar), sidebar (reuse/update `src/components/ui/sidebar.tsx`), and main content area.
  - Include the Data Input Form and Data Chart components.
  - Use CSS Grid/Flexbox to establish a two-column layout (sidebar and main content).
  - Implement error boundaries for async API calls.

### 4. src/app/auth/page.tsx
- **New File:** Create `src/app/auth/page.tsx`
- **Changes:**
  - Build a login form for village officials using email and password fields.
  - Integrate with Supabase Auth for user authentication.
  - Implement form validation and show error messages as needed.

### 5. src/components/DataInputForm.tsx
- **New File:** Create `src/components/DataInputForm.tsx`
- **Changes:**
  - Build a form component using `react-hook-form` with inputs for:
    - Village Name (text)
    - Village Head (text)
    - Population (number)
    - Infrastructure Needs (textarea or select list)
    - Additional Notes (optional textarea)
  - Integrate client-side validation (required fields, proper formats).
  - On submission, POST data via fetch to `/api/data`; include a loading state and error handling (try–catch, notifications).

### 6. src/components/DataChart.tsx
- **New File:** Create `src/components/DataChart.tsx`
- **Changes:**
  - Develop a data visualization component using either a custom chart or an already-created `src/components/ui/chart.tsx` as a base.
  - Fetch data from the `/api/data` endpoint to render charts (bar chart or line chart showing key metrics).
  - Ensure the chart is responsive and styled with modern typography and spacing.

### 7. src/app/api/data/route.ts
- **New File:** Create `src/app/api/data/route.ts`
- **Changes:**
  - Implement an API endpoint supporting:
    - **POST**: Receive data from the input form and store into a Supabase table (e.g., “village_data”).
    - **GET**: Retrieve stored data for the dashboard.
  - Use try–catch blocks for error handling and send appropriate HTTP status codes and error messages.

### 8. src/app/api/report/route.ts
- **New File:** Create `src/app/api/report/route.ts`
- **Changes:**
  - Create an API endpoint to generate aggregate report data.
  - Optionally integrate `jspdf` for PDF report generation or return a JSON summary.
  - Include error handling and validation of Supabase query responses.

### 9. src/components/ui/sidebar.tsx (Optional Update)
- **Changes:**
  - Modify the sidebar component to include navigation links:
    - “Dashboard” (links to `/dashboard`)
    - “Input Data” (if separate from dashboard page)
    - “Reports” (linking to a potential report view or triggering report generation)
    - “Logout” (to sign out from Supabase Auth)
  - Use text-based navigation with modern typography; ensure sufficient spacing and clear focus states.

### 10. README.md
- **Changes:**
  - Update documentation with setup instructions:
    - Environment variable configuration.
    - Dependency installation steps (e.g., `npm install`).
    - Running the development server.
  - Outline usage instructions for dashboard features and API endpoints.

## Error Handling and Best Practices
- Every API route must use try–catch for error capturing and log errors appropriately.
- Form components will validate user input with clear, user-friendly error messages.
- Loading states and graceful fallback UI elements will be provided during asynchronous operations.
- All external API calls (Supabase) will check for null responses and network errors.

## UI/UX Considerations
- The dashboard will use a modern, minimalist design with a header, sidebar, and clean main content area.
- The input form and charts are designed with ample white space, clear labels, and responsive layouts.
- No external icon libraries will be used; design elements rely strictly on typography, spacing, and layout.
- Placeholders via `<img>` tags (if required) will follow the provided placeholder URL guidelines.

## External Integrations
- **Supabase** will serve as the external backend for both data storage and authentication.
  - API endpoints will initialize the Supabase client using environment variables.
  - User authentication is integrated on the login page.
- All interactions with Supabase will include error handling and status code checks.

---
**Summary**  
- A Next.js dashboard prototype is built with pages for authentication (`/auth`), dashboard (`/dashboard`), and API routes (`/api/data` and `/api/report`).  
- The system leverages Supabase for external database and auth integration.  
- New components (`DataInputForm.tsx`, `DataChart.tsx`) manage data input and visualization with client-side validation.  
- Modern, minimalist UI design is maintained using CSS Grid/Flexbox, clear typography, and spacing guidelines.  
- Robust error handling is implemented in both UI components and API endpoints.  
- The README is updated with setup and usage instructions.  
- This plan ensures a production-like prototype for village monitoring data needs.
