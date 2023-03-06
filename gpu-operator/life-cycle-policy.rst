.. Date: September 25 2022
.. Author: ebohnhorst


NVIDIA GPU Operator Versioning
------------------------------

To understand the NVIDIA GPU Operator life cycle policy, it is important to know how the Operator is versioned.

As of September 2022, the NVIDIA GPU Operator follows calendar versioning.
NVIDIA GPU Operator v22.9.0 is the first release that follows calendar versioning and version 1.11 is the last release that follows the old versioning scheme.

Using version ``22.9.0`` as an example,

* The first two fields in the version are in the format of ``YY.MM`` and represent the major version and also when the Operator was initially released.
  In this example, the Operator was released in September 2022.
  Zero padding is omitted for month to remain compatible with semantic versioning.

* The third segment, ``0`` in the example, represents the dot release.
  Dot releases typically include fixes for bugs or CVEs but can include minor features like support for a new NVIDIA GPU driver.


NVIDIA GPU Operator Life Cycle
------------------------------

The NVIDIA GPU Operator life cycle policy provides a predictable support policy and timeline of when new Operator versions are released.

Starting with version v22.9.0, a new major Operator version will be released every 6 months.
Therefore, the next major release of the NVIDIA GPU Operator will be released in March 2023 and will be named v23.3.0.

Every major release of the Operator, starting with v22.9.0, is maintained for 12 months.
Bug fixes and CVEs are released throughout the 12 months while minor feature updates are only released within the first six months.

This life cycle enables Operator users to use a given version for up to 12 months.
This life cycle also provides users a 6 month period where they can plan the transition to the next major version.

The product lifecycle and versioning are subject to change in the future.

.. note::

    - Upgrades are only supported within a major release or to the next major release.


GPU Operator Component Matrix
-----------------------------

The following table shows the operands and default operand versions that correspond to an Operator version.

When post-release testing confirms support for newer versions of operands, these updates are identified as *recommended updates* to a GPU Operator version.
Refer to :ref:`upgrade` for information about upgrading the Operator.

  .. list-table::
      :header-rows: 1
      :align: center

      * - Release
        - | NVIDIA
          | GPU
          | Driver
        - | NVIDIA Driver
          | Manager for K8s
        - | NVIDIA
          | Container
          | Toolkit
        - | NVIDIA Kubernetes
          | Device Plugin
        - DCGM Exporter
        - | Node Feature
          | Discovery
        - | NVIDIA GPU Feature
          | Discovery for Kubernetes
        - | NVIDIA MIG Manager
          | for Kubernetes
        - DCGM
        - | Validator for
          | NVIDIA GPU Operator
        - | NVIDIA KubeVirt
          | GPU Device Plugin
        - | NVIDIA vGPU
          | Device Manager
        - NVIDIA GDS Driver

      * - v22.9.2
        - | `525.85.12 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-525-85-12/index.html>`__ (recommended),
          | `525.60.13 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-525-60-13/index.html>`__ (default),
          | `515.86.01 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-515-86-01/index.html>`__,
          | `510.108.03 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-510-108-03/index.html>`__,
          | `470.161.03 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-470-161-03/index.html>`__,
          | `450.216.04 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-450-216-04/index.html>`__
        - `v0.6.0 <https://ngc.nvidia.com/catalog/containers/nvidia:cloud-native:k8s-driver-manager>`__
        - `1.11.0 <https://github.com/NVIDIA/nvidia-container-toolkit/releases>`__
        - `0.13.0 <https://github.com/NVIDIA/k8s-device-plugin/releases>`__
        - `3.1.3-3.1.2 <https://github.com/NVIDIA/gpu-monitoring-tools/releases>`__
        -  v0.10.1
        - `0.7.0 <https://github.com/NVIDIA/gpu-feature-discovery/releases>`__
        - `0.5.0 <https://github.com/NVIDIA/mig-parted/tree/master/deployments/gpu-operator>`__
        - | `3.1.6 <https://docs.nvidia.com/datacenter/dcgm/latest/release-notes/changelog.html>`__ (recommended),
          | `3.1.3-1 <https://docs.nvidia.com/datacenter/dcgm/latest/release-notes/changelog.html>`__ (default)
        - v22.9.1
        - `v1.2.1 <https://github.com/NVIDIA/kubevirt-gpu-device-plugin>`__
        - v0.2.0
        - `2.14.13 <https://github.com/NVIDIA/gds-nvidia-fs/releases>`__

      * - v22.9.1
        - | `525.60.13 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-525-60-13/index.html>`__ (default),
          | `515.86.01 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-515-86-01/index.html>`__,
          | `510.108.03 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-510-108-03/index.html>`__,
          | `470.161.03 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-470-161-03/index.html>`__,
          | `450.216.04 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-450-216-04/index.html>`__
        - `v0.5.1 <https://ngc.nvidia.com/catalog/containers/nvidia:cloud-native:k8s-driver-manager>`__
        - `1.11.0 <https://github.com/NVIDIA/nvidia-container-toolkit/releases>`__
        - `0.13.0 <https://github.com/NVIDIA/k8s-device-plugin/releases>`__
        - `3.1.3-3.1.2 <https://github.com/NVIDIA/gpu-monitoring-tools/releases>`__
        -  v0.10.1
        - `0.7.0 <https://github.com/NVIDIA/gpu-feature-discovery/releases>`__
        - `0.5.0 <https://github.com/NVIDIA/mig-parted/tree/master/deployments/gpu-operator>`__
        - `3.1.3-1 <https://docs.nvidia.com/datacenter/dcgm/latest/release-notes/changelog.html>`__
        - v22.9.1
        - `v1.2.1 <https://github.com/NVIDIA/kubevirt-gpu-device-plugin>`__
        - v0.2.0
        - `2.14.13 <https://github.com/NVIDIA/gds-nvidia-fs/releases>`__

      * - v22.9.0
        - | 520.61.05,
          | `515.65.01 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-515-65-01/index.html>`__ (default),
          | `510.85.02 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-510-85-02/index.html>`__,
          | `470.141.03 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-470-141-03/index.html>`__,
          | `450.203.03 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-450-203-03/index.html>`__
        - `v0.4.2 <https://ngc.nvidia.com/catalog/containers/nvidia:cloud-native:k8s-driver-manager>`__
        - `1.11.0 <https://github.com/NVIDIA/nvidia-container-toolkit/releases>`__
        - `0.12.3 <https://github.com/NVIDIA/k8s-device-plugin/releases>`__
        - `3.0.4-3.0.0 <https://github.com/NVIDIA/gpu-monitoring-tools/releases>`__
        -  v0.10.1
        - `0.6.2 <https://github.com/NVIDIA/gpu-feature-discovery/releases>`__
        - `0.5.0 <https://github.com/NVIDIA/mig-parted/tree/master/deployments/gpu-operator>`__
        - `3.0.4-1 <https://docs.nvidia.com/datacenter/dcgm/latest/release-notes/changelog.html>`__
        - v22.9.0
        - `v1.2.1 <https://github.com/NVIDIA/kubevirt-gpu-device-plugin>`__
        - v0.2.0
        - N/A

      * - 1.11
        - | `515.48.07 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-515-48-07/index.html>`__ (default),
          | `510.47.03 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-510-47-03/index.html>`__,
          | `470.129.06 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-470-129-06/index.html>`__,
          | `450.191.01 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-450-191-01/index.html>`__
        - `v0.4.0 <https://ngc.nvidia.com/catalog/containers/nvidia:cloud-native:k8s-driver-manager>`__
        - `1.10.0 <https://github.com/NVIDIA/nvidia-container-toolkit/releases>`__
        - `0.12.2 <https://github.com/NVIDIA/k8s-device-plugin/releases>`__
        - `2.4.5-2.6.7 <https://github.com/NVIDIA/gpu-monitoring-tools/releases>`__
        -  v0.10.1
        - `0.6.1 <https://github.com/NVIDIA/gpu-feature-discovery/releases>`__
        - `0.4.2 <https://github.com/NVIDIA/mig-parted/tree/master/deployments/gpu-operator>`__
        - `2.4.5-1 <https://docs.nvidia.com/datacenter/dcgm/latest/dcgm-release-notes/index.html>`__
        - v1.11.0
        - `v1.1.2 <https://github.com/NVIDIA/kubevirt-gpu-device-plugin>`__
        - v0.1.0
        - N/A

      * - 1.10
        - `510.47.03 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-510-47-03/index.html>`__
        - `v0.3.0 <https://ngc.nvidia.com/catalog/containers/nvidia:cloud-native:k8s-driver-manager>`__
        - `1.9.0 <https://github.com/NVIDIA/nvidia-container-toolkit/releases>`__
        - `0.11.0 <https://github.com/NVIDIA/k8s-device-plugin/releases>`__
        - `2.3.4-2.6.4 <https://github.com/NVIDIA/gpu-monitoring-tools/releases>`__
        - 0.8.2
        - `0.5.0 <https://github.com/NVIDIA/gpu-feature-discovery/releases>`__
        - `0.3.0 <https://github.com/NVIDIA/mig-parted/tree/master/deployments/gpu-operator>`__
        - `2.3.4.1 <https://docs.nvidia.com/datacenter/dcgm/latest/dcgm-release-notes/index.html>`__
        - v1.10.0
        - N/A
        - N/A
        - N/A

      * - 1.9.1
        - `470.82.01 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-470-82-01/index.html>`__
        - `v0.2.0 <https://ngc.nvidia.com/catalog/containers/nvidia:cloud-native:k8s-driver-manager>`__
        - `1.7.2 <https://github.com/NVIDIA/nvidia-container-toolkit/releases>`__
        - `0.10.0 <https://github.com/NVIDIA/k8s-device-plugin/releases>`__
        - `2.3.1-2.6.1 <https://github.com/NVIDIA/gpu-monitoring-tools/releases>`__
        - 0.8.2
        - `0.4.1 <https://github.com/NVIDIA/gpu-feature-discovery/releases>`__
        - `0.2.0 <https://github.com/NVIDIA/mig-parted/tree/master/deployments/gpu-operator>`__
        - `2.3.1 <https://docs.nvidia.com/datacenter/dcgm/latest/dcgm-release-notes/index.html>`__
        - v1.9.1
        - N/A
        - N/A
        - N/A

      * - 1.9.0
        - `470.82.01 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-470-82-01/index.html>`__
        - `v0.2.0 <https://ngc.nvidia.com/catalog/containers/nvidia:cloud-native:k8s-driver-manager>`__
        - `1.7.2 <https://github.com/NVIDIA/nvidia-container-toolkit/releases>`__
        - `0.10.0 <https://github.com/NVIDIA/k8s-device-plugin/releases>`__
        - `2.3.1-2.6.0 <https://github.com/NVIDIA/gpu-monitoring-tools/releases>`__
        - 0.8.2
        - `0.4.1 <https://github.com/NVIDIA/gpu-feature-discovery/releases>`__
        - `0.2.0 <https://github.com/NVIDIA/mig-parted/tree/master/deployments/gpu-operator>`__
        - `2.3.1 <https://docs.nvidia.com/datacenter/dcgm/latest/dcgm-release-notes/index.html>`__
        - v1.9.0
        - N/A
        - N/A
        - N/A

      * - 1.8.2
        - `470.57.02 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-470-57-02/index.html>`__
        - `v0.1.0 <https://ngc.nvidia.com/catalog/containers/nvidia:cloud-native:k8s-driver-manager>`__
        - `1.7.1 <https://github.com/NVIDIA/nvidia-container-toolkit/releases>`__
        - `0.9.0 <https://github.com/NVIDIA/k8s-device-plugin/releases>`__
        - `2.2.9-2.4.0 <https://github.com/NVIDIA/gpu-monitoring-tools/releases>`__
        - 0.8.2
        - `0.4.1 <https://github.com/NVIDIA/gpu-feature-discovery/releases>`__
        - `0.1.3 <https://github.com/NVIDIA/mig-parted/tree/master/deployments/gpu-operator>`__
        - `2.2.3 <https://docs.nvidia.com/datacenter/dcgm/latest/dcgm-release-notes/index.html>`__
        - v1.8.2
        - N/A
        - N/A
        - N/A

      * - 1.8.1
        - `470.57.02 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-470-57-02/index.html>`__
        - `v0.1.0 <https://ngc.nvidia.com/catalog/containers/nvidia:cloud-native:k8s-driver-manager>`__
        - `1.6.0 <https://github.com/NVIDIA/nvidia-container-toolkit/releases>`__
        - `0.9.0 <https://github.com/NVIDIA/k8s-device-plugin/releases>`__
        - `2.2.9-2.4.0 <https://github.com/NVIDIA/gpu-monitoring-tools/releases>`__
        - 0.8.2
        - `0.4.1 <https://github.com/NVIDIA/gpu-feature-discovery/releases>`__
        - `0.1.2 <https://github.com/NVIDIA/mig-parted/tree/master/deployments/gpu-operator>`__
        - `2.2.3 <https://docs.nvidia.com/datacenter/dcgm/latest/dcgm-release-notes/index.html>`__
        - v1.8.1
        - N/A
        - N/A
        - N/A

      * - 1.8.0
        - `470.57.02 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-470-57-02/index.html>`__
        - `v0.1.0 <https://ngc.nvidia.com/catalog/containers/nvidia:cloud-native:k8s-driver-manager>`__
        - `1.6.0 <https://github.com/NVIDIA/nvidia-container-toolkit/releases>`__
        - `0.9.0 <https://github.com/NVIDIA/k8s-device-plugin/releases>`__
        - `2.2.9-2.4.0 <https://github.com/NVIDIA/gpu-monitoring-tools/releases>`__
        - 0.8.2
        - `0.4.1 <https://github.com/NVIDIA/gpu-feature-discovery/releases>`__
        - `0.1.2 <https://github.com/NVIDIA/mig-parted/tree/master/deployments/gpu-operator>`__
        - `2.2.3 <https://docs.nvidia.com/datacenter/dcgm/latest/dcgm-release-notes/index.html>`__
        - v1.8.0
        - N/A
        - N/A
        - N/A

      * - 1.7.1
        - `460.73.01 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-460-73-01/index.html>`__
        - N/A
        - `1.5.0 <https://github.com/NVIDIA/nvidia-container-toolkit/releases>`__
        - `0.9.0 <https://github.com/NVIDIA/k8s-device-plugin/releases>`__
        - `2.1.8-2.4.0-rc.2 <https://github.com/NVIDIA/gpu-monitoring-tools/releases>`__
        - 0.8.2
        - `0.4.1 <https://github.com/NVIDIA/gpu-feature-discovery/releases>`__
        - `0.1.0 <https://github.com/NVIDIA/mig-parted/tree/master/deployments/gpu-operator>`__
        - N/A
        - v1.7.1
        - N/A
        - N/A
        - N/A

      * - 1.7.0
        - `460.73.01 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-460-73-01/index.html>`__
        - N/A
        - `1.5.0 <https://github.com/NVIDIA/nvidia-container-toolkit/releases>`__
        - `0.9.0 <https://github.com/NVIDIA/k8s-device-plugin/releases>`__
        - `2.1.8-2.4.0-rc.2 <https://github.com/NVIDIA/gpu-monitoring-tools/releases>`__
        - 0.6.0
        - `0.4.1 <https://github.com/NVIDIA/gpu-feature-discovery/releases>`__
        - `0.1.0 <https://github.com/NVIDIA/mig-parted/tree/master/deployments/gpu-operator>`__
        - N/A
        - v1.7.0
        - N/A
        - N/A
        - N/A

      * - 1.6.2
        - `460.32.03 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-460-32-03/index.html>`__
        - N/A
        - `1.4.7 <https://github.com/NVIDIA/nvidia-container-toolkit/releases>`__
        - `0.8.2 <https://github.com/NVIDIA/k8s-device-plugin/releases>`__
        - `2.2.0 <https://github.com/NVIDIA/gpu-monitoring-tools/releases>`__
        - 0.6.0
        - `0.4.1 <https://github.com/NVIDIA/gpu-feature-discovery/releases>`__
        - N/A
        - N/A
        - N/A
        - N/A
        - N/A
        - N/A

      * - 1.6.1
        - `460.32.03 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-460-32-03/index.html>`__
        - N/A
        - `1.4.6 <https://github.com/NVIDIA/nvidia-container-toolkit/releases>`__
        - `0.8.2 <https://github.com/NVIDIA/k8s-device-plugin/releases>`__
        - `2.2.0 <https://github.com/NVIDIA/gpu-monitoring-tools/releases>`__
        - 0.6.0
        - `0.4.1 <https://github.com/NVIDIA/gpu-feature-discovery/releases>`__
        - N/A
        - N/A
        - N/A
        - N/A
        - N/A
        - N/A

      * - 1.6.0
        - `460.32.03 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-460-32-03/index.html>`__
        - N/A
        - `1.4.5 <https://github.com/NVIDIA/nvidia-container-toolkit/releases>`__
        - `0.8.2 <https://github.com/NVIDIA/k8s-device-plugin/releases>`__
        - `2.2.0 <https://github.com/NVIDIA/gpu-monitoring-tools/releases>`__
        - 0.6.0
        - `0.4.1 <https://github.com/NVIDIA/gpu-feature-discovery/releases>`__
        - N/A
        - N/A
        - N/A
        - N/A
        - N/A
        - N/A

      * - 1.5.2
        - `450.80.02 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-450-102-04/index.html>`__
        - N/A
        - `1.4.4 <https://github.com/NVIDIA/nvidia-container-toolkit/releases>`__
        - `0.8.1 <https://github.com/NVIDIA/k8s-device-plugin/releases>`__
        - `2.1.2 <https://github.com/NVIDIA/gpu-monitoring-tools/releases>`__
        - 0.6.0
        - `0.4.0 <https://github.com/NVIDIA/gpu-feature-discovery/releases>`__
        - N/A
        - N/A
        - N/A
        - N/A
        - N/A
        - N/A

      * - 1.5.1
        - `450.80.02 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-450-102-04/index.html>`__
        - N/A
        - `1.4.3 <https://github.com/NVIDIA/nvidia-container-toolkit/releases>`__
        - `0.7.3 <https://github.com/NVIDIA/k8s-device-plugin/releases>`__
        - `2.1.2 <https://github.com/NVIDIA/gpu-monitoring-tools/releases>`__
        - 0.6.0
        - `0.3.0 <https://github.com/NVIDIA/gpu-feature-discovery/releases>`__
        - N/A
        - N/A
        - N/A
        - N/A
        - N/A
        - N/A

      * - 1.5.0
        - `450.80.02 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-450-102-04/index.html>`__
        - N/A
        - `1.4.2 <https://github.com/NVIDIA/nvidia-container-toolkit/releases>`__
        - `0.7.3 <https://github.com/NVIDIA/k8s-device-plugin/releases>`__
        - `2.1.2 <https://github.com/NVIDIA/gpu-monitoring-tools/releases>`__
        - 0.6.0
        - `0.3.0 <https://github.com/NVIDIA/gpu-feature-discovery/releases>`__
        - N/A
        - N/A
        - N/A
        - N/A
        - N/A
        - N/A

      * - 1.4.0
        - `450.80.02 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-450-102-04/index.html>`__
        - N/A
        - `1.4.0 <https://github.com/NVIDIA/nvidia-container-toolkit/releases>`__
        - `0.7.1 <https://github.com/NVIDIA/k8s-device-plugin/releases>`__
        - `2.1.2 <https://github.com/NVIDIA/gpu-monitoring-tools/releases>`__
        - 0.6.0
        - `0.2.2 <https://github.com/NVIDIA/gpu-feature-discovery/releases>`__
        - N/A
        - N/A
        - N/A
        - N/A
        - N/A
        - N/A

      * - 1.3.0
        - `450.80.02 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-450-102-04/index.html>`__
        - N/A
        - `1.3.0 <https://github.com/NVIDIA/nvidia-container-toolkit/releases>`__
        - `0.7.0 <https://github.com/NVIDIA/k8s-device-plugin/releases>`__
        - `2.1.0 <https://github.com/NVIDIA/gpu-monitoring-tools/releases>`__
        - 0.6.0
        - `0.2.1 <https://github.com/NVIDIA/gpu-feature-discovery/releases>`__
        - N/A
        - N/A
        - N/A
        - N/A
        - N/A
        - N/A

      * - 1.2.0
        - `450.80.02 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-450-102-04/index.html>`__
        - N/A
        - `1.3.0 <https://github.com/NVIDIA/nvidia-container-toolkit/releases>`__
        - `0.7.0 <https://github.com/NVIDIA/k8s-device-plugin/releases>`__
        - `2.1.0-rc.2 <https://github.com/NVIDIA/gpu-monitoring-tools/releases>`__
        - 0.6.0
        - N/A
        - N/A
        - N/A
        - N/A
        - N/A
        - N/A
        - N/A

      * - 1.1.0
        - `440.64.00 <https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-440-6400/index.html>`__
        - N/A
        - `1.0.5 <https://github.com/NVIDIA/nvidia-container-toolkit/releases>`__
        - `1.0.0-beta4 <https://github.com/NVIDIA/k8s-device-plugin/releases>`__
        - `1.7.2 <https://github.com/NVIDIA/gpu-monitoring-tools/releases>`__
        - 0.5.0
        - N/A
        - N/A
        - N/A
        - N/A
        - N/A
        - N/A
        - N/A

  .. note::

      - Driver version could be different with NVIDIA vGPU, as it depends on the driver
        version downloaded from the `NVIDIA vGPU Software Portal  <https://nvid.nvidia.com/dashboard/#/dashboard>`_.
      - The GPU Operator is supported on all the R450, R470, R510, 515, 520 and 525 NVIDIA datacenter production drivers. For a list of supported
        datacenter drivers versions, visit this `link <https://docs.nvidia.com/datacenter/tesla/drivers/index.html#cuda-drivers>`_.
