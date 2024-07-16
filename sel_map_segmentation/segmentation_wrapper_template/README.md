# Semantic Segmentation Wrapper Template Package for Integration with sel_map.
  语义分割封装模板包用于集成 sel_map

  To help integrate your own semantic segmentation network of choice with our package, we have included this wrapper template package.
  The most important file in this is the `SemsegNetworkWrapper.py` file under `src/segmentation_wrapper_template`.
  The semantic segmentation configuration files under `config/semseg` in the `sel_map` package, used by `sel_map_segmentation` expect this format in order to find these wrappers.

  为了帮助将您选择的语义分割网络集成到我们的包中，我们提供了这个封装模板包。
  其中最重要的文件是 src/segmentation_wrapper_template 下的 SemsegNetworkWrapper.py 文件。
  sel_map 包中的 config/semseg 下的语义分割配置文件使用这种格式来找到这些封装器。

## Using the Template
   使用模板
  You can copy the template as another package if you'd like, or you can modify this package template directly.
  In order to use this as a template, you must specify your desired package name at the tops of `package.xml`, `CMakeLists.txt`, as well as under the src folder.
  That is, you should rename the `src/segmentation_wrapper_template` folder to `src/{desired_package_name}`.
  Additionally, you should rename this parent folder holding the entire template package to `{desired_package_name}` and delete the `CATKIN_IGNORE` file so the package gets included.

  您可以复制模板作为另一个包，或者可以直接修改此包模板。
  为了使用此模板，您必须在 package.xml、CMakeLists.txt 和 src 文件夹下指定所需的包名称。
  也就是说，您应该将 src/segmentation_wrapper_template 文件夹重命名为 src/{desired_package_name}。
  另外，您应该将包含整个模板包的父文件夹重命名为 {desired_package_name} 并删除 CATKIN_IGNORE 文件，以便包被包含在内。
  
  This should result in the following directory structure:
  这将产生以下目录结构：

  {desired_package_name}/
  ├── src/{desired_package_name}/
  │   ├── __init__.py
  │   └── SemsegNetworkWrapper.py
  ├── CMakeLists.txt
  ├── package.xml
  ├── README.md
  └── setup.py

  To make sure that Python can find the wrapper, edit line 4 of `setup.py` to replace `segmentation_wrapper_template` with your `{desired_package_name}`, and if needed to find your code, update `my_network_root` with the root folder name of your network code or remove it.
  After that, you can update `SemsegNetworkWrapper.py`, where in the `__init__` function, you should have it initialize the network based on parameters passed into `model` and `args`.
  In the `runSegmentation` function, you have it semantically segment the image, returning it based on the format specified by `return_numpy` and `one_hot`.
  Further specifics can be found in the `SemsegNetworkWrapper.py` files.

  为了确保 Python 能找到封装器，请编辑 setup.py 的第4行，将 segmentation_wrapper_template 替换为 {desired_package_name}，并根据需要更新 my_network_root 为您的网络代码的根文件夹名称或删除它。
  之后，您可以更新 SemsegNetworkWrapper.py，在 __init__ 函数中，您应该根据传递给 model 和 args 的参数初始化网络。
  在 runSegmentation 函数中，您应该对图像进行语义分割，根据 return_numpy 和 one_hot 指定的格式返回分割结果。
  更多具体信息可以在 SemsegNetworkWrapper.py 文件中找到。


  This are specifically made for the semseg yaml config files to associate as follows:
  这些是专门为语义分割 yaml 配置文件与之关联而制作的，如下所示：

  semseg:
    package: # Your {desired_package_name}
    model: # String that gets passed to __init__ as "model"
    extra_args: # YAML dict that gets passed to __init__ as "args"
    num_labels: # Integer number of classes used for the sel_map_segmentation package
    ongpu_projection: # Boolean flag of which the inverse is passed to runSegmentation as "return_numpy"
    onehot_projection: # Boolean flag which is passed to runSegmentation as "one_hot"
  
  semseg:
    package:              # 您的 {desired_package_name}
    model:                # 传递给 __init__ 作为 "model" 的字符串
    extra_args:           # 传递给 __init__ 作为 "args" 的 YAML 字典
    num_labels:           # 用于 sel_map_segmentation 包的类数
    ongpu_projection:     # 传递给 runSegmentation 作为 "return_numpy" 的布尔标志的反向值
    onehot_projection:    # 传递给 runSegmentation 作为 "one_hot" 的布尔标志

## General Wrapper Class Requirements
   一般封装类要求

  In case you want to make your own wrapper class from scratch, the requirements in general are as follows:
  如果您想从头开始制作自己的封装类，一般要求如下：

  * The class should be importable with `import {package}.SemsegNetworkWrapper`, where `{package}` is the respective entry from the semseg configuration file. This should generally match the ROS package name.
  类应该可以通过 import {package}.SemsegNetworkWrapper 导入，其中 {package} 是语义分割配置文件中的相应条目。通常应匹配 ROS 包名称。

  * The class constructor, or `__init__` function should include `model` and `args` arguments, where their values come from the `model` and `extra_args` entries in the semseg configuration file. These values can be used to specify the specific weights used for a set of architectures or similar.
  类构造函数或 __init__ 函数应包括 model 和 args 参数，它们的值来自语义分割配置文件中的 model 和 extra_args 条目。这些值可用于指定用于一组架构的特定权重或类似用途。

  * There should be a class function `runSegmentation` which takes a PIL Image object along with two additional boolean arguments `return_numpy` and `one_hot` that specify whether or not to return it as a numpy ndarr and specify whether to use one_hot encoding before sending to the mesh.
  应该有一个类函数 runSegmentation，它接受一个 PIL 图像对象以及两个附加的布尔参数 return_numpy 和 one_hot，指定是否将其作为 numpy ndarray 返回以及在发送到网格之前是否使用 one_hot 编码。

  * Inside this class, a member called `device` should be set to something to communicate to the sensor model what device the segmentation is being performed on.
  在这个类中，应该设置一个名为 device 的成员，以便向传感器模型传达正在执行分割的设备。

  As long as you meet these requirements, you can use expect this mapping framework to work with your own custom semantic segmentation network, based on the config yaml files under `config/semseg` in the `sel_map` package.
  只要满足这些要求，您可以使用这些配置 yaml 文件在 sel_map 包下与您自己的自定义语义分割网络一起使用此映射框架。
