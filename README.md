

# B_ONN_SIM (BINARY Optical Neural Network Simulator)

This is a transaction-level, cycle-true python-based simulator for evaluation of Binary optical neural network accelerators for various Binary Neural Network models.  

### Installation and Execution

    git clone https://github.com/Sairam954/B_ONN_SIM.git
    python main.py

### Video Tutorial
[https://drive.google.com/file/d/1zyho10si1KASeOk_zuDW2v8DLXz-D04M/view?usp=share_link](https://youtu.be/X6yifdEB7xU)

### Accelerator Configuration 

The accelerator configuration can be provided in main.py file. The configuration dictionary looks like below:
``` bash
ACCELERATOR = [
{
    ELEMENT_SIZE: 19, # The supported dot product size of the processing unit, generally equal to number of wavelengths multiplexed in weight bank/activation bank 
    ELEMENT_COUNT: 19, # Number of parallel dot products that can be performed by one processing unit, generally equal to the number of output waveguides in a processing unit  
    UNITS_COUNT: 224, # Number of processing unit present in an accelerator
    RECONFIG: [], # Useful if the processing unit element size can be reconfigured according to the convolution computation need
    VDP_TYPE: "AMM", # More information abour vector dot product can be found in our paper ([https://ieeexplore.ieee.org/abstract/document/9852767]
    NAME: "OXBNN_50", # Name of the accelerator 
    ACC_TYPE: "ONNA", # Accelerator Type for example, ANALOG, ONNA, LIGHTBULB, and ROBIN. This parameter helps in evaluation of performance metrics based on accelerator
    PRECISION: 1, # The bit precision supported  by the accelerator, this value along with ***accelerator_required_precision*** determines whether bit-slicing needs to be implemented
    BITRATE: 50, # The bit rate of the accelerator 
}
]
```
### Optical XNOR-Bitcount Based Accelerator
The below image shows OXBNN accelerator processing unit.
![image](https://user-images.githubusercontent.com/23030293/215595337-4279ec37-7486-4e54-8b04-b7e31fa5dce5.png)


### Binary_ONN_Simulator Project Structure 
``` bash
│   constants.py
│   main.py - *Runs the simulator and allows users to change the inputs according to the accelerator* 
│   README.md
│   requirements.txt
│   visualization.py - *Plots the performance metrics like FPS, FPS/W etc of various accelerators on single barplot and also provides information of the best performing accelerator* 
│   __init__.py
│
| *Script to generate model files ->(https://github.com/Sairam954/CNN_Model_Layer_Information_Generator)*
├───CNNModels - *Folder contains various CNN models available for performing simulations. 
│   │   DenseNet121.csv
│   │   GoogLeNet.csv
│   │   Inception_V3.csv
│   │   MobileNet_V2.csv
│   │   ResNet18.csv
│   │   ResNet50.csv
│   │   ShuffleNet_V2.csv
│   │   VGG-small.csv
│   │   VGG16.csv
│   │   VGG19.csv
│   │
│   └───Sample
│           ResNet50.csv
│
├───Controller - *This contains the logic for scheduling the convolutions and corresponding dot product operations on to the accelerator hardware*
│   │   controller.py
│   │   __init__.py
│   │
│   └───__pycache__
│           controller.cpython-310.pyc
│           controller.cpython-38.pyc
│           __init__.cpython-310.pyc
│           __init__.cpython-38.pyc
│
├───Exceptions - *Accelerator Custom Exceptions*
│   │   AcceleratorExceptions.py
│   │
│   └───__pycache__
│           AcceleratorExceptions.cpython-310.pyc
│           AcceleratorExceptions.cpython-38.pyc
│
├───Hardware - *Different classes corresponding to the accelerator*
│   │   Accelerator.py
│   │   Accumulator_TIA.py
│   │   Activation.py
│   │   ADC.py
│   │   Adder.py
│   │   BtoS.py
│   │   bus.py
│   │   DAC.py
│   │   eDram.py
│   │   io_interface.py
│   │   LightBulbVDP.py
│   │   MRR.py
│   │   MRRVDP.py
│   │   PD.py
│   │   Pheripheral.py
│   │   Pool.py
│   │   RobinVDP.py
│   │   router.py
│   │   Serializer.py
│   │   stochastic_MRRVDP.py
│   │   TIA.py
│   │   VCSEL.py
│   │   VDP.py
│   │   vdpelement.py
│   │   __init__.py  
│
└───PerformanceMetrics
│   │   metrics.py - *Class to calculate various peformance metrics like FPS, FPS/W and FPS/W/mm2*
│ 
│
├───Plots - *Folder containing the plots produced by visualization.py*
│   ├───ISQED
│   │       FPS_(Log_Scale).png
│   │
│   └───Sample
├───Result
│   └───ISQED - *Simulation Result of various Binary Neural Network Accelerator*
│           LIGHTBULB_All.csv
│           OXBNN_50_ALL.csv
│           OXBNN_5_ALL.csv
│           ROBIN_EO_All.csv
│           ROBIN_PO_All.csv
│           Vis_Test.csv

```

### Simulation Result CSV:
After the simulations are completed, the results are stored in the form of a csv file containing information as shown below :

![image](https://user-images.githubusercontent.com/23030293/215599495-6df0e14b-3bb4-4bd0-903d-8f9a3c619699.png)

The performance metrics are calculated by using PeformanceMetrics/metrics.py, currently it provides the above values. Users can change the file to reflect their accelerator components energy and power paraments.  

### Evaluation Visualization:
The visualization.py can take the generated simulation csv and plot barplot for the results. It also prints useful information in the console about the top two accelerators. 
![image](https://user-images.githubusercontent.com/23030293/215608379-2d0a6222-b4d0-4891-a08e-9257256aa0a4.png)

Simulation Results Analysis: 
```bash
The accelerator OXBNN_50 achieves 4.158922762163461x times better fps than LIGHTBULB
The accelerator OXBNN_50 achieves 8.384277528829282x times better fps than ROBIN_PO
The accelerator OXBNN_50 achieves 2.547075858401049x times better fps than OXBNN_5
The accelerator OXBNN_50 achieves 1.0x times better fps than OXBNN_50
Details of second best accelerator
The accelerator OXBNN_5 achieves 1.632822496607645x times better fps than LIGHTBULB
The accelerator OXBNN_5 achieves 3.29172666812232x times better fps than ROBIN_PO
The accelerator OXBNN_5 achieves 1.0x times better fps than OXBNN_5

```

### Bibtex
```bash
@INPROCEEDINGS{OXBNN23,
  author =       {Sairam Sri Vatsavai, Venkata Sai Praneeth Karempudi, and Ishan Thakkar},
  title =        {An Optical XNOR-Bitcount Based Accelerator for
Efficient Inference of Binary Neural Networks},
  booktitle =    {2023  International Symposium on Quality Electronic Design (ISQED)}, 
  year =         {2023},
  volume =       {},
  number =       {},
  pages =        {},
}
```




