# Ansible role: cdi

This role deploys the CDI controller and if configured will trigger
an image download and import through CDI.  Below are user controlled 
parameters.

## Configuration parameters
* **cdi_image_namespace**: The namespace into which the CDI components should be
  installed.  Default is `golden`
* **action_deploy_cdi**: (Y/N) Indicates that CDI should be deployed. Defaults to Y
* **action_import_cdi_image**: (Y/N) Indicates that the user wants to trigger the downloading and importing
  of an image.  Defaults to N
  * **cdi_endpoint_url**: (Required) http or s3 url where the image will be downloaded from.
  * **cdi_base64encoded_secret_user**: (Optional) base64 encoded user for download, will be
    inserted into a generated secret.
  * **cdi_base64encoded_secret_passwd**: (Optional) base64 encoded passwd for download, will be
    inserted into a generated secret.
  * **cdi_secret_name**: (Optional) Name of your secret if needed and used, default value is `cdi-secret`.
  * **cdi_pvc_name**: (Optional) Name of your pvc, default value is `golden-pvc`.
  * **cdi_pvc_size**: (Optional) Size of PVC request, default value is `10gi`.


## To only deploy the CDI Controller run as follows:

```
ansible-playbook -i inventory -e storage_role=cdi -e action_deploy_cdi=Y -e cdi_image_namespace=golden playbooks/cdi.yml

or

ansible-playbook -i inventory -e storage_role=cdi playbooks/cdi.yml  (just utilize defaults)
```

**Notice**: That only `cdi_image_namespace` and `action_deploy_cdi=Y` is defined and needed and they are automatically supplied by default.

## To deploy the downloader/import trigger for CDI run as follows:

```
ansible-playbook -i inventory -e storage_role=cdi -e action_import_cdi_image=Y -e cdi_image_namespace=golden -e cdi_endpoint_url=http://cloud.centos.org/centos/7/images/CentOS-7-x86_64-GenericCloud.qcow2 playbooks/cdi.yml
```

**Notice**: That for this at a minimum we only need `cdi_endpoint_url` and `action_import_cdi_image=Y`. If you need to use a secret to pass in credentials
then you will need to fill in a few more parameters as described above.
