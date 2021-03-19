# compliance-operator

This role allows the user to install the Compliance Operator through a Catalog Source on OpenShift or to clone the git repo and use the files in https://github.com/openshift/compliance-operator

In the defaults/main.yml, select which way you want to install the compliance operator by setting the boolean value to "yes/no" and setting the boolean for "scan" to "yes/no"

It is recommended to wait until the scan completes before trying to run the plays for results.  Sometimes the scan pods on the nodes take longer than expected or error out due to various issues.  This will cause the playbook to hang if trying to set both the "scan" and "results" booleans to "yes"

After varifying that the scan completed properly and that there are PVCs for both master and worker scans, set the boolean for "scan" to "no" and the boolean for "results" to "yes" and re-run the playbook to automate the extraction of results xml files.  

If you DO NOT need the XML files for viewing, you do not need to run the results playbook.  In OpenShift, after the scan completes, run "oc get complianceremediations" to see what remediation options there are.  After deciding which remediations need to be applied, go into the remediation by typing "oc edit compliancerememdiation <name of remediation>" and set the Apply boolean to "yes" which will result in the machineconfig applying the changes and rebooting the nodes
