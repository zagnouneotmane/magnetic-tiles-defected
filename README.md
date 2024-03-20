# Saliency defect detection of magnetic tiles

See the blog post at https://dvc.org/blog/end-to-end-computer-vision-api-part-1-data-versioning-and-ml-pipelines for a full explanation.

### This project contains two components:
1. training of image segmentation model using DVC pipelines
2. web API that calls the above model on a provided image and responds with a binary segmentation mask

### The dataset is desribed in detail here: 
https://www.researchgate.net/profile/Congying-Qiu/publication/327701995_Saliency_defect_detection_of_magnetic_tiles/links/5b9fd1bd45851574f7d25019/Saliency-defect-detection-of-magnetic-tiles.pdf

<img width="897" alt="Screen Shot 2022-02-23 at 7 50 39 PM" src="https://user-images.githubusercontent.com/17241029/155436071-c16cf286-72bb-41c0-a933-944defd3f873.png">


### The dataset itself is hosted in this repository:
[https://github.com/abin24/Magnetic-tile-defect-datasets.](https://github.com/abin24/Magnetic-tile-defect-datasets.)


## Local development setup

### Prerequisites
- pipenv
### Setup
```bash
pipenv shell
pipenv install
```

### Remote storage

AWS S3 is our remote storage configured in the [.dvc/config](https://github.com/iterative/magnetic-tiles-defect/blob/main/.dvc/config) file. 
You need to edit this file to configure your own. Many different remote storage types are supported including all major cloud providers. For more info see the [docs](https://dvc.org/doc/command-reference/remote/add).

### Model Training
```bash
dvc repro
```

The model will be saved in the `models/` directory.

Here's the DAG of the pipeline:
```
$ dvc dag
                               +----------------+                         
                               | check_packages |                         
                          *****+----------------+*****                    
                     *****        *          **       ******              
               ******          ***             **           *****         
            ***               *                  **              ******   
+-----------+               **                     *                   ***
| data_load |             **                       *                     *
+-----------+           **                         *                     *
           ***        **                           *                     *
              *     **                             *                     *
               **  *                               *                     *
          +------------+                           *                     *
          | data_split |***                        *                     *
          +------------+   *****                   *                     *
                  *             *******            *                     *
                  *                    *****       *                     *
                  *                         ****   *                     *
                  **                          +-------+                ***
                    ****                      | train |          ******   
                        ****                  +-------+     *****         
                            ***              **       ******              
                               ****        **   ******                    
                                   **     *  ***                          
                                  +----------+                            
                                  | evaluate |                            
                                  +----------+
```             

