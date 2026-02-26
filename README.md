# ServiceNowAdministration-internship

#🚫 Prevent User Deletion if Assigned to an Incident
🔐 A ServiceNow Data Integrity Protection Project
📖 Overview

In real-world IT Service Management (ITSM) environments, users (agents, analysts, engineers) are frequently assigned to incidents for tracking, troubleshooting, and resolution.

But what happens if someone accidentally deletes a user who is still assigned to active incidents?

⚠️ Broken references
⚠️ Loss of accountability
⚠️ Incomplete audit trails
⚠️ Workflow disruption

To prevent this critical issue, this project implements a Business Rule in ServiceNow that blocks user deletion if the user is assigned to any incidents.

Deletion is only allowed after:

All incidents are closed, or

Incidents are reassigned to another user

This ensures data integrity, accountability, and seamless workflow continuity.

#🎯 Project Objectives

✅ Prevent accidental deletion of users with active incident assignments
✅ Maintain referential integrity within ITSM tables
✅ Ensure workflow continuity and audit traceability
✅ Demonstrate server-side validation using Business Rules
✅ Showcase practical GlideRecord usage

#🧠 Real-World Problem Statement

In enterprise environments like those using ServiceNow, incidents are linked to users via reference fields.

If a user is deleted while still assigned:

Incident records retain invalid references

SLA tracking may fail

Reporting dashboards become inconsistent

Compliance and auditing processes are affected

This solution proactively prevents such system inconsistencies.

#🛠️ Technology Stack
Component	Details
Platform	ServiceNow (Personal Developer Instance)
Module	ITSM
Feature Used	Business Rules
Scripting Language	Server-side JavaScript
API Used	GlideRecord
Version Control	GitHub
Deployment Method	Update Sets (XML export/import)
⚙️ Implementation Breakdown
🔹 Business Rule Configuration
Setting	Value
Name	Prevent User Deletion if Assigned to an Incident
Table	sys_user (User table)
When	Before
Action	Delete
Condition	Runs on delete operation
Advanced	Enabled
🔎 How It Works

When a delete operation is triggered on a user record

The Business Rule executes before deletion

It queries the incident table

Checks if the current user is assigned to any incident

If at least one record exists:

❌ Deletion is blocked

⚠️ Error message is displayed

If no incidents exist:

✅ Deletion proceeds normally

#💻 Core Script Logic
(function executeRule(current, previous /*null when async*/) {

    var incGr = new GlideRecord('incident');
    incGr.addQuery('assigned_to', current.sys_id);
    incGr.setLimit(1); // Optimize: Only check existence
    incGr.query();

    if (incGr.next()) {
        gs.addErrorMessage('This user cannot be deleted because they are assigned to one or more incidents.');
        current.setAbortAction(true);
    }

})(current, previous);
🚀 Why This Implementation Is Efficient

✔ Uses setLimit(1) for performance optimization
✔ Executes before delete to prevent unwanted DB changes
✔ Uses server-side validation (more secure than client-side)
✔ Maintains relational integrity
✔ Lightweight and scalable

#📊 Architecture Flow

User Delete Attempt
⬇
Business Rule Triggered (Before Delete)
⬇
Query Incident Table
⬇
Incident Exists?
➡ Yes → ❌ Abort Delete + Show Error
➡ No → ✅ Allow Delete

🧪 Testing Scenarios
Scenario	Expected Result
User assigned to 1 active incident	❌ Deletion blocked
User assigned to multiple incidents	❌ Deletion blocked
User assigned only to closed incidents	❌ (Still blocked unless reassigned)
User not assigned to any incidents	✅ Deletion allowed
🔒 Key Learning Outcomes

Deep understanding of ServiceNow Business Rules

Practical use of GlideRecord

Server-side scripting best practices

Data integrity enforcement in ITSM systems

Enterprise-safe deletion control mechanism

#📈 Future Enhancements

✨ Allow deletion only if incidents are in "Closed" state
✨ Display incident count in error message
✨ Redirect to incident list automatically
✨ Convert logic into reusable Script Include
✨ Implement similar rule for Change / Problem tables

#🏁 Conclusion

This project demonstrates a real-world enterprise safeguard in ServiceNow that prevents system inconsistency and protects workflow continuity.

By implementing a simple yet powerful Business Rule, we ensure:

🔐 Data integrity
📊 Accurate reporting
🧾 Audit compliance
🔄 Workflow stability

Gonnabattula Likhita
AI & ML Student | ServiceNow Learner | Aspiring Developer
