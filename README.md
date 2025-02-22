# BuildingCharacterization
This repository holds scripts to process building functions and predict building functions based on streetview images by finetuning a tensorflow convolutional neural network.

# How to build a building classifier?

A dataset of buildings, the BAG ([data viewer](https://bagviewer.kadaster.nl/lvbag/bag-viewer/index.html#?geometry.x=121736.29375&geometry.y=487599.39169571&zoomlevel=4) | [get data via WFS](https://geodata.nationaalgeoregister.nl/bag/wfs?request=GetCapabilities)), was used to derive all building centroids and functions in Amsterdam.

__concatenateBuildingFunctions.Rmd__: The building functions were concatenated from multiple text entries to multi-label numerical classes. Functions for buildings are residential, meeting, industry, office, accomodation, education, shop and other.

__downloadStreetviewImages.py__: This script finds streetview images for a list of coordinates and stores images, metadata and information on download proces. The script is modified from a [streetview script](https://github.com/robolyst/streetview/blob/master/streetview/__init__.py) by Adrian Letchford. The streetview API is apparently not maintained by Google, but does still work (knocks on wood). Input list of coordinates is retrieved from a .csv file in ./input folder and output information (metadata, images, download process information) is stored in ./data repository.

__checkImageJPG.py__: Checks if images have correct JPG format

__createDistributionImages.py__: After having the data in the correct format, a random distribution of buildings is appointed to training (60%), validation (20%) and testing (20%) categories. The random distribution is made so that buildings in the same neighbourhood do not get in separate categories. Otherwise training and testing might be biased, because two identical buildings in the same street can both be in training and testing dataset.


__retrainAdj.py__: Re-training a convolutional neural network with premade training, validation and test datasets and to give more outcome statistics. Script was modified from [image retraining script](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/image_retraining/retrain.py) by Tensorflow.
This script has to be run from terminal as:
```
$ python2 ./retrain_adj.py --image_dir /home/david/Documents/streetview-
master/data_valid_resid_any/F30

```

__fixOutcomeStatisticsMeetingOffice.py__: The outcome statistics of meeting and office were fixed, because presence and absence values were turned around.

__calculateStatisticsDistanceAge.py__: Calculates the statistical association between independent variables distance, building age, streetview image age and neighbourhood with the amount of correct predictions

__createBackup.py__: A script that makes a back-up of reports/scripts and stores it on a USB-stick.

Feel free to download, share and use these scripts.

## Document information
* __author__ = "David Swinkels"
* __github__ = "davidswinkels"
* __purpose__ = "Part of MSc thesis Geo-Information Science at Wageningen University"
* __status__ = "Production"
