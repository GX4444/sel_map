# RGB Semantic Segmentation with Depth Image Projection
  RGB 语义分割与深度图像投影

Within this folder, there are 5 packages. Three of them are semantic segmentation networks, pre-wrapped to work with our sel_map package. All of them will pull code from their respective git repositories and perform any necessary preparation steps to use the network as well as provide arbitrary wrapper code for usage with the rest of the sel_map project. One is a template package included to simplify wrapping of your own segmentation network of choice, and the last is the library actually used by the main sel_map package to process RGB images through a segmentation network of choice and project those class labels based on the depth image and camera intrinsics.

在这个文件夹中，有5个包。其中三个是语义分割网络（前三个），已经预先封装以便与我们的 sel_map 包一起工作。所有这些包都会从各自的 Git 仓库中提取代码，并执行任何必要的准备步骤，以便使用网络并提供用于与 sel_map 项目其余部分配合使用的任意包装代码。一个包（第四个包）是一个模板包，用于简化包装您选择的语义分割网络，最后一个包是主包 sel_map 实际使用的库，用于通过选择的分割网络处理 RGB 图像，并基于深度图像和相机内参投影那些类别标签。

---

## Segmentation Networks
   分割网络

    Among the included segmentation networks, we include wrappers for:
    在包含的分割网络中，我们包括以下的封装：

    **Pretrained PyTorch models by CSAIL trained on the MIT ADE20K scene parsing dataset** [[1]][[2]]. These are a variety of baseline pretrained models. The original repository can be found at https://github.com/CSAILVision/semantic-segmentation-pytorch, from where we use commit `8f27c9b97d2ca7c6e05333d5766d144bf7d8c31b`, and the pretrained weights which should be added to the `ckpt/` folder can be found at http://sceneparsing.csail.mit.edu/model/pytorch. We chose the `ResNet50dilated + PPM_deepsup` model as our primary model.

    由 CSAIL 预训练的 PyTorch 模型，使用 MIT ADE20K 场景解析数据集训练 [1][2]。这些是各种基线预训练模型。
    原始仓库可以在 https://github.com/CSAILVision/semantic-segmentation-pytorch 找到，我们使用提交 8f27c9b97d2ca7c6e05333d5766d144bf7d8c31b，预训练的权重应添加到 ckpt/ 文件夹中，可以在 http://sceneparsing.csail.mit.edu/model/pytorch 找到。我们选择了 ResNet50dilated + PPM_deepsup 模型作为我们的主要模型。


    **A PyTorch implementation of FastSCNN** [[3]]. The specific implementation of FastSCNN here is from https://github.com/Tramac/Fast-SCNN-pytorch. We use commit `0638517d359ae1664a27dfb2cd1780a40a06c465`.

    FastSCNN 的 PyTorch 实现 [3]。这里的 FastSCNN 的具体实现来自 https://github.com/Tramac/Fast-SCNN-pytorch。我们使用提交 0638517d359ae1664a27dfb2cd1780a40a06c465。


    **PyTorch-Encoding Semantic Segmentation Models by Hang Zhang** [[4]]. This wrapper pulls from https://github.com/zhanghang1989/PyTorch-Encoding, and uses commit `331ecdd5306104614cb414b16fbcd9d1a8d40e1e`. This network also includes an additional compile step, which the wrapper will perform automatically. *Note: If there is a circular import error, make sure you have installed CUDA, since this network does not work on the CPU alone.*

    由张航（Hang Zhang）开发的 PyTorch-Encoding 语义分割模型 [4]。这个封装来自 https://github.com/zhanghang1989/PyTorch-Encoding，使用提交 331ecdd5306104614cb414b16fbcd9d1a8d40e1e。这个网络还包括一个额外的编译步骤，封装会自动执行。注意：如果出现循环导入错误，请确保您已安装 CUDA，因为该网络无法单独在 CPU 上运行。

---

## Wrapper Template

    Please see the included README in the wrapper template package. It includes instructions for how to use it.
    请参阅封装模板包（segmentation_wrapper_template）中包含的 README。它包括如何使用它的说明。
    
    This package includes the camera sensor model, and colorscale classes used for segmentation. It also includes additional scripts to evaluate the speed of a given semseg network as well as to output the segmentation of an image or set of images in a folder. Generally, a `terrain_properties` file and a `semseg_config` file are both used to select a network, set the colorscale, and store the base properties. Examples of these two files can be found in the `sel_map` package under the `config/terrain_properties` and `config/semseg` folders respectively.

    该包包括用于分割的相机传感器模型和色阶类。它还包括一些脚本，用于评估给定语义分割网络的速度，并输出文件夹中单个图像或一组图像的分割结果。通常，terrain_properties 文件和 semseg_config 文件都用于选择网络、设置色阶和存储基本属性。这两个文件的示例可以在 sel_map 包中的 config/terrain_properties 和 config/semseg 文件夹中找到。
    
---

[[1]]  B. Zhou, H. Zhao, X. Puig, T. Xiao, S. Fidler, A. Barriuso, and A. Torralba, “Semantic understanding of scenes through the ade20k dataset,” *International Journal on Computer Vision*, 2018.

[[2]] B. Zhou, H. Zhao, X. Puig, S. Fidler, A. Barriuso, and A. Torralba, “Scene parsing through ade20k dataset,” in *Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition*, 2017.

[[3]] R. P. K. Poudel, S. Liwicki, and R. Cipolla, “Fast-scnn: Fast semantic segmentation network,” in *BMVC*, 2019

[[4]] H. Zhang, K. Dana, J. Shi, Z. Zhang, X. Wang, A. Tyagi, and A. Agrawal, “Context encoding for semantic segmentation,” in *The IEEE Conference on Computer Vision and Pattern Recognition (CVPR)*, June 2018.

[1]: https://arxiv.org/abs/1608.05442
[2]: http://people.csail.mit.edu/bzhou/publication/scene-parse-camera-ready.pdf
[3]: https://arxiv.org/abs/1902.04502
[4]: https://arxiv.org/pdf/1803.08904.pdf
