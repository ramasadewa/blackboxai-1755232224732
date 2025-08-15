```markdown
# Detailed Implementation Plan for G.P.S (Gerakan Pemuda Simpur)

This plan outlines all changes and new files required to build a Next.js application that manages village data. The app will include login, signup, KYC verification, and a dashboard with multiple modules. The free alternatives will be used: Supabase for authentication and database integration, local storage for demo data, and simulated (mock) file uploads with preview.

---

## 1. Supabase Client Setup

**File:** `/src/lib/supabaseClient.ts`  
**Changes/Steps:**
- Import `{ createClient }` from `@supabase/supabase-js`.
- Read environment variables `NEXT_PUBLIC_SUPABASE_URL` and `NEXT_PUBLIC_SUPABASE_ANON_KEY`.
- Throw an error if either is missing.
- Export the initialized Supabase client for use in authentication and user operations.
- Wrap client initialization in try/catch for error handling.

---

## 2. Authentication Pages

### 2.1. Login Page

**File:** `/src/app/login/page.tsx`  
**Changes/Steps:**
- Create a modern login form using React and `react-hook-form` with controlled inputs for email and password.
- On submit, call Supabase’s `auth.signInWithPassword` via the client from `/src/lib/supabaseClient.ts`.
- Show error messages when sign-in fails.
- Include navigation links (text-only, styled with typography & spacing) to the Signup and KYC pages.
- Use semantic HTML and accessible form labels.

### 2.2. Signup Page

**File:** `/src/app/signup/page.tsx`  
**Changes/Steps:**
- Build a registration form with fields: email, password, and confirm password.
- Validate that password and confirmation match.
- On submit, call Supabase’s `auth.signUp` method.
- On successful signup, automatically redirect to the KYC page.
- Display error messages for invalid input or API errors.

### 2.3. KYC Verification Page

**File:** `/src/app/kyc/page.tsx`  
**Changes/Steps:**
- Create a form with fields for full name, ID number, and an `<input type="file">` for document upload.
- Use React state to capture and preview the selected file (using `URL.createObjectURL`).
- Validate that a file is selected; check file type/size if needed.
- Simulate file upload with a mock function that resolves with a success message.
- Provide clear error messages if no file is selected or if an error occurs during the mock upload.
- On success, redirect the user to the dashboard.

---

## 3. Dashboard & Protected Routes

### 3.1. Dashboard Layout

**File:** `/src/app/dashboard/layout.tsx`  
**Changes/Steps:**
- Create a layout wrapper to be used by all dashboard subpages.
- Implement a sidebar (either inline in this file or via a new component `/src/components/Sidebar.tsx`) with text-based links to:
  - Informasi Desa
  - Dokumen (for KTP, KK, Akta, Surat Jalan)
  - Literasi Digital & Pertanian
  - Pengembangan Masyarakat Desa
  - E-commerce Produk Lokal
- Include a header displaying the "G.P.S" brand.
- Add logic to check user authentication (using Supabase’s session or localStorage for demo) and redirect to `/login` if not authenticated.
- Handle any errors during authentication check gracefully.

### 3.2. Dashboard Home

**File:** `/src/app/dashboard/page.tsx`  
**Changes/Steps:**
- Display a welcome message and an overview of available modules.
- Provide clear calls-to-action and shortcuts to the individual modules.
- Utilize clean typography and spacing; no icons will be used, only text links.

---

## 4. Dashboard Subpages

### 4.1. Informasi Desa

**File:** `/src/app/dashboard/informasi-desa/page.tsx`  
**Changes/Steps:**
- Present static/dummy data that summarizes "dana desa" and "kegiatan desa".
- Use card components (e.g., from `/src/components/ui/card.tsx`) styled with modern CSS for layout.
- Include error boundaries if data fetching is implemented later.

### 4.2. Dokumen (Pembuatan KTP, KK, Akta, Surat Jalan)

**File:** `/src/app/dashboard/dokumen/page.tsx`  
**Changes/Steps:**
- Create a page with clearly divided sections or buttons for each document type.
- Each button should trigger a modal or navigate to a dedicated form (if further detail is required) that simulates document creation.
- Validate user inputs and catch errors (e.g., missing fields).
- Use clear text labels and modern button styling from `/src/components/ui/button.tsx`.

### 4.3. Literasi Digital dan Pertanian

**File:** `/src/app/dashboard/literasi/page.tsx`  
**Changes/Steps:**
- Develop a page that displays information, guides, or resources on digital literacy and agriculture.
- Use responsive sections with headings, paragraphs, and optionally card layouts.
- Ensure the layout remains clean and modern with ample spacing.

### 4.4. Pengembangan Masyarakat Desa

**File:** `/src/app/dashboard/pengembangan/page.tsx`  
**Changes/Steps:**
- Provide content outlining community development projects.
- Display the projects in either a list or a grid layout.
- Include descriptive text and ensure accessibility with proper heading hierarchy.

### 4.5. E-commerce Produk Lokal

**File:** `/src/app/dashboard/e-commerce/page.tsx`  
**Changes/Steps:**
- Build a product catalog page listing local products.
- Each product card should include:
  - A placeholder image using an `<img>` tag with:
    - `src="https://placehold.co/400x300?text=Local+Product+Image"`
    - A detailed `alt` text, for example: `alt="High+quality+local+product+image+showcasing+the+product+in+a+well-lit+studio+environment"`.
    - Add an `onerror` handler to replace the image with a fallback.
  - Product name and brief description.
- Lay out the products in a responsive grid with consistent spacing.
- Validate that each product card loads correctly and preserve the layout even if an image fails to load.

---

## 5. Global Style Updates

**File:** `/src/app/globals.css`  
**Changes/Steps:**
- Add global CSS rules for typography, colors, and spacing to achieve a modern, clean look.
- Define classes for headers, sidebars, buttons, and cards used in the various pages.
- Ensure responsive design using media queries where necessary.
- Maintain a consistent color palette and font family across all pages.

---

## 6. Next.config.ts Adjustments (If Needed)

**File:** `/next.config.ts`  
**Changes/Steps:**
- Verify and, if necessary, add configuration for environment variables (`NEXT_PUBLIC_SUPABASE_URL` and `NEXT_PUBLIC_SUPABASE_ANON_KEY`).
- Ensure proper image domains are set if any external URLs are ever used (not applicable for current placeholder images).

---

## 7. Error Handling and Best Practices

- Every form submission (login, signup, KYC) must include try/catch blocks to catch API or runtime errors.
- Forms must display user-friendly error messages near the respective inputs.
- Protected routes (dashboard and its subpages) should check authentication status and redirect to `/login` on failure.
- File inputs in the KYC page will validate file selection with graceful error prompts.
- Use semantic HTML, accessibility best practices, and ensure that modern CSS provides a responsive UI that maintains layout integrity even if external images fail to load.

---

## Summary

- Created a Supabase client configuration in `/src/lib/supabaseClient.ts` for authentication.
- Developed new pages for login (`/login/page.tsx`), signup (`/signup/page.tsx`), and KYC (`/kyc/page.tsx`) utilizing `react-hook-form` and modern styling.
- Built a protected dashboard with a layout (`/dashboard/layout.tsx`) containing a sidebar with five modules.
- Implemented individual dashboard subpages for village information, document processing, digital literacy/agriculture, community development, and a local product catalog with image placeholders.
- Updated global styles in `/src/app/globals.css` to ensure a clean, modern UI.
- Incorporated error handling, form validations, and accessibility across the entire application.
