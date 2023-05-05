Download Link: https://assignmentchef.com/product/solved-csye7245-lab9-acne-type-classification-pipeline-using-cnn
<br>
This lab demonstrates how to create a training pipeline that aims to identify the type of Acne-Rosacea, by training a model with images scraped from <a href="http://www.dermnet.com/">dermnet.com</a> with a confidence score. The front-end application uses Streamlit to predict using the trained model.

<strong>Orchestration with Apache Airflow</strong>

Airflow is a platform to programmatically author, schedule and monitor workflows.

In Airflow, a DAG – or a Directed Acyclic Graph – is a collection of all the tasks you want to run, organized in a way that reflects their relationships and dependencies. Use Airflow to author workflows as Directed Acyclic Graphs (DAGs) of tasks. The Airflow scheduler executes your tasks on an array of workers while following the specified dependencies. Rich command line utilities make performing complex surgeries on DAGs a snap.

The rich user interface makes it easy to visualize pipelines running in production, monitor progress, and troubleshoot issues when needed. When workflows are defined as code, they become more maintainable, versionable, testable, and collaborative.

<h1><strong>Dataset</strong></h1>

The dataset used for this lab is from <a href="http://www.dermnet.com/">Dermnet</a>. Dermnet is the largest independent photo dermatology source dedicated to online medical education though articles, photos and video. Dermnet provides information on a wide variety of skin conditions through innovative media. The following are the list of skin problems for which photo dictionary is available:

<h1><strong>Experiment Setup</strong></h1>

The following are the prerequisite setup we made for the implementation of lab:

<ol>

 <li><strong> Install the dependencies as outlined in the requirements.txt by running</strong></li>

</ol>

pip install -r requirements.txt

<ol start="2">

 <li><strong> Install Airflow in the virtual environment</strong></li>

</ol>

pip install apache-airflow

<ol start="3">

 <li><strong> Change the bucket name in </strong>s3_uploader/upload_models.py</li>

</ol>

<ol start="4">

 <li><strong> Configure airflow using following commands:</strong></li>

</ol>

– Use the current directory as $AIRFLOW_HOME

export AIRFLOW_HOME=/home/bigdata/Documents/PyCharmProjects/airflow_cnn_pipeline




– Initialize the database




airflow db init




– Create credentials to access airflow server




airflow users create 

–username admin 

–firstname YourName 

–lastname YourLastName 

–role Admin 

–email <a href="/cdn-cgi/l/email-protection#f89d80999588949db89d80999588949dd69b9795"><span class="__cf_email__" data-cfemail="aacfd2cbc7dac6cfeacfd2cbc7dac6cf84c9c5c7">[email protected]</span></a>




– Starting the Webserver to access Airflow server




airflow webserver -D







– Before running the scheduler, make sure your DAG code is within the <strong>dags</strong> folder, in our case train_model.py has our DAG code and hence it should be inside the dags folder. If we have already started the scheduler, get the pid using following command and then kill it using kill -9 &lt;pid&gt;




lsof -i tcp:8080




– Once these configurations are done, start the airflow scheduler




airflow scheduler




<h1><strong>Test Cases</strong></h1>

<ol>

 <li>After we login to the Airflow webserver on <a href="http://localhost:8080/login/">http://localhost:8080/login/</a>, we can see the CNN-Training-Pipeline in the list of DAGs, we can check the graph view to a detailed view of the workflow tasks. We further trigger the workflow to start running the sequenced tasks:</li>

</ol>




Airflow chains all the individual processes (tasks). The pipeline is scheduled to run at a predefined cadence and is constantly retraining the model using scraped data and continuously upload the trained graph and labels to S3




<strong>Task 1: UploadModels</strong>

This task uploads the retrained graph (retrained_graph_v2.pb) and label (retrained_labels.txt) from our system to AWS S3 Bucket mentioned in the bucket_name using boto3 service inside /model folder

<strong>Task 2: ScrapeData</strong>

This task scrapes data from dermnet.com and downloads the scraped images in ScrapedData-Acne-and-Rosacea-Photos directory using BeautifulSoup in get_data() function




<strong>Task 3: Cleanup</strong>

This task cleans all the empty directories in ScrapedData-Acne-and-Rosacea-Photos folder which does have any images in it.

<strong>Task 4: TrainModel</strong>

This task trains the retrained model uploaded in S3 with the newly scraped images from dermnet.com







<strong>Task 5: UploadModelsPostTraining</strong>

This task uploads the newly retrained model with scraped data to back to S3







<h1><strong> </strong></h1>

<h1><strong>Results</strong></h1>

Once the airflow DAG is successfully completed individual tasks are highlighted in dark green color as below:




<strong>Graph View:</strong>




<strong>Tree View:</strong>

<strong> </strong>

We can validate our retrained model by running the streamlit app (<a href="http://localhost:8501">http://localhost:8501</a>) which calculates the confidence score for its acne condition for each new uploaded images based on the retrained model with scraped images




<h1>Lessons Learned</h1>

<ol>

 <li>Learnt how to orchestrate tasks in a pipeline using Apache Airflow</li>

 <li>Crawling the data from web using BeautifulSoup</li>

 <li>Using streamlit app as inference and validating retrained model for confident score for new images</li>

</ol>







<h1>References</h1>

<a href="https://airflow.apache.org/docs/apache-airflow/stable/index.html">https://airflow.apache.org/docs/apache-airflow/stable/index.html</a>

<a href="https://airflow.apache.org/docs/apache-airflow/stable/index.html">https://medium.com/@dustinstansbury/understanding-apache-airflows-key-concepts-a96efed52b1a</a>

<a href="https://airflow.apache.org/docs/apache-airflow/stable/index.html">https://towardsdatascience.com/getting-started-with-airflow-locally-and-remotely-d068df7fcb4</a>











