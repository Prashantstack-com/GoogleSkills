                                      Google Cloud IAM & Cloud Storage Lab Overview

This lab focused on exploring IAM roles, creating a Cloud Storage bucket, and testing access control between two users.

Users:

Username 1 (Owner): student-04-9441eb735896@qwiklabs.net

Username 2 (Viewer / Storage Object Viewer): student-04-2e834b3afbb4@qwiklabs.net

Bucket Details

Bucket Name: qwiklabs-gcp-03-cc875adf5397

File: sample.txt

Tested via Cloud Shell:

gsutil ls gs://qwiklabs-gcp-03-cc875adf5397/

Output:

*gs://qwiklabs-gcp-03-cc875adf5397/sample.txt

Key Learnings from the lab was: 

Viewer, Editor, and Owner roles control what users can see and do at the project level.

Resource-level roles (like Storage Object Viewer) let users access specific resources even without project access.

IAM changes take a short time to propagate.

Challenges faced during the lab was:

Understanding why Username 2 could see the bucket but not modify it.

Waiting for IAM permission changes to fully take effect.

Conclusion

The lab gave me a little hands-on experience with IAM and Cloud Storage access control, helping me understand project vs resource-level permissions in Google Cloud.
