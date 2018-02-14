# ProActive Examples

This repository includes a sub directory per bucket, in which a metadata file centralizes object-related-information: bucket name, object name, object kind,..
If the "studio_template" section is specified, the object/workflow is also considered as a studio template.

The aim of this project is to centralize all proactive workflows and other related objects (scripts, images, etc). The workflows from the ProActive Examples project are pushed to Catalog storage inside proactive.

# How to build
Please run next command: ``gradlew clean zip``
This will generate the `proactive-examples.zip` file inside project's build folder.

# How to test locally
Copy the genarated proactive-examples.zip file to your `Scheduler_Distribution_Path/samples` directory.
Start your proactive distribution. From this point everything should work ok.
During scheduling startup: the proactive-examples.zip archive will be extracted to proactive-examples folder. On the next step the special groovy script will automatically push the workflows from proactive-examples folder to Catalog storage.
If you need to retest the extracting and loading of proactive-examples, please remove `proactive-examples folder`. Also to test the filling of catalog storage don't forget to clean database.

## The example of exact commands to test locally on linux:
```
1) Scheduler_Distribution_Path is the path to your local Proactive distribution folder. You need to `cd` to this folder.
2) rm -fr samples/proactive-examples*
3) rm -fr data/*
4) you need to `cd` into your locally cloned proactive-examples project folder
5) ./gradlew clean zip
6) cp build/proactive-examples.zip Scheduler_Distribution_Path/samples/
7) go back to Scheduler_Distribution_Path and start proactive-server
8) ./Scheduler_Distribution_Path/bin/proactive-server
```

# How to add a new package

1) Create a folder with the desired package name (e.g. **TextAnalysis**).

2) Add a `METADATA.json` file into the package (e.g. **TextAnalysis/METADATA.json**).

3) Insert the following JSON structure into the `METADATA.json` file:
```
{
	"metadata": {
		"slug": "textanalysis",
		"name": "Text Analysis",
		"short_description": "Text analysis with machine learning and deep learning on Docker",
		"author": "ActiveEon's Team",
		"tags": ["Samples", "Machine Learning", "Text Analysis", "Big Data", "Analytics", "Deep Learning"],
		"version": "1.0"
	},
	"catalog" : {
		"bucket" : "machine-learning",
		"objects" : [
			{
				"name" : "text_analysis",
				"metadata" : {
					"kind": "workflow",
					"commitMessage": "First commit",
					"contentType": "application/xml"
				},
				"file" : "resources/catalog/text_analysis.xml"
			}
		]
	}
}
```

3.1) Update the `metadata` fields:

* * metadata->**slug** - compact name of the package.
* * metadata->**name** - name of the package.
* * metadata->**short_description** - short description of the package.
* * metadata->**tags** - key works of the package.

3.2) Update the `catalog` fields:

* * catalog->**bucket** - Set the name of the bucket(s) for this package. A package can be installed in one or multiple buckets. Therefore, the bucket name can be a String value as in the example above (i.e `"Machine_Learning"`) or a JSON array value as in `["Bucket_1","Bucket_2","Bucket_X"]`.
* * catalog->**objects** - Add an object of each workflow of the package.

An example of a catalog object that represents a workflow:
```
{
				"name" : "text_analysis",
				"metadata" : {
					"kind": "workflow",
					"commitMessage": "First commit",
					"contentType": "application/xml"
				},
				"file" : "resources/catalog/text_analysis.xml"
			}
```
* * object->**name** - Name of the workflow.
* * object->**file** - Relative path of the XML file of the workflow.

4) Add the XML file(s) of the workflow(s) into `resources/catalog/` inside your package folder (e.g. `TextAnalysis/resources/catalog/text_analysis.xml`).

5) By default all new buckets will be added after all existing buckets inside catalog. So no need by default to add bucket name to `ordered_bucket_list` file.

But if you need to have strict order of buckets, then please update `ordered_bucket_list` by adding the package name (order by name). The whole list should be stored as 1 line without any spaces or end line character.

6) Update `build.gradle` by finding `task zip (type: Zip)` function and adding an **include** for your package (e.g. ``include 'TextAnalysis/**'``). Example:
```
task zip (type: Zip){
    archiveName="proactive-examples.zip"
    destinationDir = file('build/')
    from '.'
    include 'AWS/**'
    include 'Clearwater/**'
    include 'CloudAutomationTemplate/**'
    include 'Cron/**'
    include 'DockerBasics/**'
    include 'DockerSwarm/**'
    include 'Email/**'
    include 'FileFolderManagement/**'
    include 'FinanceMonteCarlo/**'
    include 'GetStarted/**'
    include 'HDFS/**'
    include 'HDFSOrchestration/**'
    include 'ImageAnalysis/**'
    include 'JobAnalysis/**'
    include 'LogAnalysis/**'
    include 'MLBasics/**'
    include 'MLNodeSource/**'
    include 'OpenStack/**'
    include 'RemoteVisualization/**'
    include 'Spark/**'
    include 'SparkOrchestration/**'
    include 'Storm/**'
    include 'TextAnalysis/**'
    include 'TriggerTemplate/**'
    include 'TwitterApi/**'
    include 'WebNotification/**'
    include 'ordered_bucket_list'
}
```

That's all!
