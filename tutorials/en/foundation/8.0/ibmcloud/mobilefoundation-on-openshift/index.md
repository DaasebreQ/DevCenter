---
layout: tutorial
breadcrumb_title: Foundation on Red Hat OpenShift
title: Deploy Mobile Foundation to an existing Red Hat OpenShift Container Platform
weight: 2
---
<!-- NLS_CHARSET=UTF-8 -->

Learn how to install the Mobile Foundation instance on an OpenShift cluster using the IBM Mobile Foundation Operator.

There are two ways of getting the entitlement to OpenShift Container Platform.

* You have the entitlement to IBM Cloud Pak for Applications, which includes the OpenShift Container Platform entitlement.
* You have an existing OpenShift Container Platform (bought from Red Hat).

The steps to deploy Mobile Foundation on OCP are the same irrespective of how you have obtained the OCP entitlement.

### Prerequisites
{: #prereqs}

Following are the prerequisites before you begin the process of installing Mobile Foundation instance using the Mobile Foundation Operator.

- OpenShift cluster v3.11
- [OpenShift client tools](https://docs.openshift.com/container-platform/3.11/cli_reference/get_started_cli.html) (`oc`)
- Mobile Foundation requires a database. Create a supported database and keep the database access details handy for further use. See [here](https://mobilefirstplatform.ibmcloud.com/tutorials/ru/foundation/8.0/installation-configuration/production/prod-env/databases/).
- Mobile Foundation Analytics requires mounted storage volume for persisting Analytics data (NFS recommended).



## Installing an IBM Mobile Foundation instance

### Download the IBM Mobile Foundation package
{: #download-mf-package}

Download the IBM Mobile Foundation package for Openshift from [IBM Passport Advantage (PPA)](https://www-01.ibm.com/software/passportadvantage/pao_customer.html). Unpack the archive to a directory called `workdir`.

### Validating IBM Mobile Foundation archive downloaded from Passport Advantage Archive (Optional)
{: #validating-mf-from-ppa}

IBM Mobile Foundation package for Red Hat OpenShift Container Platform, which is available in Passport Advantage, is code signed (with Digi Cert) to enforce integrity of the package. A `.sig` (signature file) and a `.pub` (RSA public key) file are shipped to the Passport Advantage along with the IBM MobileFirst Platform Foundation V8.0 .tar.gz file of IBM MobileFirst Platform Server on Red Hat OpenShift Container Platform. Customers can validate the integrity of the package by verifying the signature as below.

#### Package Information as available in Passport Advantage

**Package**: IBM MobileFirst Platform Foundation V8.0 .tar.gz file of IBM MobileFirst Platform Server on Red Hat OpenShift Container Platform English (Example: eImage Part Number: CC3FDEN)

**Signature File**: Signature file for IBM MobileFirst Platform Foundation V8.0 .tar.gz file of IBM MobileFirst Platform Server on Red Hat OpenShift Container Platform English (Example: eImage Part Number: CC3FEEN)

**RSA Public Key**: Public Key file for IBM MobileFirst Platform Foundation V8.0 .tar.gz file of IBM MobileFirst Platform Server on Red Hat OpenShift Container Platform English (eImage Part Number: CC3FFEN)

#### Steps to verify the signature

* [openssl](https://www.openssl.org), download and install the openssl toolkit.
* Now verify the IBM Mobile Foundation package using the following command.
  ```bash
  openssl dgst -sha256 -verify <PUBLIC_KEY> -signature <SIGNATURE_FILE> <IBM MOBILE FOUNDATION PACKAGE ARCHIVE>
  ```
  For example,
  ```bash
  openssl dgst -sha256 -verify CC3FFEN.pub -signature CC3FEEN.sig CC3FDEN.tar.gz
  ```  

### Unpack the IBM Mobile Foundation package
{: #unpack-mf-package}

Unpack the IBM Mobile Foundation package for Openshift using the following command.

```bash
tar xzvf IBM-MobileFoundation-Openshift-Pak-<version>.tar.gz -C <workdir>/
```

### Setup the OpenShift project for Mobile Foundation
{: #setup-openshift-for-mf}

1. Log in to OpenShift cluster and create a new project.
   ```bash
   export MFOS_PROJECT=<project-name>
   oc login -u <username> -p <password> <cluster-url>
   oc new-project $MFOS_PROJECT
   ```

2. Load and push the images to OpenShift registry from local.
   ```bash
    docker login -u <username> -p $(oc whoami -t) $(oc registry info)
    cd <workdir>/images
    ls * | xargs -I{} docker load --input {}

    for file in * ; do
      docker tag ${file/.tar.gz/} $(oc registry info)/$MFOS_PROJECT/${file/.tar.gz/}
      docker push $(oc registry info)/$MFOS_PROJECT/${file/.tar.gz/}
    done
   ```

3. Create a secret with database credentials.
  ```yaml
  cat <<EOF | oc apply -f -
  apiVersion: v1
  data:
      MFPF_ADMIN_DB_USERNAME: <base64-encoded-string>
      MFPF_ADMIN_DB_PASSWORD: <base64-encoded-string>
      MFPF_RUNTIME_DB_USERNAME: <base64-encoded-string>
      MFPF_RUNTIME_DB_PASSWORD: <base64-encoded-string>
      MFPF_PUSH_DB_USERNAME: <base64-encoded-string>
      MFPF_PUSH_DB_PASSWORD: <base64-encoded-string>
      MFPF_APPCNTR_DB_USERNAME: <base64-encoded-string>
      MFPF_APPCNTR_DB_PASSWORD: <base64-encoded-string>
  kind: Secret
  metadata:
  name: mobilefoundation-db-secret
  type: Opaque
  EOF
  ```
  > **Note**: An encoded string can be obtained using `echo -n <string-to-encode> | base64`.

4. For Mobile Foundation Analytics, configure a persistent volume (PV).

    ```yaml
    cat <<EOF | oc apply -f -
    apiVersion: v1
    kind: PersistentVolume
    metadata:
        labels:
            name: mfanalyticspv
            name: mfanalyticspv
    spec:
        accessModes:
        - ReadWriteMany
           capacity:
           storage: 20Gi
           nfs:
           path: <nfs-mount-volume-path>
           server: <nfs-server-hostname-or-ip>
    EOF
    ```

5. For Mobile Foundation Analytics, configure a persistent volume claim (PVC).

   ```yaml
   cat <<EOF | oc apply -f -
   apiVersion: v1
   kind: PersistentVolumeClaim
   metadata:
    name: mfanalyticsvolclaim
   namespace: <project-name-or-namespace>
   spec:
    accessModes:
    - ReadWriteMany
    resources:
   	  requests:
   	  storage: 20Gi
    selector:
   	 matchLabels:
   	  name: mfanalyticspv
      volumeName: mfanalyticspv
    status:
     accessModes:
     - ReadWriteMany
     capacity:
   	  storage: 20Gi
   EOF
   ```

<!--   

### Load the IBM Mobile Foundation images to the local docker registry
{: #load-mf-local-registry}

Use the following command to load the IBM Mobile Foundation images to your local docker registry.

```bash
cd mfospkg/images
ls * | xargs -I{} docker load --input {}
```

### Initialize the OpenShift cluster
{: #initialize-oc-cluster}

>**Note**: Install the OpenShift cluster and the OpenShift client tools (oc) before proceeding with this step.

Use the OpenShift client tool (`oc`) to execute the following command to initialize the OpenShift cluster.

```bash
oc login -u <username> -p <password> <openshift cluster url>
oc new-project <project-name>
```

### Make the IBM Mobile Foundation images available to OpenShift cluster
{: #make-mf-available-to-oc}

Follow the steps below to make the IBM Mobile Foundation images available to your OpenShift cluster.

1.  Log in to the Docker registry by using your access token.
    ```bash
    oc project <project-name>
    docker login -u <username> -p $(oc whoami -t) $(oc registry info)
    ```

2.  List the Mobile Foundation component images from the local docker registry.
    ```bash
    docker images | grep mf
    ```

3.  Tag and push the Mobile Foundation images to the registry. Repeat the below commands for all the Mobile Foundation images (including the `ibm-mf-operator`).
    ```bash
    docker tag <image-id-from-local-docker-registry> ${REGISTRY_INFO}/<project-name>/<mf-image-name>:<image-version>
    docker push ${REGISTRY_INFO}/<project-name>/<mf-image-name>:<image-version>
    ```

4.  Modify the image value and the tag version in the `deploy/crds/charts_v1alpha1_ibmmf_cr.yaml` to point to `${REGISTRY_INFO}/<project-name>/<mf-image-name>` and `<image-version>`.

5.  Provide the valid database details under the DB section, of each component to be installed, in the file `deploy/crds/charts_v1alpha1_ibmmf_cr.yaml`.

6.  Set the Mobile Foundation Operator image name in the `deploy/operator.yaml`, optionally you can use `sed` utility as follows.
    ```bash
    sed -i "" 's|REPLACE_IMAGE|docker-registry.default.svc:5000/mf/mf-operator:<VERSION>|g' deploy/operator.yaml
    ```

### Accessing Mobile Foundation images from an external registry with ImagePullSecrets and ImagePolicies
{: #access-mf-images-external-registry}

If the Mobile Foundation images are available under a different OpenShift project name or in an external registry like dockerhub or any other private registry then follow the steps below.

1.  Create an **imagePolicy** using the following command.

    ```yaml
    cat <<EOF | oc apply -f -
    apiVersion: securityenforcement.admission.cloud.ibm.com/v1beta1
    kind: ImagePolicy
    metadata:
     name: image-policy
     namespace: <project_or_namespace>
    spec:
     repositories:
     - name: docker.io/*
       policy: null
     - name: <image_registry_server>/*
       policy: null
    EOF
    ```

2.  Create an **imagePullSecret** to be able to pull the images from the container registry.
    ```bash
    oc create secret docker-registry <image_pull_secret_name> --docker-server=<image_registry_server> --docker-username=<user_name> --docker-password=<password>
    ```

3. Set the **imagePullSecret** name for the Operator's Service Account definition in `deploy/operator.yaml`.
```yaml
imagePullSecrets:
- name: <image_pull_secret_name>
```
-->

### Deploy the IBM Mobile Foundation Operator
{: #deploy-mf-operator}

1.  Ensure the Operator image name (*mf-operator*) with tag is set for the operator in `deploy/operator.yaml` (**REPO_URL**).
    ```bash
    sed -i 's|REPO_URL|<image-repo-url>:<image-tag>|g' deploy/operator.yaml
    ```

2.  Ensure the namespace is set for the cluster role binding definition in `deploy/cluster_role_binding.yaml` (**REPLACE_NAMESPACE**).
    ```bash
    sed -i 's|REPLACE_NAMESPACE|$MFOS_PROJECT|g' deploy/cluster_role_binding.yaml
    ```

3.  Run the following commands to deploy CRD, operator and install Security Context Constraints (SCC).
    ```bash
    oc create -f deploy/crds/charts_v1_mfoperator_crd.yaml
    oc create -f deploy/
    oc adm policy add-scc-to-group mf-operator system:serviceaccounts:$MFOS_PROJECT
    ```

### Deploy IBM Mobile Foundation components
{: #deploy-mf-components}

To deploy any of the Mobile Foundation components, modify the custom resource configuration `deploy/crds/charts_v1_mfoperator_cr.yaml` according to your requirements. Complete reference to the custom configuration is found [here](https://github.ibm.com/MobileFirst/ibm-mobilefoundation-helm-operator/blob/development/cr-configuration.md).

```bash
oc apply -f deploy/crds/charts_v1_mfoperator_cr.yaml
```

Access the Mobile Foundation server console from `https://<openshift-cluster-url>/mfpconsole`.


### Install the Mobile Foundation instance
{: #install-mf}

1.  Create the Persistent Volume (PV) resource for analytics.
    ```yaml
    cat <<EOF | oc apply -f -
    kind: PersistentVolumeClaim
    apiVersion: v1
    metadata:
      labels:
        app: mfpanalytics
      name: analyticsclaim
      namespace: <your-namespace>
    spec:
      accessModes:
        - ReadWriteMany
      resources:
        requests:
          storage: 8Gi
    EOF
    ```

2. Create a secret with the database credentials.
   ```yaml
    cat <<EOF | oc apply -f -
    apiVersion: v1
    data:
     MFPF_ADMIN_DB_USERNAME: <base64-encoded-string>
     MFPF_ADMIN_DB_PASSWORD: <base64-encoded-string>
     MFPF_RUNTIME_DB_USERNAME: <base64-encoded-string>
     MFPF_RUNTIME_DB_PASSWORD: <base64-encoded-string>
     MFPF_PUSH_DB_USERNAME: <base64-encoded-string>
     MFPF_PUSH_DB_PASSWORD: <base64-encoded-string>
     MFPF_APPCNTR_DB_USERNAME: <base64-encoded-string>
     MFPF_APPCNTR_DB_PASSWORD: <base64-encoded-string>
    kind: Secret
    metadata:
     name: mobilefoundation-db
    type: Opaque
    EOF
   ```  

3. **[OPTIONAL]**: Create the secrets below if you need to install the Application Center.
   ```yaml
   cat <<EOF | oc apply -f -
   apiVersion: v1
   data:
    MFPF_APPCNTR_ADMIN_USER: <base64-encoded-string>
    MFPF_APPCNTR_ADMIN_PASSWORD: <base64-encoded-string>
   kind: Secret
   metadata:
    name: appcntrlogin
   type: Opaque
   EOF
   ```

   ```yaml
    cat <<EOF | oc apply -f -
    apiVersion: v1
    data:
     MFPF_APPCNTR_DB_USERNAME: <base64-encoded-string>
     MFPF_APPCNTR_DB_PASSWORD: <base64-encoded-string>
    kind: Secret
    metadata:
     name: appcenter-dbsecret
    type: Opaque
    EOF
    ```

4.  Create the TLS secret for Ingress.
    ```bash
    openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout tls.key -out tls.crt -subj "/CN=hears1.fyre.ibm.com/O=hears1.fyre.ibm.com"
    kubectl create secret tls mf-tls-secret --key=tls.key --cert=tls.crt
    ```

5.  To add details like database hostname, port, secret, ingress hostname and more, customize the Custom Resource (CR) with the configuration of the Mobile Foundation instance and create the Custom Resource.
    ```yaml
    oc apply -f deploy/crds/charts_v1_mfoperator_cr.yaml
    ```

The properties that can be customized can be found [here]().

### Enabling Ingress parameters
{: #enable-ingress-parameters}

1.  For **HTTP Deployments**, ingress section in `cr.yaml` looks as below:
    ```yaml
    ingress:
      hostname: "myhost.mydomain.com"
      secret: ""
      sslPassThrough: false
    ```

2.  For **HTTPS deployments**, TLS secret is mandatory.
    * Generate `tls.key` and `tls.crt` using the following command:
      ```bash
      openssl req -new -x509 -key tls.key -out tls.crt -days 360 -subj /CN="myhost.mydomain.com"
      ```

    * Create ingress tls secret using following command:
      ```bash
      kubectl create secret tls mf-tls-secret --key=tls.key --cert=tls.crt
      ```

    ingress section in `deploy/crds/charts_v1_mfoperator_cr.yaml` looks as below:
    ```yaml
    ingress:
      hostname: "myhost.mydomain.com"
      secret: "mf-tls-secret"
      sslPassThrough: false
    ```

3.  For HTTPS to backend services, `tls.crt` needs to be imported to `keystore.jks` and `truststore.jks`.

    Pre-create a secret with `keystore.jks` and `truststore.jks` by including the `tls.crt` created in step 2 into the keystore and truststore along with keystore and truststore password using the literals KEYSTORE_PASSWORD and TRUSTSTORE_PASSWORD. Provide the secret name in the field *keystoreSecret* of respective component in the `deploy/crds/charts_v1_mfoperator_cr.yaml`.

    Keep the files `keystore.jks`, `truststore.jks` and its passwords as below.

    For example,

    ```bash
    oc create secret generic server-stores --from-file=./keystore.jks --from-file=./truststore.jks --from-literal=KEYSTORE_PASSWORD=worklight --from-literal=TRUSTSTORE_PASSWORD=worklight
    ```

    >**NOTE**: The names of the files and literals should be the same as mentioned in command above.	Provide this secret name in *keystoreSecret* input field of respective component to override the default keystores when configuring custom resource.

    ingress section in `deploy/crds/charts_v1_mfoperator_cr.yaml` looks as below:

    ```yaml
    ingress:
      hostname: "myhost.mydomain.com"
      secret: "mf-tls-secret"
      sslPassThrough: false
    https: true
    mfpserver:
      keystoreSecret: "server-stores"
    ```  

### Clean-up
{: #clean-up}

Use the following commands to perform a post installation clean up.
```bash
oc delete -f deploy/crds/charts_v1_mfoperator_cr.yaml
oc delete -f deploy/
oc delete -f deploy/crds/charts_v1_mfoperator_crd.yaml
```

## Using Oracle (or) MySQL as IBM Mobile Foundation database
{: #non-db2-mf-database}

A pre-configured database is required to store the data for Mobile Foundation server, Push and Application Center components.

Follow the instructions [here](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/installation-configuration/production/prod-env/databases/#mysql-database-and-user-requirements) to setup Mobile Foundation with a non-DB2 database.

<!--
By default, Mobile Foundation installers is packaged with IBM DB2 JDBC drivers. For Oracle and MySQL, make sure that the JDBC driver (for MySQL - use the Connector/J JDBC driver,  for Oracle - use the Oracle thin JDBC driver) is placed within a Persistent Volume.

1. Place the JDBC driver in a NFS mounted volume. Example: */nfs/share/dbdrivers*

2. Create a Persistent Volume (PV) and Persistent Volume Claim (PVC) by providing the correct NFS server details and the path where the JDBC driver is stored. Sample yaml is shown below.

    ```yaml
     # Sample PersistentVolume.yaml
     cat <<EOF | kubectl apply -f -
        apiVersion: v1
        kind: PersistentVolume
        metadata:
          labels:
            name: mfppvdbdrivers
          name: mfppvdbdrivers
        spec:
          accessModes:
          - ReadWriteMany
          capacity:
            storage: 20Gi
          nfs:
            path: <nfs_path>
            server: <nfs_server>
         EOF
    ```

    ```yaml
    # Sample PersistentVolumeClaim.yaml
      cat <<EOF | kubectl apply -f -
      apiVersion: v1
      kind: PersistentVolumeClaim
      metadata:
        name: mfppvc
        namespace: projectname-or-namespace
      spec:
        accessModes:
        - ReadWriteMany
        resources:
          requests:
             storage: 20Gi
        selector:
          matchLabels:
            name: mfppvdbdrivers
        volumeName: mfppvdbdrivers
      status:
        accessModes:
        - ReadWriteMany
        capacity:
          storage: 20Gi
      EOF
    ```   

    > **NOTE**: Make sure you add the right *projectname-or-namespace* in the yaml above.

### [OPTIONAL] Handling Mobile Foundation database operations with special (admin) privileges
{: #handle-mf-db-operations}

We can have a separate database admin secret to execute the database initialization tasks, which would in turn create the required Mobile Foundation schema and tables in the database (if it does not already exist). Through the database Admin secret, you can control the DDL operations on your database instance.

If the `MFP Server DB Admin Secret` and `MFP Appcenter DB Admin Secret` details are not provided, then the default `Database Secret Name` will be used to perform database initialization tasks.

Run the below code snippet to create a `MFP Server DB Admin Secret` for Mobile Foundation Server.

```yaml
# Create MFP Server Admin DB secret for Mobile Foundation server component
cat <<EOF | kubectl apply -f -
apiVersion: v1
data:
  MFPF_ADMIN_DB_ADMIN_USERNAME: encoded_uname
  MFPF_ADMIN_DB_ADMIN_PASSWORD: encoded_password
  MFPF_RUNTIME_DB_ADMIN_USERNAME: encoded_uname
  MFPF_RUNTIME_DB_ADMIN_PASSWORD: encoded_password
  MFPF_PUSH_DB_ADMIN_USERNAME: encoded_uname
  MFPF_PUSH_DB_ADMIN_PASSWORD: encoded_password
kind: Secret
metadata:
  name: mfpserver-dbadminsecret
type: Opaque
EOF
```

Run the below code snippet to create a `MFP Appcenter DB Admin Secret` for Appcenter.

```yaml
# Create Appcenter Admin DB secret for Mobile Foundation Appcenter
cat <<EOF | kubectl apply -f -
apiVersion: v1
data:
  MFPF_APPCNTR_DB_ADMIN_USERNAME: encoded_uname
  MFPF_APPCNTR_DB_ADMIN_PASSWORD: encoded_password
kind: Secret
metadata:
name: appcenter-dbadminsecret
type: Opaque
EOF
```
-->
## Backup and recovery of Mobile Foundation Analytics data
{: #backup-recovery-mf-analytics-data}

Mobile Foundation Analytics data is available as part of Kubernetes [PersistentVolume or PersistentVolumeClaim](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#introduction). You may be using one of the [volume plugins that Kubernetes offers](https://kubernetes.io/docs/concepts/storage/volumes/#types-of-volumes).

Backup and restore depends on the volume plugins that you use. There are different tools and mechanisms through which the volume can be backed up or restored.

Kubernetes provides [VolumeSnapshot, VolumeSnapshotContent and Restore](https://kubernetes-csi.github.io/docs/snapshot-restore-feature.html#snapshot--restore-feature) options. You may take a copy of the [volume in the cluster](https://kubernetes.io/docs/concepts/storage/volume-snapshots/#introduction) that has been provisioned by an administrator.

Various [example yaml files](https://github.com/kubernetes-csi/external-snapshotter/tree/master/examples/kubernetes) are available in the Kubernetes documentation to test the snapshot feature.

You may also leverage other tools like [AppsCode Stash](https://appscode.com/products/kubed/0.9.0/guides/disaster-recovery/stash/) to take a backup of the volume and restore the same.

<!--
## Known issues and work arounds
{: #known-issues}

* `https:true` does not work. We are working on this issue.     

* Deleting CRs/CRDs hangs at times. We're investigating the issue. Meanwhile, the work around is to patch the CR/CD as follows and then delete.
  ```bash
  oc patch crd/ibmmf.charts.helm.k8s.io -p '{"metadata":{"finalizers":[]}}' --type=merge
  ```
  {: codeblock}

* If MySQL DB is used as Mobile Foundation database and if it is not created with UTF8 character set encoding
  `dbinit pod` will fail and the pods will not start.
  Use the following command to create the database with the required encoding.
  ```bash
  # DB creation with character set encoding
  CREATE DATABASE CHARACTER SET utf8 COLLATE utf8_general_ci;
  ```
  {: codeblock}

  If the database is already created, use the ALTER command to enforce the character set encoding.
  ```bash
  # DB alter with character set encoding
  ALTER DATABASE CHARACTER SET utf8 COLLATE utf8_general_ci;
  ```
  {: codeblock}
 >Remember to delete the Custom Resource Object and re-apply the configuration.
-->