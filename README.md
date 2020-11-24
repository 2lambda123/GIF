# GIF: Generative Interpretable Faces
This is the official inmplentation fo the paper GIF: Generative Interpretable Faces - https://arxiv.org/abs/2009.00149.
GIF is a photorealistic generative face model with explicit 3D geometric and photometric control.
* __Key words:__ _Generative Interpretable Faces, conditional generative models, 3D conditioning of GANs, explicit 3D control of photorealistic faces, Photorealistic faces._
### Project page
https://gif.is.tue.mpg.de/

### Citation
If you find our work useful in your project please cite us as 
```python
@inproceedings{GIF2020,
    title = {{GIF}: Generative Interpretable Faces},
    author = {Ghosh, Partha and Gupta, Pravir Singh and Uziel, Roy and Ranjan, Anurag and Black, Michael J. and Bolkart, Timo},
    booktitle = {International Conference on 3D Vision (3DV)},
    year = {2020},
    url = {http://gif.is.tue.mpg.de/}
}
```
#### Installation
* `python3 -m venv ~/.venv/gif`
* `source ~/.venv/gif/bin/activate`
* `pip install -r requirements.txt`

## First thing first
Before Running any program you will need to download a few resource files and create a suitable placeholder for the training atifacts to be stored.
 0. you can use this link to do so <# files to appear soon>
 1. Clone with all the submodules e.g. `git clone --recurse-submodules git@github.com:ParthaEth/GIF.git`
 2. Unzip the downloaded `GIF_resources.zip` file in a suitable location. Rmember that the model atifacts can easily become a few 10s of terabytes.
 3. Now you need to provide the path to this directory in the `constants.py` script.
 4. Edit the line `resources_root = '/path/to/the/unzipped/location/of/GIF_resources'`
 5. Modify any other paths as you need
 6. Since we are not allowed to redistribute FLAME we ask you to download it form here - https://flame.is.tue.mpg.de/
 7. Please make sure to dowload 2020 version. After signing in you sould be able to download `FLAME 2020`
 8. Please place the `generic_model.pkl` file in `GIF_resources/input_files/DECA_inferred/data`

#### Preparing training data
To train GIF you will need to prepare two lmdb datasets
1. An LMDB datset containing FFHQ images in different scales
    1. To prepare this `cd prepare_lmdb`
    2. run `python prepare_ffhq_multiscale_dataset.py --n_worker N_WORKER DATASET_PATH`
    3. Here `DATASET_PATH` is the parth to the directory that contains the FFHQ images
2. An LMDB dataset containing renderings of the FLAME model
    1. To create this simply run `python create_deca_rendered_lmdb.py`
    
#### Training
* To resume training from a checkpoint run
`python train.py --run_id <runid> --ckpt /path/to/saved.mdl/file/<runid>/026000_1.model`

* To start training from scratch run 
`python train.py --run_id <runid>`

Note that the training code will take all available GPUs in the system and perform data parallelization. you can set visible GPUs by etting the `CUDA_VISIBLE_DEVICES` environment variable. Run `CUDA_VISIBLE_DEVICES=0,1 python train.py --run_id <runid>` to run on GPU 0 and 1 

#### To run random face generation follow the following steps
1. Clone this repo
2. Download the pretrained model. Please note that you have to download the model with correct run_id 
3. activate your virtual environment
4. `cd plots`
5. `python generate_random_samples.py`
6. Remember to uncomment the appropriate run_id

#### To generate figure 3
1. `cd plots`
2. `python role_of_different_parameters.py`
it will generate `batch_size` number of directories in `f'{cnst.output_root}sample/'` named `gen_iamges<batch_idx>`. Each of these directory contain a column of images as shown in figure 3 in the paper.

#### Amazon mechanical turk (AMT) evaluations:
__Disclaimer: This section can be outdated and/or have changed since the time of writing the document. It is neither intended to advertise nor recommend any particular 3rd party product. The inclusion of this guide is solely for quick reference purposes and is provided without any liability.__
* you will need 3 accounts
1. Mturk - https://requester.mturk.com/
2. Mturk sandbox - just for experiments (No real money involved) https://requestersandbox.mturk.com/
3. AWS - for uploading the images https://aws.amazon.com/

* once that is done you may follow the following steps
4. Upload the images to S3 in AWS website (into 2 different folders. e.g. model1, model2)
5. Make the files public. (You can verify it by clicking one file, and try to view the image using the link)
6. Create one project (not so sure what are the optimal values there, I believe that people at MPI have experience with that
7. In “Design Layout” insert the html code from `mturk/mturk_layout.html` or write your own layout
8. Finally you will have to upload a CSV file which will have s3 or any other public links for images that will be shown to the participants
9. You can generate such links using the `generate_csv.py` or the `create_csv.py` scripts
10. Finally follow an AMT tutorial to deploye and obtain the results
11. You may use the `plot_results.py` or `plot_histogram_results.py` script to visualize AMT results

#### Running the neive vector conditioning model
* Code to run vector conditioning to arrvie soon on a different branch:-)

## Acknowledgements
We thank H. Feng for prepraring the training data, Y. Feng and S. Sanyal for support with the rendering and projection pipeline, and C. Köhler, A. Chandrasekaran, M. Keller, M. Landry, C. Huang, A. Osman and D. Tzionas for fruitful discussions, advice and proofreading. 
We specially thank Taylor McConnell for voicing over out video. The work was partially supported by the International Max Planck Research School for Intelligent Systems (IMPRS-IS).