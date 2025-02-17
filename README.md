# OrganoidAnalysis
 This  MATLAB app is designed to process images for the following research project:

**Not so sweet: Investigating the role of receptor tyrosine kinases in taste homeostasis**

Christina M. Piarowski1,2, Jennifer K. Scott1,2, Courtney Wilson1,2, Heber I. Lara3, Ernesto Salcedo1, Elaine T. Lam4, Peter J. Dempsey5, Jakob von Moltke3, Linda A. Barlow1,2



## Version

Created in MATLAB 2024a.



## Analysis Sequence

The sequence of events should be:

1. Load Image
2. Filter
3. Segment
4. Visualize in 3D
5. Use Merge Button
6. Calculate

## Segmentation Process

1. We Used the Bio-Formats MATLAB toolbox to open each image 
2. Channels were processed separately
3. Each Channel was normalized to the maxium intensity using the **mat2gray** function
4. Filter
   1. From the image dataset, we extracted each channel separately and applied the following filters and morphological operations:
      1. a median filter with the default neighborhood settings using the **medfilt3** function
      2. a gaussian filter with a 5X5X5 filter size using the **imgaussfilt3** filter 
      3. To increase contrast and enhance separation between cells, we created an image marker by applying an Erosion Morphological Operation on the filtered volume using the **imerode** function. For this function, we used a Sphere-shaped Structuring Element with a radius of 3. We then performed  a morphological reconstruction of the filtered volume using the function **imreconstruct**. We inputed into this function the filtered volume and the volume marker created by the **imerode** function.
   2. Segment
      1.  Volumes were segmented using Otsu's method. We used the **graythresh** function to identify the intensity cut-off that would separate the cells from the background.
      2.  This function typically returns a threshold that is too high, so we multiplied this function by a factor of 0.5 or 0.3, depending on the channel.
      3.  We used **imbinarize** to create the binarize function, inputting the volume and the factored threshold value
      4.  All binarized volumes were visually inspected using the OrganoidApp slice viewer to ensure proper segmentation
   3. Analysis
      1. Count: All pixels masked by true values in the binarized images, and the divi

## Cluster Merging

The OrganoidAnalysis tool was designed to automatically identify separate organioid clusters and label them accordingly. 

But sometimes the automation process

you get a lot of clusters and it’s hard to keep track of which small clusters should belong to which large clusters, just by their label number. So, I added an interface on the main app that allows you to point and click to change the label of the cluster. 

 The way that this works is that there are two controls below the channel 1 image. After you segment the volume, you should be able to access these controls. The number spinner is where you set the main label to which you want to add other labels. So, you find the label of one of the large clusters and set the spinner to that label (it will be a number from 1 to 10, or something like that). Then, you scroll to a region of the volume where there are some small clusters. Clicking on the Merge Button allows you to click on these small clusters. Their label will automatically be changed to the label of the large cluster. You must click on the merge button for each cluster you want to merge.

![image-20250216133404890](/Users/ernesto/github/OrganoidAnalysis/assets/image-20250216133404890.png)

![image-20250216133505182](/Users/ernesto/github/OrganoidAnalysis/assets/image-20250216133505182.png)

![image-20250216133728768](/Users/ernesto/github/OrganoidAnalysis/assets/image-20250216133728768.png)

So, for example, in the above case, the green cluster is label 2. So, I set the label spinner to 2 and clicked on the merge button, which then allowed me click on one of small dark red clusters. This then merged the red cluster to the green cluster. I had to click merge button twice in this example to get the small clusters converted. 

 

You really only need to do this for one of the main clusters. Once you have those merged, then it should be pretty easy to figure which small clusters go with which large clusters. 

 

Also, A label of 0 means add to background. This is useful if you want to ignore the analysis of some noise.

 

By the way, you could also accidentally convert the background to one of the cluster labels if you are not careful. But no worries, I also added an undo button that will revert the Label volume back to the previous version. 

## Data Wrangler

The Data Wrangler is used to organize the captured data and save to a csv file. 

![image-20250216133954467](/Users/ernesto/github/OrganoidAnalysis/assets/image-20250216133954467.png)

### Update Clusters

This button just makes sure that you are getting the latest list of cluster labels from the volume. 

 If you can clean up the clusters in the slice viewer, you may not always have to manually select the clusters in the cluster list shown in the DataWrangler. In this case, you can just click on the Calculate all button, and you will get a row for each cluster. 
