# RedHat Openshift Container Platform - F5 UDF Bare Metal Deployment Blueprint

IMPORTANT NOTE: Tested with OCP 4.9.0 and RHCOS 4.9.0

Please follow instructions [here](https://github.com/gelmistefano/f5-udf-ocp-baremetal-blueprint)

Once finished you can access the openshift console using the linux-jumphots via XRDP using Firefox.
Openshift console is here: https://console-openshift-console.apps.ocp.f5-udf.com

The username us "kubeadmin" and you can look for the auto-generated password on ocp-web in /usr/share/nginx/html/installations/auth/kubeadmin-password

```bash
sudo cat /usr/share/nginx/html/installations/auth/kubeadmin-password
```
