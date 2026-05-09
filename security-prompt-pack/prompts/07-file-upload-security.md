# Security Prompt Pack — Section 07
## File Upload Security

**Vulnerability:** AI builds file upload features without security checks. Users can upload PHP scripts disguised as images, HTML files with malicious JavaScript, or 5GB files that crash your server.

**VentureMind risk:** MarkItDown document ingestion, founder document uploads, KYC file submissions.

---

## PROMPT 1 — Audit all file upload endpoints

```
Review every file upload feature in this app. For each one, tell me:
(1) What file types are allowed?
(2) Is the file type checked by extension only, or by actually reading
    the file's content/MIME type?
(3) Is there a file size limit? What is it?
(4) Where are uploaded files stored —
    in a public directory, a private directory, or cloud storage?
(5) Are filenames sanitised to prevent path traversal attacks?
List every upload endpoint and what's missing.
```

**Expected output:** All upload endpoints catalogued. Missing checks = red.

---

## PROMPT 2 — Add proper file validation

```
For every file upload in this app, add these checks:
(1) Validate the file's actual content type by reading the file header
    (magic bytes), not just the extension
(2) Set a maximum file size (5MB for images, 25MB for documents)
(3) Only allow specific, expected file types (e.g., jpg, png, pdf, docx)
(4) Reject any file with a double extension (like file.php.jpg)
(5) Strip all metadata from uploaded images using a library like sharp
(6) Generate a new random filename for every upload — never use the original filename
```

**Expected output:** Magic byte validation. Random filenames. Metadata stripped.

---

## PROMPT 3 — Secure file storage

```
Review where uploaded files are stored. Make sure:
(1) Files are stored outside the web root —
    they should never be directly accessible via URL
    without going through your application
(2) Use cloud storage (like S3 or Supabase Storage)
    with proper access controls rather than local disk
(3) Serve files through a proxy endpoint that checks
    if the requesting user has permission to view that file
(4) Set the Content-Disposition header to 'attachment'
    for file downloads so browsers don't try to execute them
```

**Expected output:** Files outside web root. Cloud storage with access controls.

---

## PROMPT 4 — Scan uploads for malicious content

```
Add a check that scans uploaded files for malicious content before storing them.
For images: re-encode the image using a library
(sharp for Node.js or Pillow for Python) — this strips any embedded scripts or metadata.
For documents: check for embedded macros, scripts, or suspicious content.
For any file: verify the content type matches the extension,
reject files with executable content,
and log any suspicious upload attempts with the user's ID and IP address.
```

**Expected output:** Re-encoding active. Suspicious uploads logged and rejected.

---

## PROMPT 5 — Prevent denial-of-service via uploads

```
Add protections against upload-based denial-of-service:
(1) Set a maximum file size at the server/middleware level
    (not just the frontend)
(2) Limit the number of files a user can upload per minute
(3) Limit total storage per user if applicable
(4) Process uploads asynchronously so a large file doesn't block your server
(5) Add timeout limits on upload requests
Show me where each limit is enforced.
```

**Expected output:** DoS protections at middleware level. Async processing enabled.

---

## Test yourself

Try uploading a `.txt` file renamed to `.jpg`.
If your app accepts it = your validation is extension-only.
Real validation reads the file's actual content.