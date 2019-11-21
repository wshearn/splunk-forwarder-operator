# splunk-forwarder-operator

This operator manages Splunk Universal Forwarder. It deploys a daemonset which 
deploys a pod on each noed including the masters. It expects the service account
for the namespace can deploy privilaged pods. It also needs a secret that holds
the forwarder auth.

If you are using splunk cloud you can download the spl file, extract it with
`tar xvf splunkclouduf.spl` and create a secret with the files as is and they
will be mounted in the correct place.

The CRD is very to point to the files you want to ship(currently only supports
monitor://).

```json
apiVersion: splunkforwarder.managed.openshift.io/v1alpha1
kind: SplunkForwarder
metadata:
  name: example-splunkforwarder
spec:
  image: dockerimageurl
  imageTag: "versiontag"
  splunkLicenseAccepted: true
  splunkInputs:
  - path: /host/var/log/openshift-apiserver/audit.log
    index: openshift_managed_audit
    whitelist: \.log$
    sourcetype: _json
  - path: /host/var/log/containers/ip-*-*-*-*ec2internal-debug*.log
    index: openshift_managed_debug_node
    whitelist: \.log$
    sourcetype: _json
```

The image and imageTag are for the image in /forwarder (currently version 
7.3.2-c60db69f8e32)