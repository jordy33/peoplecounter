### People Counter

# Install jetcard
```
https://github.com/NVIDIA-AI-IOT/jetcard
```
User: jetson Password: jetson


# Install jetcam

https://github.com/NVIDIA-AI-IOT/jetcam

# install the basics

```
$ mkdir -p ${HOME}/project
$ cd ${HOME}/project
$ git clone https://github.com/jkjung-avt/jetson_nano.git
$ cd jetson_nano/
$ ./install_basics.sh
```
# install 4Gb swapfile
```
mkdir ~/project
cd ~/project
git clone https://github.com/JetsonHacksNano/installSwapfile 
cd installSwapfile
edit installSwapfile.sh --> change swapfile from 6gbto 4g
./ installSwapfile.sh
```



from jetcam.csi_camera import CSICamera
import ipywidgets
from IPython.display import display
from jetcam.utils import bgr8_to_jpeg

#
camera = CSICamera(width=224, height=224)
image = camera.read()
#
image_widget = ipywidgets.Image(format='jpeg')

image_widget.value = bgr8_to_jpeg(image)

display(image_widget)

camera.running = True

def update_image(change):
    image = change['new']
    image_widget.value = bgr8_to_jpeg(image)
    
camera.observe(update_image, names='value')

#To stop it, unattach the callback with the unobserve method.

camera.unobserve(update_image, names='value')


#Another way to view the image stream
import traitlets

camera_link = traitlets.dlink((camera, 'value'), (image_widget, 'value'), transform=bgr8_to_jpeg)

#You can remove the camera/widget link with the unlink method.

camera_link.unlink()

#... and reconnect it again with link.

camera_link.link()


#Shut down the kernel of this notebook to release the camera resource.

#To do so, shut down the notebook's kernel from the JupyterLab pull-down menu: Kernel->Shutdown Kernel, then restart it with Kernel->Restart Kernel.


If the camera setup appears "stuck" or the images "frozen", follow these steps:

Shut down the notebook kernel as explained above
Open a terminal on the Jetson Nano by clicking the "Terminal" icon on the "Launch" page
Enter the following command in the terminal window: sudo systemctl restart nvargus-daemon with password:dlinano


# People Counter

# install opencv 3.4.0
```
sudo nvpmodel -m 0
sudo jetson_clocks
cd ${HOME}/project/jetson_nano
./install_opencv-3.4.6.sh
```

# Install Dlib
```
sudo pip3 install dlib
# Install imutils
sudo pip3 install imutils
```

```
git clone https://github.com/jordy33/peoplecounter.git
```
# Install scipy
```
sudo pip3 install scipy

```
#GPU graph
```
git clone https://github.com/jetsonhacks/gpuGraphTX
sudo apt-get install python-matplotlib
./gpuGraph.py
```

### People counter



```
python people_counter.py --prototxt mobilenet_ssd/MobileNetSSD_deploy.prototxt --model mobilenet_ssd/MobileNetSSD_deploy.caffemodel --input 'nvarguscamerasrc ! video/x-raw(memory:NVMM), width=1280, height=720, framerate=21/1, format=NV12 ! nvvidconv flip-method=2 ! video/x-raw, width=960, height=616 format=BGRx ! videoconvert ! appsink'
