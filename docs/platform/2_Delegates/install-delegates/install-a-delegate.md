---
title: Install a delegate
description: How to install Harness delegates using Helm, Terraform, Kubernetes, or Docker.
# sidebar_position: 2
---
```mdx-code-block
import install_two from './static/install-a-delegate-02.png'
import install_four from './static/install-a-delegate-04.png'
import install_five from './static/install-a-delegate-05.png'
import install_twoseven from './static/install-a-delegate-27.png'
import install_seven from './static/install-a-delegate-07.png'
import install_eight from './static/install-a-delegate-08.png'
import install_eleven from './static/install-a-delegate-11.png'
import install_twelve from './static/install-a-delegate-12.png'
import install_onethree from './static/install-a-delegate-13.png'
import install_onefive from './static/install-a-delegate-15.png'
import install_onesix from './static/install-a-delegate-16.png'
import install_onenine from './static/install-a-delegate-19.png'
import install_twenty from './static/install-a-delegate-20.png'
import install_twofive from './static/install-a-delegate-25.png'
import install_twothree from './static/install-a-delegate-23.png'
import install_twonine from './static/install-a-delegate-29.png'
import install_30 from './static/install-a-delegate-30.png'
import install_39 from './static/install-a-delegate-39.png'
import install_31 from './static/install-a-delegate-31.png'
import install_33 from './static/install-a-delegate-33.png'
import install_35 from './static/install-a-delegate-35.png'
import install_37 from './static/install-a-delegate-37.png'
import install_40 from './static/install-a-delegate-40.png'
```


This document introduces the delegate installer and installation of Harness delegates in NextGen environments running Kubernetes or Docker. Like the installer, this document includes the workflow for delegate installation by Helm chart, Terraform Plan, and Kubernetes manifest or Docker.

The process of installing a delegate includes the following steps:

- Go to the **New Delegate** page
- Select an environment: **Kubernetes** or **Docker**
- Select the mode of installation: Helm chart, Terraform Plan, or Kubernetes manifest

## Go to the New Delegate page

You can install a delegate from the **New Delegate** installation page.

| 1 <p>Go to **Account Settings**</p> | 2 <p>Select **Account Resources**</p> | 3 <p>Choose **Delegates**</p> |
| :-: | :-: | :-: |
| ![](./static/install-a-delegate-01.png) | ![](./static/install-a-delegate-02.png) | ![](./static/install-a-delegate-03.png) |


In addition to providing basic information about installed delegates, the **Delegates** page gives you access to the delegate installer.

```mdx-code-block
<img src={install_four} width="600" />
```

To install a delegate, click **+ New Delegate**.

```mdx-code-block
<img src={install_five} width="200" />
```

The delegate installation process has changed. The installation process is entirely done from the **New Delegate** page.

```mdx-code-block
<img src={install_twoseven} width="800" />
```

If you prefer a more familiar installation process, click **Switch back to the old delegate install experience**.

```mdx-code-block
<img src={install_seven} width="350" />
```

Otherwise, continue with the following steps.

## Select an environment

Select your target environment: **Kubernetes** or **Docker**.

```mdx-code-block
<img src={install_eight} width="350" />
```

### Kubernetes environment

In **Install your Delegate**, select [**Helm Chart**](#helm-based-install-on-kubernetes), [**Terraform**](#terraform-based-install-on-kubernetes), or [**Kubernetes Manifest**](#kubernetes-based-install-on-kubernetes).

```mdx-code-block
<img src={install_eleven} width="350" />
```

#### Helm-based install on Kubernetes

Use the following steps to install a delegate on Kubernetes using a Helm chart.

On the **New Delegate** page, select **Kubernetes**, and then click **Helm Chart**.

##### Name the delegate

Before you install the delegate, accept or modify the default delegate name.

```mdx-code-block
<img src={install_twelve} width="300" />
```

Delegates are identified by their names. Delegate names must conform to the following guidelines:

- Delegate names must be unique within a namespace and should be unique in your cluster. 
- A valid name includes only lowercase letters and does not start or end with a number. 
- The dash character (“-”) can be used as a separator between letters.

##### Add the repository

Add the Harness Helm chart repository to your local Helm registry. Use the following command:

```
helm repo add harness-delegate https://app.harness.io/storage/harness-download/delegate-helm-chart/
```

##### Update the repository

Use the following command to ensure you retrieve the latest version of the Harness Helm chart:

```
helm repo update harness-delegate
```

##### Install the delegate

Copy and paste the following instructions into your CLI. These instructions are modified based on your account settings and the configuration options selected above. For descriptions of the values, see the table that follows.

```
helm upgrade -i helm-delegate --namespace harness-delegate-ng --create-namespace \
  harness-delegate/harness-delegate-ng \
  --set delegateName=helm-delegate \
  --set accountId=yOGUkZC9THWFWgVA6tSj-g \
  --set delegateToken=NTg4YzNjZDc1NzAyMTE2MzBhMTJlYTI1MDkyYjRjMzg= \
  --set managerEndpoint=https://myserver.io  \
  --set delegateDockerImage=harness/delegate:23.02.78306 \
  --set replicas=1 --set upgrader.enabled=false
```

| **Value** | **Description** |
| :-- | :-- |
| `delegateName` | The name of the delegate. This value identifies the delegate. |
| `accountId` | The account ID for the account with which the delegate is associated. You can find this value in **Account Settings**. |
| `delegateToken` | The value of the delegate token. The token authenticates your delegate to Harness Manager. |
| `managerEndpoint` | The endpoint of Harness Manager in your Harness cluster. | 
| `delegateDockerImage` | The location and version of the Docker image that delivers your delegate |
| `replicas` | The replica pods to be created for the delegate. By default, the installation creates one replica pod. |
| `upgrader.enabled` | Whether the delegate is automatically updated. Automatic update is not compatible with customizations of the delegate image. For more information, see [Delegate auto-upgrade](/docs/platform/delegates/configure-delegates/delegate-auto-update/). |


#### Terraform-based install on Kubernetes

Use the following steps to install a delegate on Kubernetes using a Terraform Plan.

On the **New Delegate** page, select **Kubernetes**, and then click **Terraform**.

```mdx-code-block
<img src={install_onefive} width="350" />
```

##### Name the delegate

Before you install the delegate, accept or modify the default delegate name.

```mdx-code-block
<img src={install_onesix} width="300" />
```

Delegates are identified by their names. Delegate names must conform to the following guidelines:

- Delegate names must be unique within a namespace and should be unique in your cluster. 
- A valid name includes only lowercase letters and does not start or end with a number. 
- The dash character (“-”) can be used as a separator between letters.

##### Create and apply the Terraform Plan

1. Copy the Terraform module definition code from the Create the main.tf file section.

2. Save the code in the main.tf file in some location.

3. Use the following instruction to initialize Terraform:

   ```
   terraform init
   ```

4. Apply the delegate module definition file:

   ```
   terraform apply
   ```
   
#### Kubernetes-based install on Kubernetes 

On the **New Delegate** page, select **Kubernetes**, and then click **Kubernetes Manifest**.

```mdx-code-block
<img src={install_onenine} width="350" />
```

##### Name the delegate

Before you install the delegate, you must give it a name.

```mdx-code-block
<img src={install_twenty} width="300" />
```

Delegates are identified by their names. Delegate names must conform to the following guidelines:

- Delegate names must be unique within a namespace and should be unique in your cluster. 
- A valid name includes only lowercase letters and does not start or end with a number. 
- The dash character (“-”) can be used as a separator between letters.

##### Download the delegate YAML

Use the following cURL instruction to download the Kubernetes YAML file to the target directory for installation:

```
curl -LO https://raw.githubusercontent.com/harness/delegate-kubernetes-manifest/main/harness-delegate.yaml
```

##### Modify the delegate YAML

Open the harness-delegate.yaml file. Find and specify the following placeholder values as described.

| **Value** | **Description** |
| :-- | :-- |
| `PUT_YOUR_DELEGATE_NAME` | The name of the delegate. |
| `PUT_YOUR_ACCOUNT_ID` | Your Harness account ID. |
| `PUT_YOUR_MANAGER_ENDPOINT` | The URL of your cluster. See the following table of Harness clusters and endpoints. |
| `PUT_YOUR_DELEGATE_TOKEN` | Your delegate token. To find it, go to **Account Settings > Account Resources**, select **Delegate**, and then select **Tokens**. For more information on how to add your delegate token to the harness-delegate.yaml file, see [Secure delegates with tokens](/docs/platform/delegates/secure-delegates/secure-delegates-with-tokens/). |

Your Harness Manager endpoint depends on your Harness cluster location. Use the following table to find your Harness Manager endpoint on your Harness cluster.

| **Harness cluster location** | **Harness Manager endpoint** |
| :-- | :-- |
| SaaS prod-1 | https://app.harness.io |
| SaaS prod-2 | https://app.harness.io/gratis |
| SaaS prod-3 | https://app3.harness.io |
| [CDCE Docker](https://developer.harness.io/tutorials/deploy-services/cdce-helm-k8s) | `https://<HARNESS_HOST>` if the Docker delegate is remoted from CDCE or http://host.docker.internal if the Docker delegate is located on the same host as CDCE |
| [CDCE Helm](https://developer.harness.io/tutorials/deploy-services/cdce-helm-k8s) | `http://<HARNESS_HOST>:7143` where `HARNESS_HOST` is the public IP address of the Kubernetes node that runs CDCE Helm. |

##### Install the delegate

Use the `kubectl apply` command to apply the harness-delegate.yaml file:

```
$ kubectl apply -f harness-delegate.yaml
```


### Docker environment

Use the following process to install a delegate on Docker.

On the **New Delegate** page, select **Docker**.

```mdx-code-block
<img src={install_twofive} width="350" />
```

#### Name the delegate

Accept or change the default delegate name of `docker-delegate`.

```mdx-code-block
<img src={install_twothree} width="300" />
```

Delegates are identified by their names. Delegate names must conform to the following guidelines:

- Delegate names must be unique within a namespace and should be unique in your cluster. 
- A valid name includes only lowercase letters and does not start or end with a number. 
- The dash character (“-”) can be used as a separator between letters.

#### Install the delegate

Use the `docker run` command to install the delegate with the specified parameters:

```
docker run --cpus=1 --memory=2g \
  -e DELEGATE_NAME=docker-delegate \
  -e NEXT_GEN="true" \
  -e DELEGATE_TYPE="DOCKER" \
  -e ACCOUNT_ID=XXXXXXXxxxxxxxxx \
  -e DELEGATE_TOKEN=XXXXXXXxxxxxxxxx \
  -e LOG_STREAMING_SERVICE_URL=https://myserver.io/log-service/ \
  -e MANAGER_HOST_AND_PORT=https://myserver.io harness/delegate:23.02.78306
```

Specify the parameters as follows.

| **Value** | **Description** |
| :-- | :-- |
| `DELEGATE_NAME` | The specified name of the delegate. This value identifies the delegate. |
| `NEXT_GEN` | Whether the delegate runs in NextGen or FirstGen Harness. A value of "true" indicates NextGen. |
| `DELEGATE_TYPE` | The type of the delegate, in this case `DOCKER`. |
| `ACCOUNT_ID` | The account ID for the account with which the delegate is associated. You can find this value in **Account Settings**. | 
| `DELEGATE_TOKEN` | The value of the delegate token. The token authenticates your delegate to Harness Manager.  |
| `LOG_STREAMING_SERVICE_URL` | The location of the log service in your Harness cluster. |
| `MANAGER_HOST_AND_PORT` | The endpoint and port number of Harness Manager in your Harness cluster. |

Your Harness Manager endpoint depends on your Harness cluster location. Use the following table to find your Harness Manager endpoint on your Harness cluster.

| **Harness cluster location** | **Harness Manager endpoint** |
| :-- | :-- |
| SaaS prod-1 | https://app.harness.io |
| SaaS prod-2 | https://app.harness.io/gratis |
| SaaS prod-3 | https://app3.harness.io |
| [CDCE Docker](https://developer.harness.io/tutorials/deploy-services/cdce-helm-k8s) | `https://<HARNESS_HOST>` if the Docker delegate is remoted from CDCE or http://host.docker.internal if the Docker delegate is located on the same host as CDCE |
| [CDCE Helm](https://developer.harness.io/tutorials/deploy-services/cdce-helm-k8s) | `http://<HARNESS_HOST>:7143` where `HARNESS_HOST` is the public IP address of the Kubernetes node that runs CDCE Helm. |


### Verify the delegate connection

The delegate installation process ends with delegate registration with Harness Manager. The verification process confirms that the delegate is registered and that the delegate is sending “heartbeats” to Harness Manager. 

```mdx-code-block
<img src={install_onethree} width="500" />
```

To verify the delegate, click **Verify**. Harness Manager listens for the delegate heartbeat.

```mdx-code-block
<img src={install_twonine} width="600" />
```

After the delegate is registered and initialized, a success message is shown.

Click **Done** to close the installer. 

### Troubleshooting

The delegate installer provides troubleshooting information for each installation process. If the delegate cannot be verified, click **Troubleshoot** for steps you can use to resolve the problem. This section includes the same information.

Harness asks for feedback after the troubleshooting steps. You are asked, **Did the delegate come up?** 


```mdx-code-block
<img src={install_40} width="600" />
```

If the steps did not resolve the problem, click **No** and use the form to describe the issue. You'll also find links to Harness Support and to [Harness Documentation](https://developer.harness.io/docs/category/delegates).


#### Troubleshoot Helm

Use the following steps to troubleshoot your installation of the delegate using Helm.

```mdx-code-block
<img src={install_31} width="600" />
```

1. Verify that Helm is correctly installed:

   Check for Helm:
   
   ```
   helm
   ```
   
   And then check for the installed version of Helm:

   ```
   helm version
   ```

   If you receive the message `Error: rendered manifests contain a resource that already exists...`, delete the existing namespace and retry the Helm upgrade command to deploy the delegate.
   
   For further instructions on troubleshooting your Helm installation, go to [Helm troubleshooting guide](https://helm.sh/docs/faq/troubleshooting/).

2. Check the status of the delegate on your cluster:

   ```
   kubectl describe pods -n <namespace>
   ```

3. If the pod did not start, check the delegate logs:

   ```
   kubectl logs -f <harnessDelegateName> -n <namespace>
   ```

   If the state of the delegate pod is `CrashLoopBackOff`, check your allocation of compute resources (CPU and memory) to the cluster. A state of `CrashLoopBackOff` indicates insufficent Kubernetes cluster resources.

4. If the delegate pod is not healthy, use the `kubectl describe` command to get more information:

   ```
   kubectl describe <pod_name> -n <namespace>
   ```


#### Troubleshoot Terraform

Use the following steps to troubleshoot your installation of the delegate using Terraform.


```mdx-code-block
<img src={install_33} width="600" />
```

1. Verify that Terraform is correctly installed:

   ```
   terraform -version
   ```
   
   For further instructions on troubleshooting your installation of Terraform, see the [Terraform troubleshooting guide](https://developer.hashicorp.com/terraform/enterprise/vcs/troubleshooting).

2. Check the status of the delegate on your cluster:

   ```
   kubectl describe pods -n <namespace>
   ```

3. If the pod did not start, check the delegate logs:

   ```
   kubectl logs -f <harnessDelegateName> -n <namespace>
   ```

   If the state of the delegate pod is `CrashLoopBackOff`, check your allocation of compute resources (CPU and memory) to the cluster. A state of `CrashLoopBackOff` indicates insufficent Kubernetes cluster resources.

4. If the delegate pod is not healthy, use the `kubectl describe` command to get more information:

   ```
   kubectl describe <pod_name> -n <namespace>
   ```

#### Troubleshoot Kubernetes

Use the following steps to troubleshoot your installation of the delegate using Kubernetes.

```mdx-code-block
<img src={install_35} width="600" />
```

1. Check the status of the delegate on your cluster:

   ```
   kubectl describe pods -n <namespace>
   ```

2. If the pod did not start, check the delegate logs:

   ```
   kubectl logs -f <harnessDelegateName> -n <namespace>
   ```

   If the state of the delegate pod is `CrashLoopBackOff`, check your allocation of compute resources (CPU and memory) to the cluster. A state of `CrashLoopBackOff` indicates insufficent Kubernetes cluster resources.

3. If the delegate pod is not healthy, use the `kubectl describe` command to get more information:

   ```
   kubectl describe <pod_name> -n <namespace>
   ```


#### Troubleshoot Docker

Use the following steps to troubleshoot your installation of the delegate using Docker:

```mdx-code-block
<img src={install_37} width="600" />
```

1. Check the status of the delegate on your cluster:

   ```
   docker container ls -a
   ```
   
2. If the pod is not running, check the delegate logs:

   ```
   docker container logs <delegatename> -f
   ```
   
3. Restart the delegate container. To stop the container:

   ```
   docker container stop <delegatename>
   ```
   
   To start the container:
   
   ```
   docker container start <delegatename>
   ```
   
4. Make sure the container has sufficient CPU and memory resources. If not, remove the older containers:

   ```
   docker container rm [container id]
   ```




