<a id="compute-image-console-guide"></a>
## Compute > Image > Console Guide

<a id="create-image"></a>
## Create Image

Images can be created from the root block storage of an instance. For t2, m2, c2, r2, and x1 type instances, excluding u2 type instances, images can be created while running, but data consistency is not guaranteed. For u2 type instances, images can only be created when the instance is stopped.

Before creating an image of a Linux instance, it is recommended to initialize machine-id to prevent duplication. For detailed instructions on initializing machine-id, see [Guide for Initialization of Linux machine-id](#guide-for-initialization-of-linux-machine-id).

To create an image of a Windows instance, it is recommended to prepare for image creation using Sysprep and then stop the instance. For detailed instructions on using sysprep, see [Windows Sysprep Guide](#windows-sysprep).

When creating an image from a running Windows instance, if the Windows instance was created from an image distributed before 2019. 05. 28., preliminary steps are required for accurate operation. The Windows version of the image used to create the instance can be found under **Image Name** in the **Instance Details**. For more details, see [Guide to Creating Images from Running Windows Instances](#guide-to-creating-images-from-running-windows-instances).

> [Caution]
> The size of the created image may be larger than the actual usage of the root block storage.

<a id="modify-image"></a>
## Modify Image

On the **Compute > Image** service page, select the desired image and click **Modify Image** to modify the image. The items that can be modified are as follows.

* **Image Name**: The name information of the image.
* **Description**: The description information of the image.
* **OS Category**: The OS type information of the image. OS category cannot be changed.
* **OS Version**: The OS version information of the image.
* **Maximum CPU**: The maximum number of CPU cores when creating an instance with this image. If not entered, the maximum CPU core limit is not applied.
* **Minimum CPU**: The minimum number of CPU cores when creating an instance with this image. If not entered, the minimum CPU core limit is not applied.
* **Minimum Memory**: The minimum RAM size when creating an instance with this image. The default unit is **MB**. The minimum memory value must be entered.
* **Minimum Block Storage**: The minimum disk size when creating an instance with this image. The default unit is **GB**. The minimum block storage value must be entered.
* **Whether to Use Image Creation Feature**: Information on whether to allow creating other images from this image.
* **Whether to Use Image Download Feature**: Information on whether to allow users to directly download the image file.
* **Whether to Use User Script Feature**: Information on whether to use the user script feature when creating an instance with this image.
* **Target Services**: Information about the services where this image will be exposed. This item only determines whether the image is exposed in the corresponding service, and does not guarantee normal operation.