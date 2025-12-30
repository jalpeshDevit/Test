# Job Management System - Complete Workflow Documentation

## Table of Contents
1. [System Overview](#system-overview)
2. [User Roles & Responsibilities](#user-roles--responsibilities)
3. [Complete Workflow Process](#complete-workflow-process)
4. [Stage-by-Stage Detailed Flow](#stage-by-stage-detailed-flow)
5. [Status Lifecycle](#status-lifecycle)
6. [Notification Matrix](#notification-matrix)
7. [User Access Control Matrix](#user-access-control-matrix)
8. [Dashboard Views by Role](#dashboard-views-by-role)
9. [Database Schema & State Changes](#database-schema--state-changes)
10. [Business Rules & Validations](#business-rules--validations)
11. [Technical Implementation Guidelines](#technical-implementation-guidelines)

---

## 1. System Overview

### 1.1 Purpose
The Job Management System is a comprehensive web and mobile application designed to streamline the entire job lifecycle from order creation through delivery for a printing/manufacturing business.

### 1.2 Core Workflow
```
SALES → ACCOUNTS (Parallel) → DESIGN → PRODUCTION → DELIVERY
         ↓
    (Both work simultaneously after job creation)
```

### 1.3 Key Features
- Role-based access control
- Real-time notifications
- File management and version control
- Automated billing calculations
- Production stage tracking
- Comprehensive reporting
- Mobile app support

---

## 2. User Roles & Responsibilities

### 2.1 Sales User
**Primary Responsibilities:**
- Create new job orders
- Enter customer and product details
- Upload reference files and specifications
- Track job progress across all stages
- Coordinate delivery with customers
- View reports and analytics

**Access Level:** Limited to own created jobs

### 2.2 Accounts Team
**Primary Responsibilities:**
- Add billing details to new jobs
- Calculate costs (Die, Plate, Job costs)
- Generate invoices with VAT calculations
- Track payment status
- Generate billing reports
- Maintain financial records

**Access Level:** All jobs for billing purposes

### 2.3 Design / Prepress Team
**Primary Responsibilities:**
- Review job specifications
- Create and upload design files
- Manage design revisions
- Update design status (In Progress, Sent for Approval, Approved)
- Add design notes and remarks
- Submit approved designs for production

**Access Level:** Assigned design jobs only

### 2.4 Production Team
**Primary Responsibilities:**
- Manage printing operations
- Track production stages (Printing, Finishing, Packing)
- Record material consumption
- Update production status
- Log production issues
- Mark jobs as completed

**Access Level:** Production-ready jobs only

### 2.5 Admin
**Primary Responsibilities:**
- Create and manage user accounts
- Configure system settings
- Access all modules and data
- Generate comprehensive reports
- Monitor system performance
- Manage role-based permissions

**Access Level:** Full system access

---

## 3. Complete Workflow Process

### 3.1 High-Level Process Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                    JOB LIFECYCLE OVERVIEW                        │
└─────────────────────────────────────────────────────────────────┘

STAGE 1: JOB CREATION (Sales User)
    ↓
    • Create job with customer & product details
    • Upload reference files
    • Set delivery expectations
    • Submit job
    ↓
    Status: NEW
    Notifications: → Accounts Team, Design Team

STAGE 2: PARALLEL PROCESSING
    ↓
    ┌─────────────────────────┬─────────────────────────┐
    │   ACCOUNTS TEAM         │   DESIGN TEAM           │
    │   (Works First)         │   (Waits for Billing)   │
    ├─────────────────────────┼─────────────────────────┤
    │ • View new job          │ • Receives notification │
    │ • Enter billing details │ • Waits for billing     │
    │ • Calculate costs       │   completion            │
    │ • Submit billing        │                         │
    └─────────────────────────┴─────────────────────────┘
    ↓
    Status: BILLING COMPLETED → READY FOR DESIGN
    Notifications: → Design Team (ready to start), Sales User

STAGE 3: DESIGN PROCESSING
    ↓
    • Design team starts work
    • Upload design files
    • Update status: In Progress → Sent for Approval → Approved
    • Add design notes
    ↓
    Status: DESIGN APPROVED → READY FOR PRODUCTION
    Notifications: → Production Team, Sales User

STAGE 4: PRODUCTION PROCESSING
    ↓
    • Printing stage
    • Finishing stage (UV, Die Cut, etc.)
    • Packing stage
    • Record material usage
    • Log production details
    ↓
    Status: PRODUCTION COMPLETED → DELIVERY READY
    Notifications: → Sales User, Accounts Team

STAGE 5: DELIVERY & COMPLETION
    ↓
    • Sales coordinates delivery
    • Update delivery status
    • Mark as completed
    ↓
    Status: COMPLETED ✅
    Notifications: → All Stakeholders
```

---

## 4. Stage-by-Stage Detailed Flow

### STAGE 1: Job Creation by Sales User

#### Step 1.1: Login & Navigation
```
User Action:
1. Sales user logs into the system
2. Navigates to "Create New Job" or clicks "New Job" button
3. Job creation form opens
```

#### Step 1.2: Enter Customer Information
```
Form Section 1: Customer Details
├── Search existing customer (Autocomplete dropdown)
│   OR
├── Add new customer:
│   ├── Company Name *
│   ├── Contact Person *
│   ├── Email *
│   ├── Phone Number *
│   └── Address
└── Select Customer
```

#### Step 1.3: Enter Product Details
```
Form Section 2: Product Information
├── Product/Job Name * (Text input)
├── Number of Variants * (Numeric input)
├── Quantity * (Numeric input)
├── Size/Dimensions (e.g., 10x15 cm)
└── Description (Text area)
```

#### Step 1.4: Enter Material Specifications
```
Form Section 3: Material Details
├── Material Type * (Dropdown: Paper, Cardboard, Plastic, etc.)
├── GSM (Grams per Square Meter) *
├── Sheet/Roll * (Radio button selection)
├── Material Size *
├── Required Quantity *
└── Material Cutting Size
```

#### Step 1.5: Printing Specifications
```
Form Section 4: Printing Details
├── Printing Method * (Radio buttons)
│   ├── Flexo Printing
│   └── Offset Printing
├── Printing Size *
├── Number of Colors
└── Special Requirements (Text area)
```

#### Step 1.6: Finishing Requirements
```
Form Section 5: Finishing Options
├── UV Varnish (Checkbox)
├── Die Cut (Checkbox)
├── Lamination (Checkbox)
├── Packing Requirements (Dropdown)
└── Other Finishing (Text area)
```

#### Step 1.7: Order Details
```
Form Section 6: Order Information
├── Order Type * (Radio buttons)
│   ├── New Order
│   └── Repeat Order
├── Expected Delivery Date * (Date picker)
├── Priority Level (Dropdown: Normal, High, Urgent)
└── Roll/Pieces Count
```

#### Step 1.8: File Upload
```
Form Section 7: Reference Files
├── Upload Files (Drag & drop or Browse)
│   ├── Accepted formats: PDF, AI, PSD, JPG, PNG
│   ├── Max file size: 50MB per file
│   └── Multiple files allowed
├── File Preview
└── File Description (optional)
```

#### Step 1.9: Review & Submit
```
Review Screen:
├── Display all entered information
├── Edit button for each section
├── Validation checks:
│   ├── All required fields filled
│   ├── Valid email format
│   ├── Valid date (future date)
│   └── At least one file uploaded
└── Submit Button
```

#### Step 1.10: System Actions After Submit
```
Backend Processing:
1. Generate unique Job ID (e.g., JOB-2025-001)
2. Save job data to database:
   ├── jobs table: INSERT new record
   ├── Set status = "NEW"
   ├── Set created_by = Current User ID
   ├── Set created_date = Current timestamp
   └── Save uploaded files to server
3. Create notifications:
   ├── Accounts Team: "New job #JOB-2025-001 requires billing"
   └── Design Team: "New job #JOB-2025-001 awaiting billing completion"
4. Send email notifications (if enabled)
5. Update dashboard counters
6. Return success message to user

Frontend Response:
├── Show success message: "Job created successfully"
├── Display Job ID: "JOB-2025-001"
├── Redirect to Job Details page
└── Update job list with new entry
```

---

### STAGE 2A: Accounts Team - Billing Process

#### Step 2A.1: View Pending Jobs
```
Accounts User Action:
1. Login to system
2. Dashboard shows:
   ├── Pending Billing: 5 jobs
   ├── Billing In Progress: 2 jobs
   └── Billing Completed: 25 jobs
3. Navigate to "Jobs Requiring Billing" section
4. Job list displays:
   ├── Job ID
   ├── Customer Name
   ├── Product Name
   ├── Created Date
   ├── Status: "NEW"
   └── Action: "Add Billing" button
```

#### Step 2A.2: Apply Filters
```
Filter Options:
├── Status Filter:
│   ├── Pending Billing
│   ├── Billing In Progress
│   └── Billing Completed
├── Customer Filter (Dropdown)
├── Date Range (From - To)
└── Search by Job ID or Customer Name
```

#### Step 2A.3: Open Job for Billing
```
User clicks "Add Billing" or job row
System displays:
┌──────────────────────────────────────┐
│    BILLING DETAILS SCREEN            │
├──────────────────────────────────────┤
│ Section 1: Job Information (Read-only)
│  ├── Job ID: JOB-2025-001
│  ├── Customer: ABC Company
│  ├── Product: Product Labels
│  ├── Size: 10x15 cm
│  ├── Quantity: 5000 pieces
│  ├── Printing Type: Flexo
│  └── Material: 80 GSM Paper
│
│ Section 2: Cost Entry (Editable)
│  ├── Die Cost: [____] SAR
│  ├── Plate Cost: [____] SAR
│  └── Job Cost: [____] SAR
│
│ Section 3: Auto-Calculated (Read-only)
│  ├── Subtotal: 0.00 SAR
│  ├── VAT (16%): 0.00 SAR
│  └── Total Amount: 0.00 SAR
│
│ [Save as Draft] [Submit Billing]
└──────────────────────────────────────┘
```

#### Step 2A.4: Enter Billing Details
```
User Input:
├── Die Cost: 500 SAR (Example)
├── Plate Cost: 300 SAR
└── Job Cost: 2000 SAR

System Auto-Calculation (Real-time):
├── Subtotal = 500 + 300 + 2000 = 2800 SAR
├── VAT = 2800 × 0.16 = 448 SAR
└── Total = 2800 + 448 = 3248 SAR

Display updates automatically as user types
```

#### Step 2A.5: Validation
```
System Checks:
├── Die Cost > 0
├── Plate Cost > 0
├── Job Cost > 0
├── All fields are numeric
└── No negative values allowed

If validation fails:
├── Show error message below field
├── Disable Submit button
└── Highlight invalid fields in red
```

#### Step 2A.6: Submit Billing
```
User clicks "Submit Billing"

Confirmation Modal:
┌──────────────────────────────────────┐
│  Confirm Billing Submission          │
├──────────────────────────────────────┤
│  Die Cost:      500 SAR              │
│  Plate Cost:    300 SAR              │
│  Job Cost:      2000 SAR             │
│  ─────────────────────               │
│  Subtotal:      2800 SAR             │
│  VAT (16%):     448 SAR              │
│  ─────────────────────               │
│  Total:         3248 SAR             │
│                                       │
│  [Cancel]  [Confirm & Submit]        │
└──────────────────────────────────────┘
```

#### Step 2A.7: System Actions After Billing Submit
```
Backend Processing:
1. Update database:
   ├── jobs table:
   │   ├── UPDATE billing_status = "COMPLETED"
   │   ├── UPDATE job_status = "READY_FOR_DESIGN"
   │   └── UPDATE updated_date = Current timestamp
   ├── billing_details table:
   │   ├── INSERT new billing record
   │   ├── job_id = JOB-2025-001
   │   ├── die_cost = 500
   │   ├── plate_cost = 300
   │   ├── job_cost = 2000
   │   ├── subtotal = 2800
   │   ├── vat_amount = 448
   │   ├── total_amount = 3248
   │   └── billing_date = Current timestamp
   └── Save billing_pdf to server

2. Generate invoice PDF

3. Create notifications:
   ├── Design Team: "Job #JOB-2025-001 ready for design"
   └── Sales User: "Billing completed for Job #JOB-2025-001"

4. Send email notifications

5. Update KPI counters

Frontend Response:
├── Show success message
├── Update job status in list
├── Redirect to job list
└── Highlight updated job
```

---

### STAGE 2B: Design Team Waits & Starts

#### Step 2B.1: Notification Received
```
Design User receives two notifications:
1. Initial: "New job #JOB-2025-001 created - awaiting billing"
2. Update: "Job #JOB-2025-001 ready for design - billing complete"
```

#### Step 2B.2: View Ready Jobs
```
Design User Action:
1. Login to system
2. Dashboard shows:
   ├── Ready for Design: 4 jobs
   ├── In Progress: 6 jobs
   ├── Sent for Approval: 2 jobs
   └── Approved: 10 jobs
3. Navigate to "Jobs Ready for Design"
4. Apply filter: Status = "Ready for Design"
```

#### Step 2B.3: Open Job Details
```
Design user clicks on job
System displays:
┌──────────────────────────────────────┐
│    DESIGN JOB DETAILS                │
├──────────────────────────────────────┤
│ Section 1: Job Information
│  ├── Job ID: JOB-2025-001
│  ├── Customer: ABC Company
│  ├── Product: Product Labels
│  ├── Size: 10x15 cm
│  ├── Printing Type: Flexo
│  ├── Quantity: 5000 pieces
│  └── Expected Delivery: 15-Feb-2025
│
│ Section 2: Material Details
│  ├── Material: 80 GSM Paper
│  ├── Sheet/Roll: Sheet
│  ├── Material Size: A4
│  ├── Cutting Size: 10.5x15.5 cm
│  └── Printing Size: 10x15 cm
│
│ Section 3: Finishing Requirements
│  ├── UV Varnish: Yes
│  ├── Die Cut: Yes
│  └── Packing: Standard box packing
│
│ Section 4: Reference Files
│  ├── [📄 reference_design.pdf]
│  ├── [🖼️ sample_image.jpg]
│  └── [Download All]
│
│ Section 5: Billing Information
│  ├── Total Amount: 3248 SAR
│  └── Status: Billing Completed ✅
│
│ [Start Design Work]
└──────────────────────────────────────┘
```

#### Step 2B.4: Start Design Work
```
User clicks "Start Design Work"

System Actions:
1. Update job status:
   ├── design_status = "IN_PROGRESS"
   └── design_started_date = Current timestamp

2. Assign job to current design user

3. Create notification:
   └── Sales User: "Design work started for Job #JOB-2025-001"

4. Enable design file upload section
```

---

### STAGE 3: Design Processing

#### Step 3.1: Design Work Interface
```
Design Work Screen:
┌──────────────────────────────────────┐
│    DESIGN WORKSPACE                  │
├──────────────────────────────────────┤
│ Current Status: IN PROGRESS 🎨       │
│                                       │
│ Upload Design Files:                 │
│  ┌────────────────────────────────┐ │
│  │  Drag & Drop Files Here        │ │
│  │  or Click to Browse            │ │
│  │                                 │ │
│  │  Supported: AI, PSD, PDF, SVG  │ │
│  │  Max size: 100MB per file      │ │
│  └────────────────────────────────┘ │
│                                       │
│ Uploaded Files:                      │
│  ├── [📄 design_v1.ai] [Delete]     │
│  ├── [📄 design_v2.pdf] [Delete]    │
│  └── [Add More Files]                │
│                                       │
│ Design Notes & Remarks:              │
│  ┌────────────────────────────────┐ │
│  │ [Text area for internal notes] │ │
│  └────────────────────────────────┘ │
│                                       │
│ Update Status:                       │
│  ( ) In Progress                     │
│  ( ) Sent for Approval               │
│  ( ) Approved                        │
│                                       │
│ [Save Draft] [Update Status]         │
└──────────────────────────────────────┘
```

#### Step 3.2: Upload Design Files
```
User Action:
1. Drag files to upload area OR click browse
2. Select design files from computer
3. Files upload with progress indicator
4. System validates:
   ├── File format (AI, PSD, PDF, SVG)
   ├── File size (< 100MB)
   └── File integrity

Success:
├── File saved to server
├── File record in database:
│   ├── file_id
│   ├── job_id
│   ├── file_name
│   ├── file_path
│   ├── file_size
│   ├── file_type
│   ├── version_number
│   ├── uploaded_by
│   └── uploaded_date
├── File appears in uploaded list
└── Success message displayed
```

#### Step 3.3: Version Control
```
If user uploads file with same name:
┌──────────────────────────────────────┐
│  File Already Exists                 │
├──────────────────────────────────────┤
│  A file named "design_v1.ai" already│
│  exists. What would you like to do? │
│                                       │
│  ( ) Replace existing file           │
│  ( ) Keep both (create version 2)    │
│  ( ) Cancel upload                   │
│                                       │
│  [Cancel] [Proceed]                  │
└──────────────────────────────────────┘

If "Keep both" selected:
├── Rename new file: design_v1_v2.ai
├── Increment version number
└── Keep history of all versions
```

#### Step 3.4: Add Design Notes
```
Internal Notes Section:
┌──────────────────────────────────────┐
│ Design Notes:                        │
├──────────────────────────────────────┤
│ - Used CMYK color mode               │
│ - Added 3mm bleed                    │
│ - Die cut lines marked in spot color│
│ - UV varnish area marked             │
│                                       │
│ [Save Notes]                         │
└──────────────────────────────────────┘

Notes are saved:
├── design_notes table: INSERT record
├── Timestamp added
├── Designer name added
└── Visible to team members
```

#### Step 3.5: Update Design Status

##### 3.5.1 Send for Approval
```
User selects: "Sent for Approval"
User clicks: "Update Status"

System Actions:
1. Validate:
   ├── At least one design file uploaded ✓
   └── Design notes added (optional)

2. Update database:
   ├── design_status = "SENT_FOR_APPROVAL"
   ├── sent_for_approval_date = Current timestamp
   └── status_updated_by = Current User ID

3. Create notifications:
   └── Sales User: "Design ready for approval - Job #JOB-2025-001"

4. Send email with design preview link

5. Update job list
```

##### 3.5.2 Design Approved
```
After customer/internal approval:

User selects: "Approved"
User clicks: "Update Status"

Confirmation Modal:
┌──────────────────────────────────────┐
│  Approve Design                      │
├──────────────────────────────────────┤
│  Are you sure you want to approve    │
│  this design and send it to          │
│  production?                         │
│                                       │
│  This action will:                   │
│  ✓ Lock design files                │
│  ✓ Notify production team           │
│  ✓ Update job status                │
│                                       │
│  [Cancel] [Confirm Approval]         │
└──────────────────────────────────────┘

System Actions:
1. Update database:
   ├── design_status = "APPROVED"
   ├── job_status = "READY_FOR_PRODUCTION"
   ├── design_approved_date = Current timestamp
   ├── approved_by = Current User ID
   └── Lock design files (no further edits)

2. Create notifications:
   ├── Production Team: "New job ready - #JOB-2025-001"
   └── Sales User: "Design approved - #JOB-2025-001"

3. Send email notifications

4. Update KPI counters

5. Move job to production queue
```

---

### STAGE 4: Production Processing

#### Step 4.1: Production Job List
```
Production User Action:
1. Login to system
2. Dashboard shows:
   ├── Ready for Production: 3 jobs
   ├── Printing: 5 jobs
   ├── Finishing: 3 jobs
   ├── Packing: 2 jobs
   └── Completed: 20 jobs
3. Navigate to "Ready for Production"
```

#### Step 4.2: View Production Job Details
```
User clicks on job
System displays:
┌──────────────────────────────────────┐
│    PRODUCTION JOB DETAILS            │
├──────────────────────────────────────┤
│ Job ID: JOB-2025-001                 │
│ Customer: ABC Company                │
│ Product: Product Labels              │
│ Status: READY FOR PRODUCTION 🏭      │
│                                       │
│ Printing Specifications:             │
│  ├── Type: Flexo Printing            │
│  ├── Size: 10x15 cm                  │
│  ├── Quantity: 5000 pieces           │
│  ├── Colors: 4 Color (CMYK)          │
│  └── Material: 80 GSM Paper          │
│                                       │
│ Material Details:                    │
│  ├── Roll/Sheet: Sheet               │
│  ├── Material Size: A4               │
│  ├── Cutting Size: 10.5x15.5 cm      │
│  └── Required Quantity: 5000 sheets  │
│                                       │
│ Finishing Requirements:              │
│  ├── ✓ UV Varnish                    │
│  ├── ✓ Die Cut                       │
│  └── Packing: Standard boxes         │
│                                       │
│ Design Files:                        │
│  ├── [📄 final_design.pdf] [View]   │
│  └── [Download All]                  │
│                                       │
│ [Start Production]                   │
└──────────────────────────────────────┘
```

#### Step 4.3: Start Production - Printing Stage
```
User clicks "Start Production"

Printing Stage Screen:
┌──────────────────────────────────────┐
│    PRINTING STAGE                    │
├──────────────────────────────────────┤
│ Current Stage: PRINTING 🖨️           │
│                                       │
│ Material Issuance:                   │
│  ├── Material Type: 80 GSM Paper     │
│  ├── Required: 5000 sheets           │
│  ├── Issued: [____] sheets           │
│  └── Wastage: [____] sheets          │
│                                       │
│ Printing Details:                    │
│  ├── Machine Number: [________]      │
│  ├── Operator Name: [________]       │
│  ├── Start Time: [Auto-filled]       │
│  └── Expected End: [Auto-calculated] │
│                                       │
│ Production Notes:                    │
│  ┌────────────────────────────────┐ │
│  │ [Any issues or observations]   │ │
│  └────────────────────────────────┘ │
│                                       │
│ Quality Check:                       │
│  ├── [ ] Color matching verified     │
│  ├── [ ] Registration checked        │
│  └── [ ] Sample approved             │
│                                       │
│ [Save Progress] [Complete Printing]  │
└──────────────────────────────────────┘
```

#### Step 4.4: Complete Printing & Move to Finishing
```
User fills details:
├── Issued: 5200 sheets (including wastage buffer)
├── Machine Number: FLEXO-01
├── Operator: John Doe
├── Notes: "Good print quality, minor adjustment needed"
└── All quality checks: ✓

User clicks "Complete Printing"

System Actions:
1. Validate:
   ├── Issued quantity > 0
   ├── Machine number entered
   └── Quality checks completed

2. Update database:
   ├── production_tracking table:
   │   ├── stage = "PRINTING"
   │   ├── status = "COMPLETED"
   │   ├── material_issued = 5200
   │   ├── material_wastage = 200
   │   ├── machine_number = "FLEXO-01"
   │   ├── operator_name = "John Doe"
   │   ├── start_time = (saved earlier)
   │   ├── end_time = Current timestamp
   │   └── notes = (saved notes)
   ├── jobs table:
   │   └── UPDATE production_status = "FINISHING"

3. Create notification:
   └── Sales User: "Printing completed for Job #JOB-2025-001"

4. Open Finishing stage automatically
```

#### Step 4.5: Finishing Stage
```
Finishing Stage Screen:
┌──────────────────────────────────────┐
│    FINISHING STAGE                   │
├──────────────────────────────────────┤
│ Current Stage: FINISHING ✂️          │
│                                       │
│ Finishing Tasks:                     │
│  ┌─ UV Varnish ─────────────────┐   │
│  │ [ ] Machine: UV-01            │   │
│  │ [ ] Operator: [_______]       │   │
│  │ [ ] Start: [Auto]             │   │
│  │ [ ] End: [____]               │   │
│  │ [ ] Quality OK                │   │
│  └───────────────────────────────┘   │
│                                       │
│  ┌─ Die Cutting ─────────────────┐   │
│  │ [ ] Machine: DIE-02           │   │
│  │ [ ] Die Number: [_______]     │   │
│  │ [ ] Output: [____] pieces     │   │
│  │ [ ] Wastage: [____] pieces    │   │
│  │ [ ] Quality OK                │   │
│  └───────────────────────────────┘   │
│                                       │
│ Finishing Notes:                     │
│  ┌────────────────────────────────┐ │
│  │ [Any issues during finishing]  │ │
│  └────────────────────────────────┘ │
│                                       │
│ [Save Progress] [Complete Finishing] │
└──────────────────────────────────────┘
```

#### Step 4.6: Complete Finishing & Move to Packing
```
User completes all finishing tasks
User clicks "Complete Finishing"

System Actions:
1. Validate all finishing tasks completed

2. Update database:
   ├── production_tracking table:
   │   ├── INSERT finishing record
   │   ├── uv_varnish_status = "COMPLETED"
   │   ├── die_cut_status = "COMPLETED"
   │   ├── output_quantity = 4950
   │   ├── wastage_quantity = 50
   │   └── finish_end_time = Current timestamp
   ├── jobs table:
   │   └── UPDATE production_status = "PACKING"

3. Create notification:
   └── Sales User: "Finishing completed for Job #JOB-2025-001"

4. Open Packing stage
```

#### Step 4.7: Packing Stage
```
Packing Stage Screen:
┌──────────────────────────────────────┐
│    PACKING STAGE                     │
├──────────────────────────────────────┤
│ Current Stage: PACKING 📦            │
│                                       │
│ Final Quantity:                      │
│  ├── Produced: 4950 pieces           │
│  ├── Ordered: 5000 pieces            │
│  ├── Shortfall: 50 pieces            │
│  └── Status: ⚠️ Under by 1%          │
│                                       │
│ Packing Details:                     │
│  ├── Packing Type: Standard Boxes    │
│  ├── Pieces per Box: [____]          │
│  ├── Number of Boxes: [Auto-calc]    │
│  ├──

