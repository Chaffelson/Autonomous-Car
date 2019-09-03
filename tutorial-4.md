# Tutorial 3: AI to the edge

We will use Cloudera Data Platform to have access car data in Hadoop - HDFS for when we work in Cloudera Data Science Workbench (CDSW) and train the Keras CNN model. This access to HDFS will also allow us to save the model into HDFS from CDSW. CDSW will be running on the same ec2 instance as CDH and HDFS, but in a docker container.

Download the source code to train the model to your local computer

~~~bash
wget https://raw.githubusercontent.com/james94/Autonomous-Car/master/documentation/assets/src/hdfs-train.zip -O ~/hdfs-train.zip
~~~

now open an instance of Cloudera Data Science Workbench

- CDSW runs at web address:

 `http://cdsw.<cdp-public-cloud-dns>.nip.io`

Sign in to CDSW and go to Admin > Engines

Add a new Engine Profile with 4vCPU and 20Gb Mem 

Go to Projects, then Select new project and name it CSDV

![cdsw-ui](./documentation/assets/images/tutorial3/cdsw-ui.jpg)

then select a local folder with option `Upload Tar or Gzip` and upload the CSDV project you downloaded earlier:

![new-project-cdsw](./documentation/assets/images/tutorial3/new-project-cdsw.jpg)

Once the folder uploads to CDSW, open a new workbench:

![openwb](./documentation/assets/images/tutorial3/openwb.jpg)

when selecting to open a new workbench ensure that you have an engine configuration with at least 20Gb of RAM or a GPU with 8GB of RAM and Python3

![openwb](./documentation/assets/images/tutorial3/engine.jpg)

when you start the session open `hdfs-train` in the left panel and click on the read me file and ensure the packages listed there are installed on your CDSW engine, you can do this by selecting the lines between the ~~~ rows and using the `Run Lines` command from the menu

![rundep](./documentation/assets/images/tutorial3/run-dep.jpg)

![deps](./documentation/assets/images/tutorial3/deps.jpg)

now we can begin training our model, select the `hdfs-model.py` file on the CDSW file explorer, but before we can run the training script we must ensure that there is data flowing in to HDFS from our CEM cluster

enter the following command on the workbench:

`!hdfs dfs -ls /tmp/data/input/racetrack/image/logitech`

the output should show a few files stored into HDFS. However, if not all of the files you intended to send over are stored into HDFS yet, you should wait because the more data we have the better the model will be.

![dataflowing](./documentation/assets/images/tutorial3/dataflowing.jpg)

continue to wait until all of the files that were stored on the edge device have been moved to HDFS. Once all of the files have been moved and you have installed all of the dependencies you are ready to begin training

Now select the `hdfs-model.py` file and select `Run>Run All` your training should look like this

![training](./documentation/assets/images/tutorial3/training.jpg)

for more details about how the training works check out this [blog](link)

### Back to the Edge

Now that you have a model stored on HDFS we can move it back to the edge to complete the cycle. Navigate to NiFi UI and create a new `GetHDFS` processor and connect it to an **output** port

Update the following processor properties:

**Table 7:** Update **GetHDFS** Properties

| Property  | Value  |
|:---|---:|
| `Hadoop Configuration Resources` | `/etc/hadoop/conf.cloudera.hdfs/core-site.xml` |
| `Directory`  | `/tmp/data/input/racetrack/image/`  |
| `Recurse Subdirectories`  |  `false`  |

your NiFi canvas should look like this

![gethdfs](./documentation/assets/images/tutorial3/gethdfs.jpg)

Now navigate to CEM UI and lay out a new RPG

Add URL NiFi is running on:

**Table 8:** Add new RPG for NiFi's Output Port

| Setting  | Value  |
|:---|---:|
| `URL` | `http://<nifi-public-DNS>:8080/nifi/` |

connect the RPG to a new `PutFile` processor and name it **GetModel**

Connect the new RPG to the processor, then add the NiFi origin input port ID you want to send the csv data:

**Table 9:** Add **Output Port ID** for RPG connection

| Settings  | Value  |
|:---|---:|
| `Source Output Port ID` | `<NiFi-Output-port-ID>` |

once you are finished your flow should look like this

![minifiedge](./documentation/assets/images/tutorial3/minifi-edge.jpg)

now right click oin the `GetModel` processor and select **configure**

**Table 10:** Update **GetModel** Properties

| Property  | Value  |
|:---|---:|
| `Directory`  | `/tmp/fromHDFS/model/`  |
| `Keep Source File`  | `false`  |
| `Create Missing Directories`  |  `true`  |

Next select **Action** and publish the MiNiFi flow. Finally, open the NiFi UI and start the processors to begin the flow of the model back into the edge device.

![saved-model](./documentation/assets/images/tutorial3/saved-model.jpg)

We can simply move the model into the location where ROS is expecting it to be and run CDSV with our new AI.

## Conclusion

We began the Edge to AI journey with raw data and ended with a practical Machine Learning model back into the device which was used to collect the data itself, with automation scripts we may use this model to enable the car to drive autonomously while we stream even more data to HDFS to continue the training process. Further, we may author a flow which deploys the same model across a fleet of autonomous vehicles this vehicles in turn can share the different data that they all gather to improve the model even further. With this tutorial you now know how to build your own Edge to AI cycle we are exited to see what your create with this new knowledge, you can share your experiments and creations on the new [Cloudera Community](link).
