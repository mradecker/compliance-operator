# Compliance Operator Automation

This role allows the user to install the Compliance Operator through a Catalog Source on OpenShift or to clone the git repo and use the files in https://github.com/openshift/compliance-operator, run compliance scans based on profiles set in the defaults/main.yml, and extract results ( Compliance Check Results and XML results ).

Make sure you are logged into the OpenShift cluster with the appropriate credentials to create projects and OpenShift/Kubernetes resources.

# Install and run scans

In the defaults/main.yml, select which way you want to install the compliance operator by setting the boolean value to "yes/no" and setting the boolean for "scan" to "yes/no"

Use the compliance-operator.yml file to run the playbook.  Ensure you have the correct host listed in this file.  Default is "bastion"

  ex. ansible-playbook compliance-operator.yml -vv
  
# Extraction of results

It is recommended to wait until the scan completes before trying to run the plays for results.  Sometimes the scan pods on the nodes take longer than expected or error out due to various issues.  This will cause the playbook to hang if trying to set both the "scan" and "results" booleans to "yes"

Run "oc get pods" and ensure that all scan pods have a status of "completed" and there are PVCs for each type of scan (worker, master, ocp4).

After varifying that the scan completed properly and that there are PVCs for both master and worker scans, set the boolean for "scan" to "no" and the boolean for "results" to "yes" and re-run the playbook to automate the extraction of results xml files and get Compliance Check Results.  

In OpenShift, after the scan completes and results are checked, run "oc get complianceremediations" to see what remediation options there are.  

After deciding which remediations need to be applied, go into the remediation by typing "oc edit compliancerememdiation <name of remediation>" and set the Apply boolean to "yes" which will result in the machineconfig applying the changes and rebooting the nodes
