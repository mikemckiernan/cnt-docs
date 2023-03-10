.. Date: July 30 2020
.. Author: pramarao

*****************************************
Overview
*****************************************

.. image:: graphics/nvidia-gpu-operator-image.jpg
   :width: 600

Kubernetes provides access to special hardware resources such as NVIDIA GPUs, NICs, Infiniband adapters and other devices 
through the `device plugin framework <https://kubernetes.io/docs/concepts/extend-kubernetes/compute-storage-net/device-plugins/>`_. 
However, configuring and managing nodes with these hardware resources requires 
configuration of multiple software components such as drivers, container runtimes or other libraries which are difficult 
and prone to errors. The NVIDIA GPU Operator uses the `operator framework <https://coreos.com/blog/introducing-operator-framework>`_ 
within Kubernetes to automate the management of all NVIDIA software components needed to provision GPU. These components include the NVIDIA drivers (to enable CUDA), 
Kubernetes device plugin for GPUs, the `NVIDIA Container Toolkit <https://github.com/NVIDIA/nvidia-docker>`_, 
automatic node labelling using `GFD <https://github.com/NVIDIA/gpu-feature-discovery>`_, `DCGM <https://developer.nvidia.com/dcgm>`_ based monitoring and others.

----

Documentation
==============

Browse through the following documents for getting started, platform support and release notes.

Getting Started
---------------

The :ref:`operator-install-guide-1.9.1` guide includes information on installing the GPU Operator in a Kubernetes cluster.

Release Notes
---------------

Refer to :ref:`operator-release-notes-1.9.1` for information about releases.

Platform Support
------------------

The :ref:`operator-platform-support-1.9.1` describes the supported platform configurations.

Licenses and Contributing
=========================

The NVIDIA GPU Operator is is licensed under `Apache 2.0 <https://www.apache.org/licenses/LICENSE-2.0>`_ and 
contributions are accepted with a DCO. See the `contributing <https://github.com/NVIDIA/gpu-operator/blob/master/CONTRIBUTING.md>`_ document for 
more information on how to contribute and the release artifacts.

The NVIDIA GPU Operator includes components governed by the following NVIDIA End User License Agreements. By installing and using the GPU Operator, 
you accept the terms and conditions of these licenses. 

* NVIDIA Driver 
  The license for the NVIDIA datacenter drivers is available at this `link <https://www.nvidia.com/content/DriverDownload-March2009/licence.php?lang=us>`_.

* NVIDIA Data Center GPU Manager (DCGM)
  The license for the NVIDIA DCGM is available on the product `page <https://www.developer.vidia.com/dcgm>`_.


Since the underlying images may include components licensed under open-source licenses such as GPL, 
the sources for these components are archived on the CUDA opensource `index <https://developer.download.nvidia.com/compute/cuda/opensource/>`_.

