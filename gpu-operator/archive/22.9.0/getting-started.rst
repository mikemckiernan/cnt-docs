.. Date: July 30 2020
.. Author: pramarao

.. _operator-install-guide-22.9.0:

*****************************************
Getting Started
*****************************************
This document provides instructions, including pre-requisites for getting started with the NVIDIA GPU Operator.

----

Red Hat OpenShift 4
====================

For installing the GPU Operator on clusters with Red Hat OpenShift using RHCOS worker nodes,
follow the :ref:`user guide <openshift-introduction-22.9.0>`.


----

VMware vSphere with Tanzu
=========================

For installing the GPU Operator on VMware vSphere with Tanzu leveraging NVIDIA AI Enterprise,
follow the :ref:`NVIDIA AI Enterprise document <install-gpu-operator-22.9.0-nvaie>`.

----

Google Cloud Anthos
====================

For getting started with NVIDIA GPUs for Google Cloud Anthos, follow the getting started
`document <https://docs.nvidia.com/datacenter/cloud-native/kubernetes/anthos-guide.html>`_.

----

Prerequisites
=============

Before installing the GPU Operator, you should ensure that the Kubernetes cluster meets some prerequisites.

#. Nodes must be configured with a container engine such as Docker CE/EE, ``cri-o``, or ``containerd``. For **docker**, follow the official install
   `instructions <https://docs.docker.com/engine/install/>`_.
#. Node Feature Discovery (NFD) is a dependency for the Operator on each node. By default, NFD master and worker are automatically deployed by the Operator.
   If NFD is already running in the cluster prior to the deployment of the operator, then the Operator can be configured to not to install NFD.
#. For monitoring in Kubernetes 1.13 and 1.14, enable the kubelet ``KubeletPodResources`` `feature <https://kubernetes.io/docs/reference/command-line-tools-reference/feature-gates/>`_
   gate. From Kubernetes 1.15 onwards, its enabled by default.

.. note::

   To enable the ``KubeletPodResources`` feature gate, run the following command: ``echo -e "KUBELET_EXTRA_ARGS=--feature-gates=KubeletPodResources=true" | sudo tee /etc/default/kubelet``

Before installing the GPU Operator on NVIDIA vGPU, ensure the following.

#. The NVIDIA vGPU Host Driver version 12.0 (or later) is pre-installed on all hypervisors hosting NVIDIA vGPU accelerated Kubernetes worker node virtual machines. Please refer to `NVIDIA vGPU Documentation <https://docs.nvidia.com/grid/12.0/index.html>`_ for details.
#. A NVIDIA vGPU License Server is installed and reachable from all Kubernetes worker node virtual machines.
#. A private registry is available to upload the NVIDIA vGPU specific driver container image.
#. Each Kubernetes worker node in the cluster has access to the private registry. Private registry access is usually managed through imagePullSecrets. See the Kubernetes Documentation for more information. The user is required to provide these secrets to the NVIDIA GPU-Operator in the driver section of the values.yaml file.
#. Git and Docker/Podman are required to build the vGPU driver image from source repository and push to local registry.

.. note::

    Uploading the NVIDIA vGPU driver to a publicly available repository or otherwise publicly sharing the driver is a violation of the NVIDIA vGPU EULA.


The rest of this document includes instructions for installing the GPU Operator on supported Linux distributions.

Install Kubernetes
===================
.. Shared content for K8s

Refer to :ref:`install-k8s` for getting started with setting up a Kubernetes cluster.

.. Shared content for the GPU Operator install

.. include:: install-gpu-operator.rst

Running Sample GPU Applications
=================================

CUDA VectorAdd
----------------

In the first example, let's run a simple CUDA sample, which adds two vectors together:

.. code-block:: console

   $ cat << EOF | kubectl create -f -
   apiVersion: v1
   kind: Pod
   metadata:
     name: cuda-vectoradd
   spec:
     restartPolicy: OnFailure
     containers:
     - name: cuda-vectoradd
       image: "nvidia/samples:vectoradd-cuda11.2.1"
       resources:
         limits:
            nvidia.com/gpu: 1
   EOF

The sample should run fairly quickly. If you view the logs of the container:

.. code-block:: console

   [Vector addition of 50000 elements]
   Copy input data from the host memory to the CUDA device
   CUDA kernel launch with 196 blocks of 256 threads
   Copy output data from the CUDA device to the host memory
   Test PASSED
   Done


CUDA FP16 Matrix multiply
----------------------------

In the second example, let's try running a CUDA load generator, which does an FP16 matrix-multiply on the GPU using
the Tensor Cores when available:

.. code-block:: console

   $ cat << EOF | kubectl create -f -
   apiVersion: v1
   kind: Pod
   metadata:
      name: dcgmproftester
   spec:
      restartPolicy: OnFailure
      containers:
      - name: dcgmproftester11
        image: nvidia/samples:dcgmproftester-2.0.10-cuda11.0-ubuntu18.04
        args: ["--no-dcgm-validation", "-t 1004", "-d 30"]
        resources:
          limits:
             nvidia.com/gpu: 1
        securityContext:
          capabilities:
            add: ["SYS_ADMIN"]

   EOF

and then view the logs of the ``dcgmproftester`` pod:

.. code-block:: console

   $ kubectl logs -f dcgmproftester

You should see the FP16 GEMM being run on the GPU:

.. code-block:: console

   Skipping CreateDcgmGroups() since DCGM validation is disabled
   CU_DEVICE_ATTRIBUTE_MAX_THREADS_PER_MULTIPROCESSOR: 1024
   CU_DEVICE_ATTRIBUTE_MULTIPROCESSOR_COUNT: 40
   CU_DEVICE_ATTRIBUTE_MAX_SHARED_MEMORY_PER_MULTIPROCESSOR: 65536
   CU_DEVICE_ATTRIBUTE_COMPUTE_CAPABILITY_MAJOR: 7
   CU_DEVICE_ATTRIBUTE_COMPUTE_CAPABILITY_MINOR: 5
   CU_DEVICE_ATTRIBUTE_GLOBAL_MEMORY_BUS_WIDTH: 256
   CU_DEVICE_ATTRIBUTE_MEMORY_CLOCK_RATE: 5001000
   Max Memory bandwidth: 320064000000 bytes (320.06 GiB)
   CudaInit completed successfully.

   Skipping WatchFields() since DCGM validation is disabled
   TensorEngineActive: generated ???, dcgm 0.000 (26096.4 gflops)
   TensorEngineActive: generated ???, dcgm 0.000 (26344.4 gflops)
   TensorEngineActive: generated ???, dcgm 0.000 (26351.2 gflops)
   TensorEngineActive: generated ???, dcgm 0.000 (26359.9 gflops)
   TensorEngineActive: generated ???, dcgm 0.000 (26750.7 gflops)
   TensorEngineActive: generated ???, dcgm 0.000 (25378.8 gflops)

You will observe that on an NVIDIA T4, this has resulted in ~26 TFLOPS of FP16 GEMM performance.

Jupyter Notebook
------------------

In the next example, let's try running a TensorFlow Jupyter notebook.

First, deploy the pods:

.. code-block:: console

   $ kubectl apply -f https://nvidia.github.io/gpu-operator/notebook-example.yml

Check to determine if the pod has successfully started:

.. code-block:: console

   $ kubectl get pod tf-notebook

.. code-block:: console

   NAMESPACE                NAME                                                              READY   STATUS      RESTARTS   AGE
   default                  tf-notebook                                                       1/1     Running     0          3m45s

Since the example also includes a service, let's obtain the external port at which the notebook is accessible:

.. code-block:: console

   $ kubectl get svc -A

.. code-block:: console

   NAMESPACE                NAME                                                    TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                  AGE
   default                  tf-notebook                                             NodePort    10.106.229.20   <none>        80:30001/TCP             4m41s
   ..

And the token for the Jupyter notebook:

.. code-block:: console

   $ kubectl logs tf-notebook

.. code-block:: console

   [I 21:50:23.188 NotebookApp] Writing notebook server cookie secret to /root/.local/share/jupyter/runtime/notebook_cookie_secret
   [I 21:50:23.390 NotebookApp] Serving notebooks from local directory: /tf
   [I 21:50:23.391 NotebookApp] The Jupyter Notebook is running at:
   [I 21:50:23.391 NotebookApp] http://tf-notebook:8888/?token=3660c9ee9b225458faaf853200bc512ff2206f635ab2b1d9
   [I 21:50:23.391 NotebookApp]  or http://127.0.0.1:8888/?token=3660c9ee9b225458faaf853200bc512ff2206f635ab2b1d9
   [I 21:50:23.391 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
   [C 21:50:23.394 NotebookApp]

      To access the notebook, open this file in a browser:
         file:///root/.local/share/jupyter/runtime/nbserver-1-open.html
      Or copy and paste one of these URLs:
         http://tf-notebook:8888/?token=3660c9ee9b225458faaf853200bc512ff2206f635ab2b1d9
      or http://127.0.0.1:8888/?token=3660c9ee9b225458faaf853200bc512ff2206f635ab2b1d9

The notebook should now be accessible from your browser at this URL: ``http:://<your-machine-ip>:30001/?token=3660c9ee9b225458faaf853200bc512ff2206f635ab2b1d9``

Demo
======

Check out the demo below where we scale GPU nodes in a K8s cluster using the GPU Operator:

.. image:: graphics/gpu-operator-demo.gif
   :width: 1440

GPU Telemetry
==============
To gather GPU telemetry in Kubernetes, the GPU Operator deploys the ``dcgm-exporter``. ``dcgm-exporter``, based
on `DCGM <https://developer.nvidia.com/dcgm>`_ exposes GPU metrics for Prometheus and can be visualized using Grafana. ``dcgm-exporter`` is architected to take advantage of
``KubeletPodResources`` `API <https://kubernetes.io/docs/reference/command-line-tools-reference/feature-gates/>`_ and exposes GPU metrics in a format that can be
scraped by Prometheus.

Custom Metrics Config
---------------------

With GPU Operator users can customize the metrics to be collected by ``dcgm-exporter``. Below are the steps for this

 1. Fetch the metrics file and save as dcgm-metrics.csv

   .. code-block:: console

      $ curl https://raw.githubusercontent.com/NVIDIA/dcgm-exporter/main/etc/dcp-metrics-included.csv > dcgm-metrics.csv

 2. Edit the metrics file as required to add/remove any metrics to be collected.
   
 3. Create a Namespace ``gpu-operator`` if one is not already present.

  .. code-block:: console

     $ kubectl create namespace gpu-operator

 4. Create a ConfigMap using the file edited above.

   .. code-block:: console

      $ kubectl create configmap metrics-config -n gpu-operator --from-file=dcgm-metrics.csv

 5. Install GPU Operator with additional options ``--set dcgmExporter.config.name=metrics-config`` and
    ``--set dcgmExporter.env[0].name=DCGM_EXPORTER_COLLECTORS --set dcgmExporter.env[0].value=/etc/dcgm-exporter/dcgm-metrics.csv``


Collecting Metrics on NVIDIA DGX A100 with DGX OS
----------------------------------------------------

NVIDIA DGX systems running with DGX OS bundles drivers, DCGM, etc. in the system image and have `nv-hostengine` running already.
To avoid any compatibility issues, it is recommended to have `dcgm-exporter` connect to the existing `nv-hostengine` daemon to gather/publish
GPU telemetry data.

.. warning::

   The `dcgm-exporter` container image includes a DCGM client library (``libdcgm.so``) to communicate with
   `nv-hostengine`. In this deployment scenario we have `dcgm-exporter` (or rather ``libdcgm.so``) connect
   to an existing `nv-hostengine` running on the host. The DCGM client library uses an internal protocol to exchange
   information with `nv-hostengine`. To avoid any potential incompatibilities between the container image's DCGM client library
   and the host's `nv-hostengine`, it is strongly recommended to use a version of DCGM on which `dcgm-exporter` is based is
   greater than or equal to (but not less than) the version of DCGM running on the host. This can be easily determined by
   comparing the version tags of the `dcgm-exporter` image and by running ``nv-hostengine --version`` on the host.

In this scenario, we need to set ``DCGM_REMOTE_HOSTENGINE_INFO`` to ``localhost:5555`` for `dcgm-exporter` to connect to `nv-hostengine` running on the host.

.. code-block:: console

   $ kubectl patch clusterpolicy/cluster-policy --type='json' -p='[{"op": "add", "path": "/spec/dcgmExporter/env/-", "value":{"name":"DCGM_REMOTE_HOSTENGINE_INFO", "value":"localhost:5555"}}]'

Verify `dcgm-exporter` pod is running after this change

.. code-block:: console

   $ kubectl get pods -l app=nvidia-dcgm-exporter --all-namespaces


The rest of this section walks through how to setup Prometheus, Grafana using Operators and using Prometheus with ``dcgm-exporter``.

.. Shared content for kube-prometheus

.. include:: /kubernetes/kube-prometheus.rst

Now you can see the Prometheus and Grafana pods:

.. code-block:: console

   $ kubectl get pods -n prometheus

.. code-block:: console

   NAME                                                              READY   STATUS    RESTARTS   AGE
   alertmanager-kube-prometheus-stack-1637-alertmanager-0            2/2     Running   0          23s
   kube-prometheus-stack-1637-operator-7bd6d6455c-pcv6n              1/1     Running   0          25s
   kube-prometheus-stack-1637791640-grafana-f99f499df-kwm4f          2/2     Running   0          25s
   kube-prometheus-stack-1637791640-kube-state-metrics-65bf4526xnl   1/1     Running   0          25s
   kube-prometheus-stack-1637791640-prometheus-node-exporter-8pwc4   1/1     Running   0          25s
   kube-prometheus-stack-1637791640-prometheus-node-exporter-nvzhq   1/1     Running   0          25s
   prometheus-kube-prometheus-stack-1637-prometheus-0                2/2     Running   0          23s

You can view the services setup as part of the operator and ``dcgm-exporter``:

.. code-block:: console

   $ kubectl get svc -A

.. code-block:: console

   NAMESPACE      NAME                                                        TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                        AGE
   default        kubernetes                                                  ClusterIP   10.96.0.1        <none>        443/TCP                        34d
   gpu-operator   gpu-operator                                                ClusterIP   10.106.165.20    <none>        8080/TCP                       29m
   gpu-operator   gpu-operator-node-feature-discovery-master                  ClusterIP   10.102.207.205   <none>        8080/TCP                       30m
   gpu-operator   nvidia-dcgm-exporter                                        ClusterIP   10.108.99.82     <none>        9400/TCP                       29m
   kube-system    kube-dns                                                    ClusterIP   10.96.0.10       <none>        53/UDP,53/TCP,9153/TCP         34d
   kube-system    kube-prometheus-stack-1637-coredns                          ClusterIP   None             <none>        9153/TCP                       56s
   kube-system    kube-prometheus-stack-1637-kube-controller-manager          ClusterIP   None             <none>        10252/TCP                      56s
   kube-system    kube-prometheus-stack-1637-kube-etcd                        ClusterIP   None             <none>        2379/TCP                       56s
   kube-system    kube-prometheus-stack-1637-kube-proxy                       ClusterIP   None             <none>        10249/TCP                      56s
   kube-system    kube-prometheus-stack-1637-kube-scheduler                   ClusterIP   None             <none>        10251/TCP                      56s
   kube-system    kube-prometheus-stack-1637-kubelet                          ClusterIP   None             <none>        10250/TCP,10255/TCP,4194/TCP   6m42s
   prometheus     alertmanager-operated                                       ClusterIP   None             <none>        9093/TCP,9094/TCP,9094/UDP     54s
   prometheus     kube-prometheus-stack-1637-alertmanager                     ClusterIP   10.99.137.105    <none>        9093/TCP                       56s
   prometheus     kube-prometheus-stack-1637-operator                         ClusterIP   10.101.198.43    <none>        443/TCP                        56s
   prometheus     kube-prometheus-stack-1637-prometheus                       NodePort    10.105.175.245   <none>        9090:30090/TCP                 56s
   prometheus     kube-prometheus-stack-1637791640-grafana                    ClusterIP   10.111.115.192   <none>        80/TCP                         56s
   prometheus     kube-prometheus-stack-1637791640-kube-state-metrics         ClusterIP   10.105.66.181    <none>        8080/TCP                       56s
   prometheus     kube-prometheus-stack-1637791640-prometheus-node-exporter   ClusterIP   10.108.72.70     <none>        9100/TCP                       56s
   prometheus     prometheus-operated                                         ClusterIP   None             <none>        9090/TCP                       54s

You can observe that the Prometheus server is available at port 30090 on the node's IP address. Open your browser to ``http://<machine-ip-address>:30090``.
It may take a few minutes for DCGM to start publishing the metrics to Prometheus. The metrics availability can be verified by typing ``DCGM_FI_DEV_GPU_UTIL``
in the event bar to determine if the GPU metrics are visible:

.. image:: ../kubernetes/graphics/dcgm-e2e/001-dcgm-e2e-prom-screenshot.png
   :width: 800

Using Grafana
---------------
You can also launch the Grafana tools for visualizing the GPU metrics.

There are two mechanisms for dealing with the ports on which Grafana is available - the service can be patched or port-forwarding can be used to reach the home page.
Either option can be chosen based on preference.

Patching the Grafana Service
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
By default, Grafana uses a ``ClusterIP`` to expose the ports on which the service is accessible. This can be changed to a ``NodePort`` instead, so the page is accessible
from the browser, similar to the Prometheus dashboard.

You can use `kubectl patch <https://kubernetes.io/docs/tasks/manage-kubernetes-objects/update-api-object-kubectl-patch/>`_ to update the service API
object to expose a ``NodePort`` instead.

First, modify the spec to change the service type:

.. code-block:: console

   $ cat << EOF | tee grafana-patch.yaml
   spec:
     type: NodePort
     nodePort: 32322
   EOF

And now use ``kubectl patch``:

.. code-block:: console

   $ kubectl patch svc prometheus-operator-1637791640-grafana -n prometheus --patch "$(cat grafana-patch.yaml)"

.. code-block:: console

   service/prometheus-operator-1637791640-grafana patched

You can verify that the service is now exposed at an externally accessible port:

.. code-block:: console

   $ kubectl get svc -A

.. code-block:: console

   NAMESPACE     NAME                                                      TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                        AGE
   <snip>
   prometheus    prometheus-operator-1637791640-grafana                    NodePort    10.111.115.192   <none>        80:32258/TCP                   2m2s

Open your browser to ``http://<machine-ip-address>:32258`` and view the Grafana login page. Access Grafana home using the ``admin`` username.
The password credentials for the login are available in the ``prometheus.values`` file we edited in the earlier section of the doc:

.. code-block:: console

   ## Deploy default dashboards.
   ##
   defaultDashboardsEnabled: true

   adminPassword: prom-operator

.. image:: ../kubernetes/graphics/dcgm-e2e/002-dcgm-e2e-grafana-screenshot.png
   :width: 800

.. _operator-upgrades-22.9.0:

Upgrade
===========================

Using Helm
-----------

Starting with GPU Operator v1.8.0, the GPU Operator supports dynamic updates to existing resources. This allows
the GPU Operator to ensure settings from the `ClusterPolicy` Spec are always applied and current.

Since Helm does not `support <https://helm.sh/docs/chart_best_practices/custom_resource_definitions/#some-caveats-and-explanations>`_ auto upgrade of existing CRDs, the user has following options to
upgrade the GPU Operator chart:

Option 1 - manually upgrade CRD
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. blockdiag::

   blockdiag admin {
      A [label = "Update CRD from the latest chart", color = "#00CC00"];
      B [label = "Upgrade via Helm"];

      A -> B;
   }

With this workflow, all existing GPU operator resources are updated inline and the `ClusterPolicy` resource is patched with updates from ``values.yaml``.

Download the CRD from the specific `<release-tag>` from the Git repo. For example:

.. code-block:: console

   $ wget https://gitlab.com/nvidia/kubernetes/gpu-operator/-/raw/<release-tag>/deployments/gpu-operator/crds/nvidia.com_clusterpolicies_crd.yaml

Apply the CRD using the file downloaded above:

.. code-block:: console

   $  kubectl apply -f nvidia.com_clusterpolicies_crd.yaml

Fetch latest values from the chart (replace the ``.x`` below with the desired version)

.. code-block:: console

   $ helm show values nvidia/gpu-operator --version=v22.9.x > values-22.9.x.yaml

Update the values file as needed.

And upgrade via Helm:

.. code-block:: console

   $ helm upgrade gpu-operator -n gpu-operator -f values-22.9.x.yaml

Option 2 - auto upgrade CRD using Helm hook
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Starting with GPU Operator v22.09, a ``pre-upgrade`` Helm `hook <https://helm.sh/docs/topics/charts_hooks/#the-available-hooks>`_ is utilized to automatically upgrade to latest CRD.
A new parameter ``operator.upgradeCRD`` is added to to trigger this hook during GPU Operator upgrade using Helm. This is disabled by default.
This parameter needs to be set using ``--set operator.upgradeCRD=true`` option during upgrade command as below.

Fetch latest values from the chart (replace the ``.x`` below with the desired version)

.. code-block:: console

   $ helm show values nvidia/gpu-operator --version=v22.9.x > values-v22.9.x.yaml

.. code-block:: console

   $ helm upgrade gpu-operator -n gpu-operator --set operator.upgradeCRD=true --disable-openapi-validation -f values-v22.9.x.yaml

.. note::

   * Option ``--disable-openapi-validation`` is required in this case so that Helm will not try to validate if CR instance from the new chart is valid as per old CRD.
     Since CR instance in the Chart is valid for the upgraded CRD, this will be compatible.

   * Helm hooks used with the GPU Operator use the operator image itself. If operator image itself cannot be pulled successfully (either due to network error or an invalid NGC registry secret in case of NVAIE), hooks will fail.
     In this case, chart needs to be deleted using ``--no-hooks`` option to avoid deletion to be hung on hook failures.

Cluster Policy Updates
^^^^^^^^^^^^^^^^^^^^^^^

The GPU Operator also supports dynamic updates to the ``ClusterPolicy`` CustomResource using ``kubectl``:

.. code-block:: console

   $ kubectl edit clusterpolicy

After the edits are complete, Kubernetes will automatically apply the updates to cluster.

Additional Controls for Driver Upgrades
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

While most of the GPU Operator managed daemonset pods can be updated without dependencies, the NVIDIA driver daemonset needs special handling.
This is due to the fact that the driver kernel modules have to be unloaded and loaded again on each driver container restart.
In turn this has certain dependencies:

#. All clients to the GPU driver have to be disabled
#. The current GPU driver kernel modules have to be unloaded
#. The updated driver pods need to start
#. The GPU driver has to be installed and new kernel modules loaded
#. The driver clients disabled initially have to be enabled again

In order to achieve this, a new component called `k8s-driver-manager` is added which will ensure that, all
existing GPU driver clients are disabled and current modules are unloaded. This component is added as an `initContainer`
within the driver daemonset.

.. The diagram below illustrates the functionality of the `k8s-driver-manager`.

Since the `k8s-driver-manager` evicts pods from the node to complete the driver upgrade, users can control the node drain
behavior using environment variables as specified in the GPU Operator Helm chart (see the ``driver.manager.env`` variables):

.. code-block:: yaml

   imagePullPolicy: IfNotPresent
   env:
   - name: ENABLE_AUTO_DRAIN
      value: "true"
   - name: DRAIN_USE_FORCE
      value: "false"
   - name: DRAIN_POD_SELECTOR_LABEL
      value: ""
   - name: DRAIN_TIMEOUT_SECONDS
      value: "0s"
   - name: DRAIN_DELETE_EMPTYDIR_DATA
      value: "false"

* The *DRAIN_POD_SELECTOR_LABEL* env can be used to let `k8s-driver-manager` only evict GPU pods with matching labels from the node.
  This way, CPU only pods will not be affected during driver upgrades.

* *DRAIN_USE_FORCE* needs to be enabled for evicting GPU pods that are not managed by any of the replication controllers (Deployment, Daemonset, StatefulSet, ReplicaSet).

Using OLM in OpenShift
-----------------------

For upgrading the GPU Operator when running in OpenShift, refer to the official documentation on upgrading installed operators:
https://docs.openshift.com/container-platform/4.8/operators/admin/olm-upgrading-operators.html


Uninstall
===========================

To uninstall the operator:

.. code-block:: console

   $ helm delete -n gpu-operator $(helm list -n gpu-operator | grep gpu-operator | awk '{print $1}')

You should now see all the pods being deleted:

.. code-block:: console

   $ kubectl get pods -n gpu-operator

.. code-block:: console

   No resources found.

By default, Helm does not `support <https://helm.sh/docs/chart_best_practices/custom_resource_definitions/#some-caveats-and-explanations>`_ deletion of existing CRDs when the Chart is deleted.
Thus ``clusterpolicy`` CRD will still remain by default.

.. code-block:: console

   $ kubectl get crds -A | grep -i clusterpolicies.nvidia.com

To overcome this, a ``post-delete`` `hook <https://helm.sh/docs/topics/charts_hooks/#the-available-hooks>`_ is used in the GPU Operator to perform the CRD cleanup. A new parameter ``operator.cleanupCRD``
is added to enable this hook. This is disabled by default. This paramter needs to be enabled with ``--set operator.cleanupCRD=true`` during install or upgrade for automatic CRD cleanup to happen on chart deletion.

.. note::

   * After un-install of GPU Operator, the NVIDIA driver modules might still be loaded.
   Either reboot the node or unload them using the following command:

   .. code-block:: console

      $ sudo rmmod nvidia_modeset nvidia_uvm nvidia

   * Helm hooks used with the GPU Operator use the operator image itself. If operator image itself cannot be pulled successfully (either due to network error or an invalid NGC registry secret in case of NVAIE), hooks will fail.
     In this case, chart needs to be deleted using ``--no-hooks`` option to avoid deletion to be hung on hook failures.
