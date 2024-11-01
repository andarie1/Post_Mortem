📝 Post-Mortem Report
Incident Date: October 29th
Time of Outage: 2:35 PM
Affected Service: Production server at https://3.73.44.38/index.php/Main_Page
Impact: User access interrupted, significant business workflow disruption

Problem Identification & Resolution Steps:
Access Credentials: Requested SSH key from sys-admin and saved it in a secure .txt file for entry. Deleted later for a safety reason.

Server Login: Used the command 'ssh -i file_with_key ec2-user@3.73.44.30' to access the server.

Process Check: Ran top to observe current processes and identify resource usage.

Disk Space Issue Identified: Attempted cat /etc/, but encountered an error: bash: No space left on device. This indicated storage saturation.

File Search for Large Items: Executed 'find / -type f -size +100M -exec du -h {} +' to locate oversized files. Found a 7GB log file at /var/log/httpd/access_log.

File Clearance: Elevated privileges with sudo su to truncate the 7GB log file, freeing up significant space.

Monitoring Space Reclaim: Observed a 33% disk space recovery after 15 minutes.

Task Review: Used crontab -l to check scheduled tasks, finding a script that archived data every minute.

Crontab Adjustment: Reduced the frequency of the archiving job to daily to prevent excessive log growth.

Workflow Verification: Reloaded the server page to confirm improved accessibility and functionality.

Preventive Measures
New Backup Script: Created a backup script (/sys/log/my_path/backup-v20.sh) and added it to crontab for regular monitoring.

Server Recovery Protocol: Advised sys-admin to maintain a regular production backup schedule.

Resolution Time: 1 hour and 22 minutes
Status: Server functionality restored; ongoing monitoring scheduled.



Conclusion
These modifications would strengthen server management, streamline incident response, and prevent similar space saturation issues in the future.






