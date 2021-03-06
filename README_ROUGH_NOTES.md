### This file has ROUGH NOTES , steps i followed , errors logs etc .( REFER with a pinch o salt)  


<p align="center">
    <img src="https://github.com/DigitalCognition-GIS/spaceNet/blob/master/ScreenCaptures_QGIS_SpaceNet_etc/Screenshot%20from%202020-07-05%2017-18-10.png" width= "650px">
</p>

<h1 align="center">SpaceNet - https://spacenet.ai/</h1>

>This repository will contain both code and additional reading material refrences for analytics and psudo-projects done with the SpaceNet Data. **- SpaceNet 2020**
 
> If you are a GIS developer or a GeoSpatial Scientist - kindly feel free to contribute. 


<br/>


### Table of Contents of this repository

- [X] `Intro to AWS Hosted Open Data - SpaceNet` 
- [X] `Download and Preprocess SpaceNet Data` 
- [X] `Building Footprints from SpaceNet Data` 
- [X] `` 

<br/>

### References - Work in Progress

### Install the AWS CLI on Ubuntu  

- Source URL - https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-linux.html  

> $ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"  

```
% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 31.5M  100 31.5M    0     0  4566k      0  0:00:07  0:00:07 --:--:-- 6713k
```

unzip awscliv2.zip   
sudo ./aws/install   


> $ aws --version   
```
aws-cli/2.0.28 Python/3.7.3 Linux/5.3.0-62-generic botocore/2.0.0dev32  
```

- if you dont have already - Create an IAM User for AWS CLI Login   
> Source - https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html   

- First attempt to download the data from AWS , dint go right - i was following the Download code snippet at the Bottom Right of this page -https://spacenet.ai/las-vegas/ ``` AOI 2 – Vegas – Building Extraction Testing ``` ,  as seen below got an Error  , that compressed GNUZip file - tar.gz , is not at that Path.    
Its possible i misunderstood the instructions to downLoad.   


```
aws s3 cp s3://spacenet-dataset/spacenet/SN2_buildings/tarballs/AOI_2_Vegas_test_public.tar.gz .   
 fatal error: An error occurred (404) when calling the HeadObject    
operation: Key "spacenet/SN2_buildings/tarballs/AOI_2_Vegas_test_public.tar.gz" does not exist  

```

- Here onwards i googled a bit and found the right resource - https://spacenetchallenge.github.io/datasets/datasetHomePage.html    
whereupon whence i list out files at AWS S3 - i can see listed the ```.TIF ``` files.
```
(base) dhankar@dhankar-1:~/spaceNet/spaceNet$ aws s3 ls s3://spacenet-dataset/
                           PRE /
                           PRE AOIs/
                           PRE Hosted-Datasets/
                           PRE SpaceNet_Off-Nadir_Dataset/
                           PRE spacenet-model-weights/
                           PRE spacenet-stac/
                           PRE spacenet/
(base) dhankar@dhankar-1:~/spaceNet/spaceNet$ aws s3 ls s3://spacenet-dataset/AOIs/
                           PRE AOI_10_Dar_Es_Salaam/
                           PRE AOI_1_Rio/
                           PRE AOI_2_Vegas/
                           PRE AOI_3_Paris/
                           PRE AOI_4_Shanghai/
                           PRE AOI_5_Khartoum/
                           PRE AOI_6_Atlanta/
                           PRE AOI_7_Moscow/
                           PRE AOI_8_Mumbai/
                           PRE AOI_9_San_Juan/
(base) dhankar@dhankar-1:~/spaceNet/spaceNet$ aws s3 ls s3://spacenet-dataset/AOIs/AOI_2_Vegas/PS-RGB/
2019-08-02 02:17:49      45245 056155973040_01_LAYOUT.JPG
2019-08-02 02:17:49       6584 056155973040_01_README.TXT
2019-08-02 02:17:49       8320 056155973040_01_README.XML
2019-08-02 02:17:49  402656903 15OCT22183656-S2AS_R1C1-056155973040_01_P001.TIF
2019-08-04 03:57:37  536760406 15OCT22183656-S2AS_R1C1-056155973040_01_P001_COG.TIF
2019-08-02 02:17:49  402656903 15OCT22183656-S2AS_R1C2-056155973040_01_P001.TIF

```

- As seen above we have a small segment of the files within the DIRECTORY -    
The ```15OCT22183656-S2AS_R1C1-056155973040_01_P001_COG.TIF``` file which is a COG.TIF file fomat is as defined here - https://www.cogeo.org/   

> A Cloud Optimized GeoTIFF (COG) is a regular GeoTIFF file, aimed at being hosted on a HTTP file server, with an internal organization that enables more efficient workflows on the cloud. It does this by leveraging the ability of clients issuing ​HTTP GET range requests to ask for just the parts of a file they need.   

 

```
$ aws s3 cp s3://spacenet-dataset/AOIs/AOI_2_Vegas/PS-RGB/15OCT22183656-S2AS_R7C7-056155973040_01_P001_COG.TIF /home/dhankar/_dc_all/spaceNet/test_data

download: s3://spacenet-dataset/AOIs/AOI_2_Vegas/PS-RGB/15OCT22183656-S2AS_R7C7-056155973040_01_P001_COG.TIF to test_data/15OCT22183656-S2AS_R7C7-056155973040_01_P001_COG.TIF
```
- This ```15OCT22183656-S2AS_R7C7-056155973040_01_P001_COG.TIF``` is a 252MB File .

<br/>

- Within the Spacenet DataSet Diectory - , we have ```Geotiffs of Panchromatic raster data``` .   
So what exactly are - ``` geotiffs of Panchromatic raster data``` , how can we understand this File format further?    

> The - geotiffs of Panchromatic raster data - mean we have GeoTiFF Raster Data Files which are Band 8 PanChromatic .   
Panchromatic is further defined within these links / reads - ( Work In Progress) 
- ArcGIS - 
- QGIS - 

> The panchromatic data is usually a representative of a range of wavelengths and bands, for example, the thermal infrared or visible, i.e., it combines different colors hence the name “pan” chromatic. This image is more of a combination of blue, green and red data in a measure of the reflectance. Some, however, can also include radiation at a longer wavelength than the red light, called “the near infrared” radiation.    
Source = https://eos.com/panchromatic/ ( EOS.COM ) 



<br/>



Rohit Dhankar - https://www.linkedin.com/in/rohitdhankar/




