### Production Environment
IntelliJ + Maven

### Localized LOF
In our project, we firstly implemented localized LOF algorithm using only one Map-Reduce job. We have 3 input parameters in this job. The first one is the input data path, the second one is output data path, the third one is the number of k.

For example, in the run configurations, we set the program arguments as:

```
input_outlier/ output_outlier/ "20"
```

So, in this example, we put the data under the "input_outlier" directory and we export the output to the "output_outlier" directory and we set k as 20.

### Data-Driven Distributed LOF (DDLOF)

According to the paper ***Distributed Local Outier Detection in Big Data***, there are five jobs to implement this algorithm.

They are:

* Skew-aware Partitioning
* Core Partition KNN search
* Support Partition KNN search
* Compute LRD
* compute LOF

Instead of using skew-aware partitioning, we simplified the partitioning part and self-defined the partitioning method based on our customized dataset. By sampling this step which is hard to implement, we can still focus on the Data Driven KNN search. Though we simplified this step, we need to do some preparation in the "CalSuppBound.java" before we implement our DDLOF algorithm.

By running the "CalSuppBound.java", we can calculate every grid's extended distances. The output result will be stored in a file which we will later use in our distributed LOF process. In personal computer, the path of output result is:

```
/Users/daojun/Desktop/mapreduce/SuppBound/SuppBound.txt
```

In the "DDLof.java" file, we have four jobs, they are:

* Core Partition KNN search
* Support Partition KNN search
* Compute LRD
* compute LOF

The output of the first job will be the input of the second job. The output of the second job will be the input of the third job. The output of the third job will be the input of the fourth job. Thus, in the main configuration, we link the four jobs together to run in sequence.

***Note***: We write a java method to return a point's support grid id. It is "findSuppId" method. In the method, we need to read the file that is the output of "CalSuppBound.java". So in order to run "DDLof.java" in your computer, you need to change the path corresponding to the output path of "CalSuppBound.java" in your computer.

In "DDLof.java", we have 6 input parameters. They are the input data path, the output path of the first job, the output path of the second job, the output path of the third job, the output path of the fourth job and k.

For example, we can set the program arguments as:

```
input_outlier/ output_outlier_1/ output_outlier_2/ output_outlier_3/ output_outlier_4/ "20"
```
