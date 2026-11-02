# AWS Internship — Task: Static Website Hosting on Amazon S3

**Date:** 2 November 2026  
**Task:** Static Website Hosting on S3 (React, HTML)  
**AWS Region:** Asia Pacific (Mumbai) — `ap-south-1`

## 1. Task Objective

Host a React-based portfolio website on Amazon S3 using Static Website Hosting.

The objective was to understand how static website files can be deployed and accessed publicly through an S3 website endpoint without managing an EC2 server.

## 2. Technologies and Services Used

- Amazon S3
- React.js
- HTML, CSS, JavaScript
- Vite
- AWS Bucket Policy
- Static Website Hosting

## 3. Practical Implementation

### Step 1: Prepare the React project

- Opened the portfolio project in VS Code.
- Installed project dependencies using `npm install`.
- Generated the production build using:

```bash
npm run build
```

- Vite generated the production-ready files inside the `dist/` directory.
- The `dist` directory contained `index.html`, CSS, JavaScript, images, and other assets.

### Step 2: Create an S3 bucket

Created the following bucket:

`ishika-portfolio-practice-2026`

Configuration:
- AWS Region: Mumbai (`ap-south-1`)
- Bucket type: General Purpose
- Object Ownership: Bucket owner enforced
- Versioning: Disabled
- Default encryption: SSE-S3
- Block Public Access: Initially enabled

### Step 3: Upload website files

- Uploaded the contents of the `dist/` folder to the bucket root.
- Uploaded 154 files.
- Total uploaded size: 12.2 MB.
- Upload result: 154 succeeded, 0 failed.

### Step 4: Enable Static Website Hosting

Navigated to:

S3 → Bucket → Properties → Static Website Hosting → Edit

Configuration:
- Enabled Static Website Hosting.
- Selected Host a static website.
- Index document: `index.html`.

### Step 5: Configure public access

- Updated the bucket-level Block Public Access setting.
- Added a bucket policy granting public read-only access through `s3:GetObject`.

The policy allowed visitors to view website files but did not grant public upload or delete permissions.

### Step 6: Test the website

- Opened the S3 Bucket Website Endpoint in a browser.
- Initially, the website displayed a blank white page.
- Browser Developer Tools showed 404 errors for CSS and JavaScript files.

## 4. Error and Debugging

**Problem:** CSS and JavaScript assets returned 404 Not Found.

**Root cause:** The Vite configuration contained the GitHub Pages base path:

```ts
base: '/Ishika-Portfolio/',
```

S3 was hosting the website from the bucket root, so this path was incorrect.

**Solution:** Updated `vite.config.ts`:

```ts
export default defineConfig({
  base: '/',
  plugins: [react()],
})
```

Generated a new production build:

```bash
npm run build
```

Then uploaded the updated `index.html` and `assets/` folder to S3, replacing the old build files.

## 5. Final Result

- Portfolio successfully hosted on Amazon S3.
- Website homepage loaded successfully.
- CSS and JavaScript assets loaded correctly.
- Portfolio was accessible through the S3 static website endpoint.

## 6. Important Concepts Learned

1. What is Amazon S3?
2. What is Static Website Hosting?
3. What is a production build?
4. Why do we use `npm run build`?
5. What is the `dist/` folder?
6. What is a bucket policy?
7. What is public read access?
8. What is `s3:GetObject`?
9. What causes 404 errors for website assets?
10. Why must the Vite base path match the hosting environment?

## 7. Security and Billing Notes

- Public website access means bucket objects can be accessed by visitors.
- The bucket policy grants read-only access, not public write access.
- S3 static website endpoints use HTTP, not HTTPS.
- HTTPS can be configured using Amazon CloudFront.
- AWS Billing showed an estimated November 2026 bill of USD 0.00 at the time of checking.
- Billing estimates may be delayed and can change with usage.

**Task Status: Successfully Completed**

**Practical Outcome:** React Portfolio Website Hosted on Amazon S3.
