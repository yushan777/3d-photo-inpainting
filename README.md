Forked from this [repository](https://github.com/vt-vl-lab/3d-photo-inpainting).  

Updated dependencies and code and removed deprecated elements.  

Converts a 2D image into a 3D mesh and then renders a video simulating camera motion around that mesh.

---

# 3D Photography using Context-aware Layered Depth Inpainting

[![Open 3DPhotoInpainting in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1706ToQrkIZshRSJSHvZ1RuCiM__YX3Bz)

### [[Paper](https://arxiv.org/abs/2004.04727)] [[Project Website](https://shihmengli.github.io/3D-Photo-Inpainting/)] [[Google Colab](https://colab.research.google.com/drive/1706ToQrkIZshRSJSHvZ1RuCiM__YX3Bz)]

<p align='center'>
<img src='https://filebox.ece.vt.edu/~jbhuang/project/3DPhoto/3DPhoto_teaser.jpg' width='900'/>
</p>

We propose a method for converting a single RGB-D input image into a 3D photo, i.e., a multi-layer representation for novel view synthesis that contains hallucinated color and depth structures in regions occluded in the original view. We use a Layered Depth Image with explicit pixel connectivity as underlying representation, and present a learning-based inpainting model that iteratively synthesizes new local color-and-depth content into the occluded region in a spatial context-aware manner. The resulting 3D photos can be efficiently rendered with motion parallax using standard graphics engines. We validate the effectiveness of our method on a wide range of challenging everyday scenes and show fewer artifacts when compared with the state-of-the-arts.
<br/>

**3D Photography using Context-aware Layered Depth Inpainting**
<br/>
[Meng-Li Shih](https://shihmengli.github.io/), 
[Shih-Yang Su](https://lemonatsu.github.io/), 
[Johannes Kopf](https://johanneskopf.de/), and
[Jia-Bin Huang](https://filebox.ece.vt.edu/~jbhuang/)
<br/>
In IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 2020.


## Prerequisites

- Linux (tested on Linux Mint 21.2)
- Python 3.10 
- PyTorch 2.7

and the Python dependencies listed in [requirements.txt](requirements.txt)
- To get started, please run the following commands:
    ```bash
    python3 -m venv venv
    source venv/bin/activate
    
    pip install torch==2.7.0+cu126 torchvision==0.22.0+cu126 --extra-index-url https://download.pytorch.org/whl/cu126
    pip install -r requirements.txt
    ```
- Next, please download the model weight using the following command:
    ```bash
    chmod +x download.sh
    ./download.sh
    ```
The `download.sh` script will do the following:
Create a `checkpoints/` directory
Downloads four model files
 - Three (`color-model.pth, depth-model.pth, edge-model.pth`)models go into `checkpoints/` directoru.
 - One (`model.pt`) goes into the `MiDaS/` directory.

Clones the [BoostingMonocularDepth](https://github.com/compphoto/BoostingMonocularDepth.git) repository.

Creates subdirectory `BoostingMonocularDepth/pix2pix/checkpoints/mergemodel/` and downloads 
Downloads Mergenet weights (`latest_net_G.pth`) into the `BoostingMonocularDepth/pix2pix/checkpoints/mergemodel/` directory.
Downloads A MiDaS model (`model-f46da743.pt`) into the `BoostingMonocularDepth/midas/` directory.

## Quick start
Please follow the instructions in this section. 
This should allow to execute our results.
For more detailed instructions, please refer to [`DOCUMENTATION.md`](DOCUMENTATION.md).

## Execute
1. Put ```.jpg``` files (e.g., test.jpg) into the ```image``` folder. 
    - E.g., `image/moon.jpg`
2. Run the following command
    ```bash
    source venv/bin/activate
    python main.py --config argument.yml
    ```
    - Note: The 3D photo generation process usually takes about 2-3 minutes depending on the available computing resources.
3. The results are stored in the following directories:
    - Corresponding depth map estimated by [MiDaS](https://github.com/intel-isl/MiDaS.git) 
        - E.g. ```depth/moon.npy```, ```depth/moon.png```
        - User could edit ```depth/moon.png``` manually. 
            - Remember to set the following two flags as listed below if user wants to use manually edited ```depth/moon.png``` as input for 3D Photo.
                - `depth_format: '.png'`
                - `require_midas: False`
    - Inpainted 3D mesh (Optional: User need to switch on the flag `save_ply`)
        - E.g. ```mesh/moon.ply```
    - Rendered videos with zoom-in motion
        - E.g. ```video/moon_zoom-in.mp4```
    - Rendered videos with swing motion
        - E.g. ```video/moon_swing.mp4```
    - Rendered videos with circle motion
        - E.g. ```video/moon_circle.mp4```         
    - Rendered videos with dolly zoom-in effect
        - E.g. ```video/moon_dolly-zoom-in.mp4```
        - Note: We assume that the object of focus is located at the center of the image.
4. (Optional) If you want to change the default configuration. Please read [`DOCUMENTATION.md`](DOCUMENTATION.md) and modified ```argument.yml```.


## License
This work is licensed under MIT License. See [LICENSE](LICENSE) for details. 

If you find our code/models useful, please consider citing our paper:
```
@inproceedings{Shih3DP20,
  author = {Shih, Meng-Li and Su, Shih-Yang and Kopf, Johannes and Huang, Jia-Bin},
  title = {3D Photography using Context-aware Layered Depth Inpainting},
  booktitle = {IEEE Conference on Computer Vision and Pattern Recognition (CVPR)},
  year = {2020}
}
```

## Acknowledgments
- We thank Pratul Srinivasan for providing clarification of the method [Srinivasan et al. CVPR 2019](https://people.eecs.berkeley.edu/~pratul/publication/mpi_extrapolation/).
- We thank the author of [Zhou et al. 2018](https://people.eecs.berkeley.edu/~tinghuiz/projects/mpi/), [Choi et al. 2019](https://github.com/NVlabs/extreme-view-synth/), [Mildenhall et al. 2019](https://github.com/Fyusion/LLFF), [Srinivasan et al. 2019](https://github.com/google-research/google-research/tree/ac9b04e1dbdac468fda53e798a326fe9124e49fe/mpi_extrapolation), [Wiles et al. 2020](http://www.robots.ox.ac.uk/~ow/synsin.html), [Niklaus et al. 2019](https://github.com/sniklaus/3d-ken-burns) for providing their implementations online.
- Our code builds upon [EdgeConnect](https://github.com/knazeri/edge-connect), [MiDaS](https://github.com/intel-isl/MiDaS.git) and [pytorch-inpainting-with-partial-conv](https://github.com/naoto0804/pytorch-inpainting-with-partial-conv)
