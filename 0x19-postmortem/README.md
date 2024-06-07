Postmortem: Outage on E-learning Platform "EduPro"

Issue Summary:
Duration: The outage occurred on June 1, 2024, from 10:00 to 12:45 GMT.
Impact: Our E-learning platform, EduPro, was significantly disrupted. Users experienced slow page loads, intermittent access to course materials, and failure to submit assignments. Approximately 85% of users were affected, leading to numerous customer complaints and a temporary halt in online classes.
Root Cause: A misconfigured Nginx server directive, combined with an unplanned database update, caused high server load and database connection timeouts.

Timeline:
10:00 - Issue detected by the on-call engineer via monitoring alerts showing high CPU usage and increased error rates.
10:05 - Users began reporting issues accessing the site and submitting assignments.
10:15 - Initial investigation focused on server performance metrics, assuming a spike in user traffic was overwhelming the servers.
10:30 - Misleading path: Suspected a DDoS attack, leading to temporary implementation of restrictive firewall rules.
10:45 - Web development team investigated the application code and server configurations.
11:15 - Issue escalated to the database team after noticing high query response times and frequent connection drops.
11:45 - Root cause identified: A recent change in Nginx configuration and a simultaneous unplanned database update were causing conflicts, leading to server overload and connection issues.
12:00 - Database update was rolled back to the previous stable state.
12:15 - Nginx configuration was corrected, and server load started to stabilize.
12:45 - Full service was restored, and performance returned to normal levels.

Root Cause and Resolution:
Root Cause: The incident stemmed from a combination of issues:
Misconfigured Nginx Directive: A recent change in the Nginx server configuration to handle SSL termination inadvertently set a very low timeout for upstream connections. This caused frequent request timeouts under load.
Unplanned Database Update: Simultaneously, a database update was applied during a peak usage period without prior notification or proper load testing. This update included a schema change that led to slow query performance and increased CPU usage.
Resolution: The following steps were taken to resolve the issue:
Database Rollback: The database was rolled back to the previous version, which alleviated the high CPU load and query response times.
Nginx Configuration Fix: The Nginx configuration was reverted to the previous settings. Specifically, the timeout values were adjusted to more appropriate levels to handle expected traffic loads.
Restart Services: All related services were restarted to ensure the new configurations took effect and cleared any residual issues.

Corrective and Preventative Measures:
Improvements Needed:
Stricter Change Management: Implement a more rigorous change management process for server configurations and database updates, including mandatory pre-deployment reviews and load testing.
Enhanced Monitoring: Improve monitoring to better detect and isolate issues related to server configuration and database performance.
Documentation and Training: Enhance documentation around server configuration changes and provide additional training for engineers on handling high-availability systems.
Specific Tasks:
Review and Document Nginx Settings: Review all Nginx configuration settings and document the rationale for each setting to prevent similar issues.
Implement Database Update Protocol: Develop a standardized protocol for database updates, including scheduled maintenance windows, load testing, and rollback plans.
Increase Monitoring Granularity: Add detailed monitoring for Nginx directives and database query performance metrics to catch configuration issues early.
Create a Change Review Board: Establish a Change Review Board to oversee and approve all significant infrastructure changes.
Conduct Training Sessions: Organize regular training sessions for the engineering team on best practices for server configuration and database management.

