# Stackline Full Stack Assignment

## Overview

This is a sample eCommerce website that includes:
- Product List Page
- Search Results Page
- Product Detail Page

The application contains various bugs including UX issues, design problems, functionality bugs, and potential security vulnerabilities.

## Getting Started

```bash
yarn install
yarn dev
```

## Your Task

1. **Identify and fix bugs** - Review the application thoroughly and fix any issues you find
2. **Document your work** - Create a comprehensive README that includes:
   - What bugs/issues you identified
   - How you fixed each issue
   - Why you chose your approach
   - Any improvements or enhancements you made

We recommend spending no more than 2 hours on this assignment. We are more interested in the quality of your work and your communication than the amount of time you spend or how many bugs you fix!

## Submission

- Fork this repository
- Make your fixes and improvements
- **Replace this README** with your own that clearly documents all changes and your reasoning
- Provide your Stackline contact with a link to a git repository where you have committed your changes

We're looking for clear communication about your problem-solving process as much as the technical fixes themselves.

## My Changes
### 1. **Search Function Was Too Strict**
**Issue:**  
The original search logic matched only exact strings. Searching for `"giftcard"` failed while `"gift card"` worked because the logic used simple `.includes()` matching.

**Fix:**  
Integrated **Fuse.js**, a lightweight fuzzy-search library that supports partial, typo-tolerant, and weighted matching.

**Why This Approach:**
- Enables fuzzy matching and relevance ranking.
- Handles typos and variations (e.g., "gif card" still finds "gift card").
- No backend dependency — improves responsiveness.
- Scales easily with additional searchable fields in the future.

### 2. **Clear Filters Did Not Fully Reset UI**
**Issue:**
Clicking Clear Filters reset some state values but the dropdowns still showed the previously selected options.
Re-selecting the same category afterward did not trigger a re-render or update.

**Fix:**
Introduced a filterResetKey that forces controlled `<Select>` components to remount when filters are cleared.
Also used `router.replace("/")` to remove stale query parameters from the URL.

**Why This Approach:**
- key property forces a new component render, fully resetting dropdowns.
- Prevents stale selections from persisting across state resets.
- Clears URL parameters to maintain consistent UI and logic state.
- Guarantees predictable and intuitive user experience.

### 3. **Subcategory Dropdown Displayed All Subcategories**
**Issue:**
When a user selected a category, the subcategory dropdown displayed every subcategory in the database — not just those related to the selected category.

**Fix:**
Updated the fetch logic to request subcategories filtered by category from the API.

**Why This Approach:**
- Reduces noise by only showing relevant subcategories.
- Prevents confusion and accidental selection of unrelated filters.
- Optimizes API response payloads and improves load times.
- Increases overall clarity and contextual accuracy for users.

### 4. **Filters Lost When Navigating Between Pages**
**Issue:**
When clicking “View Details” on a product and then returning to the main page, previously applied filters and search queries were lost.
This forced users to reapply filters each time, creating poor navigation continuity.

**Fix:**
Passed filter and search values via URL query parameters when navigating to the product page, then restored them on back navigation.

**Why This Approach:**
- Keeps user context intact during navigation.
- URL query params make state reload-safe and shareable.
- Provides smooth back-navigation that restores prior filters instantly.
- Improves UX and session continuity significantly.

### 5. **Filters Lost After Page Reload**
**Issue:**
Refreshing the page reset all filters and search inputs, showing all products again.

**Fix:**
Initialized filter and search states from the URL using useSearchParams() to sync state with query parameters.

**Why This Approach:**
- Ensures filter state persistence on reloads or direct links.
- Makes the app URL-driven, improving consistency and SEO.
- Allows users to bookmark or share filtered views easily.

### 6. **Filters Persisted Even After Clearing and Reloading**
**Issue:**
After clicking Clear Filters, reloading the page re-applied previous filters because query parameters still existed in the URL.

**Fix:**
Used router.replace("/") to clear all query parameters after filters are reset.

**Why This Approach:**
- Keeps both the UI state and browser URL in sync.
- Prevents old filter parameters from being restored on reload.
- Creates a true “reset” behavior that aligns with user expectations.

### 7. **Image Loading Error (Next.js Unconfigured Host)**
**Issue:**
When loading product images from external sources (like Amazon URLs), Next.js threw an error:
Invalid src prop (https://m.media-amazon.com/...) on next/image, hostname "m.media-amazon.com" is not configured under images in your next.config.js.

**Fix:**
Configured the external domains in next.config.ts to explicitly allow Amazon image hosts.

**Why This Approach:**
- Required by Next.js next/image for security and optimization.
- Allows images to load from trusted Amazon domains without errors.
- Ensures consistent product rendering across all environments.
- Keeps Next.js’s image optimization intact while extending external sources.
