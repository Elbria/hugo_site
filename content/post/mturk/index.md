---
title: 'Amazon Mechanical Turk Tutorial '
subtitle: 'Setting up an evaluation task with qualification test from scratch :rocket:'
summary: Setting up an Amazon Mechanical Turk evaluation with Qualification Test from scratch
authors:
- admin
date: "2020-01-19T00:00:00Z"
lastmod: "2020-01-19T00:00:00Z"
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Placement options: 1 = Full column width, 2 = Out-set, 3 = Screen-width
# Focal point options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
image:
  placement: 4
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/CpkOjOcXdUY)'
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

Amazon Mechanical Turk (MTurk) is a crowdsourcing marketplace that can be used to get human annotations via hiring workers to perform human intelligent tasks (HITs). In this tutorial, we are going to cover the basics on how to set up an evaluation task on MTurk from scratch. To this end, we will be focusing on the evaluation of the semantic similarity between sentences as our exemplar task; though all steps described could be easily adapted to any task.

-------------------------------------------------------------------
  
### Overview

This tutorial is split into the following five parts assuming that no prior knowledge about MTurk is required. You can treat the following list like a table of contents; if you'd like to jump to a specific section, just click it.

* [**Getting the basics: Setting up accounts**](#setting-up-accounts)
* [**Getting the basics: Concepts and Terminology**](#concepts-and-terminology)
* [**Quality Control: Create your own qualification type**](#qualification-type)
* [**Creating an MTurk project: Using predefined layouts**](#mturk-project)
* [**Go live: Publishing batches**](#publishing-batches)
 
-------------------------------------------------------------------

### Setting up accounts

To get started you need four accounts:  an AWS account, and an account on the MTurk Requester site (those are needed to use MTurk when you are ready to go live and publish your task), and one account on the Requester Sandbox, and one on the Worker Sandbox (those are needed for testing your task on an isolated environment that looks like the real MTurk website before going live and publish your task on the real website).

**:one: AWS account**

AWS (Amazon Web Services) is a cloud platform offering reliable, scalable, and inexpensive cloud computing services and MTurk is one of those. Your billing information is stored with your AWS account, rather than your MTurk requester account. That being said, MTurk has no direct link to your credit card.   

To [sign up for an AWS account](https://portal.aws.amazon.com/billing/signup#/start) you will need: a) an email account, b) a valid credit card (you will not be charged as there is a generous free tier), and c) a phone number (you will receive an automated phone call to verify your identity).

Once you have created an AWS account, you could create an IAM user to securetely control access to your AWS resources. An IAM user could grant permission to administer and use resources in your AWS account (such as access the MTurk API) without having to share your root credentials. To add an IAM user, click on the ``My Security Credentials`` tab and then ``Add user`` following the ``Users`` tab under the DashBoard appearing on the left of the page.   

**:two: MTurk Requester account**

Next, you will need to [create and register an MTurk Requester account](https://requester.mturk.com/create/projects/new).

{{% alert note %}}
After completing the two above steps you will have to [link your AWS account to your MTurk Requester account](https://requester.mturk.com/developer) via using your AWS Root user credentials. 
{{% /alert %}}

**:three: Requester SandBox account**


We are now going to [create a Requester SandBox account]( https://requestersandbox.mturk.com) in the Amazon Mechanical Turk Sandbox testing environment. This website looks exactly the same as the real MTurk website and we are going to use it to test on our tasks and qualifications before we launch them for real.

{{% alert note %}}
At this point, you will also need to [link your AWS account to your Requester SandBox account](https://requestersandbox.mturk.com/developer) as per Link Your AWS account to your MTurk Requester account.
{{% /alert %}}

**:four: Worker SandBox account**

Finally, to test how our task will be presented to workers we are going to [create a Worker SandBox account](https://workersandbox.mturk.com).

-------------------------------------------------------------------

### Concepts and Terminology

Below, we briefly describe the basic conceptd and terminology you should know to effectively use MTurk.

* **Requester**: a company, organization, or person that creates and submits tasks (HITs) to MTurk for Workers to perform. In our case, we are the Requesters.

* **Human Intelligent Task (HIT)**: a task that a Requester submits to MTurk for Workers to perform. A HIT represents a single, self-contained task, for example, "Describe what emotion is conveyed in the following text." In our case, an HIT is one example that we want to get an annotation for.

* **Worker**: a person who performs the tasks specified by a Requester in a HIT.

* **Assignment**: specifies how many people can submit completed work for your HIT. (Hint: the number of assignments is the same as the number of workers working on a single HIT).

* **Reward**: the money a Requester pays Workers for satisfactory work they do on their HITs.

-------------------------------------------------------------------
### Qualification Type 

Amazon Mechanical Turk gives us the ability to add qualification types in the creation or processing of our HIT for better quality control. Once we attach a qualification type to an HIT, a Worker can only perform the task if they have this qualification. Apart from predefined qualifications, MTurk gives us the flexibility to create our own qualification type to represent a Worker's skill or ability to perform the task at hand. 

For our purposes, we are now going to create a costumized qualification test consisting of multiple choice questionsusing the MTurk API. Once the qualification type has been attached to our HIT we can find it under the  ``Qualification Types you have created`` tab on the ``Worker requirements section`` (more on this later). 

In this tutorial we are going to access the MTurk API using [Boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html) the Amazon Web Services SDK for Python. First, we need to install the latest Boto3 release via pip:

```python
pip install boto3
```

Once installation is done, we are ready to create, update, delete or assign a qualification type to a worker or an HIT at Amazon Mechanical Turk!

Import the required libraries:

```python
   import argparse
   import logging
   import boto3
   import os
```

To make our code flexible we pass MTurk parameters as arguments to the main script. **Important note: Even if you prefer to hard code those parameters it is highly recommended to atleast pass the IAM credentials as such!**

```python 
def main():

	"""
	Code for creating/updating/deleting a qualification type at Amazon Mechanical Turk
	Important Note: Do not hard code the key and secret_key arguments
	"""

	parser = argparse.ArgumentParser(description='Create qualification type for English-French bilingual speakers')
	parser.add_argument('--aws_access_key_id', help='aws_access_key_id -- DO NOT HARDCODE IT')
	parser.add_argument('--aws_secret_access_key', help='aws_secret_access_key -- DO NOT HARDCODE IT')
	parser.add_argument('--questions', help='qualification questions (xml file)')
	parser.add_argument('--answers', help='answers to qualification questions (xml file)')
	parser.add_argument('--worker_id', help='worker id, if given we give worker access to the qualification type', \                            	
					default=None)	
	parser.add_argument('--Name',  help='name of qualification test', default='English French qualification test for bilingual speakers.')
	parser.add_argument('--Keywords', help='keywords that help worker find your test', \
					default='test, qualification, english, french, same meaning, same, meaning, bilingual')
	parser.add_argument('--Description', help='description of qualification test', \
                       			default='This is a qualification test for bilingual English-French speakers')
	parser.add_argument('--TestDurationInSeconds', help='time for workers to complete the test', default=5400)
	parser.add_argument('--RetryDelayInSeconds',help='time workers should wait until they retake the test', default=1)
	parser.add_argument('--update', help='if true it updates an existing qualification type', action='store_true')
	parser.add_argument('--verbose', help='increase output verbosity', default=True)
	parser.add_argument('--delete', help='delete qualification type', action='store_true')
	args = parser.parse_args()

	if args.verbose:
		logging.basicConfig(format='%(asctime)s : %(levelname)s : %(message)s', level=logging.INFO)
```

The first step in the creation of our qualification test is to read our question and answer files which should be in xml format. The answer file contains the gold standard answers to the questions provided under the question file and will be later used to automatically assign scores for Workers taking the test. Below, we include exemplar [QuestionForm](#exemplar-questionform-file) and [AnswerKey](#exemplar-answerkey-file) files in xml format.
```python
	questions = open(args.questions, mode='r').read()
	answers = open(args.answers, mode='r').read()
``` 
Following, we create a low-level service [client](https://boto3.amazonaws.com/v1/documentation/api/latest/_modules/boto3.html#client) using boto3.

```python
	mturk = boto3.client('mturk',
                         	aws_access_key_id=args.aws_access_key_id,
                         	aws_secret_access_key=args.aws_secret_access_key,
                         	region_name='us-east-1',
                         	endpoint_url='https://mturk-requester-sandbox.us-east-1.amazonaws.com')
```


{{% alert note %}}
The ``endpoint_url`` argument should be set to ``'https://mturk-requester-sandbox.us-east-1.amazonaws.com'`` during testing time at sandbox. When you are ready to go live at MTurk, replace it with the url: ``'https://mturk-requester.us-east-1.amazonaws.com'``.
{{% /alert %}}

Let's now create our first qualification type! Note that each qualification type is associated with a unique ID that could be used to update, delete or assign a qualification type to a worker through boto3. That being said, it is important to save this ID so that we could refer to our qualification type in the future. 

```python
	if not args.update:

		# Create qualification type based on questions-answers provided and save the qualification id
		try:
			qual_response = mturk.create_qualification_type(
                        				Name=args.Name,
                         				Keywords=args.Keywords,
                         				Description=args.Description,
                         				QualificationTypeStatus='Active',
                               				Test=questions,
                               				AnswerKey=answers,
                               				RetryDelayInSeconds=args.RetryDelayInSeconds,
                               				TestDurationInSeconds=args.TestDurationInSeconds)

			qualification_type_id  = qual_response['QualificationType']['QualificationTypeId']

			logging.info(' Congrats! You have created a new qualification type')
			logging.info(' You can refer to it using the following id: %s' % (qualification_type_id))
			logging.warning(' The qualification_type_id is saved under: qualification_type_id file.')
			logging.warning(' This is the id you will use to refer to your qualification test when creating your HIT!')

			q_id = open('qualification_type_id','w')
			q_id.write(qualification_type_id)

		# If the qualification type has already been created try read the if from file
 		except:
			logging.warning(' You have already created your qualification type. Read from qualification_type_id file...')
           		try:
               			q_id = open('qualification_type_id','r')
               			qualification_type_id =  q_id.readline()
           		except:
               			logging.error(' You have probably deleted the qualification type id file')
```

If we want to update an already created qualification type we can simply access it through the its unique ID.

```python
	# Update an already created qualification type
	else:
		logging.warning(' You have already created your qualification type. Read from qualification_type_id file...')
		try:
			q_id = open('qualification_type_id', 'r')
			qualification_type_id = q_id.readline()
           		mturk.update_qualification_type(
                   			QualificationTypeId=qualification_type_id,
                   			Description=args.Description,
                   			Test=questions,
                   			AnswerKey=answers,
                  			RetryDelayInSeconds=args.RetryDelayInSeconds,
                   			TestDurationInSeconds=args.TestDurationInSeconds)

		except:
           		logging.error(' You have probably deleted the qualification type id file')
```

Now that we have learned how to create and update a qualification type we are going to assign it to a worker. To do so we should be provided with the ID of the worker. Note that this is important at test time as you may wish to link your type to your worker account and take the test. When you are ready to shift from SandBox to the real platform you can just link the qualification type to your HIT or to a specific worker easily through the MTurk website.

```python
	# If worker id is provided try to link to it
	if args.worker_id:
		mturk.associate_qualification_with_worker(
           				QualificationTypeId=qualification_type_id,
           				WorkerId=args.worker_id,
           				IntegerValue=0,
           				SendNotification=True)

		response = mturk.list_workers_with_qualification_type(
           				QualificationTypeId=qualification_type_id)

 		logging.info(' You have associated your qualification type to the worker with id: %s ' % str(response))
	else:
		logging.info(' You may want to associate your qualification type to a worker or attach it to an HIT!')
```

Finally, you could delete the qualification type via again using its ID. 

```python
        # Delete the qualification type 
	if args.delete:
		try:
			q_id = open('qualification_type_id', 'r')
			qualification_type_id = q_id.readline()
			mturk.delete_qualification_type(QualificationTypeId=qualification_type_id)
			os.remove('qualification_type_id')
			logging.warning(' You have already created your qualification type. Read from qualification_type_id file...')
		except:
			logging.error(' You have probably deleted the qualification type id file')
```

Done! We have now created our own qualification test. Note that this is just one way to ensure high quality annotations through MTurk; there are also plenty of other [tips on how to use crowdsourcing through quality control](https://software.intel.com/en-us/articles/hands-on-ai-part-7-augment-ai-with-human-intelligence-using-amazon-mechanical-turk).

### Exemplar QuestionForm file

```xml
<QuestionForm xmlns='http://mechanicalturk.amazonaws.com/AWSMechanicalTurkDataSchemas/2005-10-01/QuestionForm.xsd'>
    <Question>
      <QuestionIdentifier>en_fr_qual_test_0</QuestionIdentifier>
      <DisplayName>Q0</DisplayName>
      <IsRequired>true</IsRequired>
      <QuestionContent>
        <Text> Which statement best describes the relationship between the English and the French sentence? </Text>
          <Text> English and French texts: </Text>
          <EmbeddedBinary>
          <EmbeddedMimeType>
            <Type>image</Type>
            <SubType>png</SubType>
          </EmbeddedMimeType>
          <DataURL>https://path_to_bucket.s3.amazonaws.com/0.png</DataURL>
          <AltText>english-french sentence-pair</AltText>
          <Width>700</Width>
          <Height>200</Height>
        </EmbeddedBinary>
      </QuestionContent>
      <AnswerSpecification>
        <SelectionAnswer>
          <StyleSuggestion>radiobutton</StyleSuggestion>
          <Selections>
            <Selection>
              <SelectionIdentifier>1</SelectionIdentifier>
              <Text> are completely unrelated.</Text>
            </Selection>
            <Selection>
              <SelectionIdentifier>2</SelectionIdentifier>
              <Text> have a few words in common but convey unrelated information about them.</Text>
            </Selection>
            <Selection>
              <SelectionIdentifier>3</SelectionIdentifier>
              <Text> convey mostly the same information, but some information is added and/or missing on either or both sides.</Text>
            </Selection>
              <Selection>
              <SelectionIdentifier>4</SelectionIdentifier>
              <Text> have the exact same meaning.</Text>
            </Selection>
          </Selections>
        </SelectionAnswer>
      </AnswerSpecification>
  </Question>
</QuestionForm>
```
### Exemplar AnswerKey file

```xml
<AnswerKey xmlns="http://mechanicalturk.amazonaws.com/AWSMechanicalTurkDataSchemas/2005-10-01/AnswerKey.xsd">
  <Question>
    <QuestionIdentifier>en_fr_qual_test_0</QuestionIdentifier>
    <AnswerOption>
      <SelectionIdentifier>1</SelectionIdentifier>
      <AnswerScore>-1</AnswerScore>
    </AnswerOption>
    <AnswerOption>
      <SelectionIdentifier>2</SelectionIdentifier>
      <AnswerScore>0</AnswerScore>
    </AnswerOption>
    <AnswerOption>
      <SelectionIdentifier>3</SelectionIdentifier>
      <AnswerScore>1</AnswerScore>
    </AnswerOption>
    <AnswerOption>
      <SelectionIdentifier>4</SelectionIdentifier>
      <AnswerScore>0</AnswerScore>
    </AnswerOption>
  </Question>
</AnswerKey>
```
-------------------------------------------------------------------
### MTurk Project

Now that we have created our qualification test we are ready to create our MTurk project using one of the customizable templates. First, log in to the MTurk Sandbox and click on the ``New Project`` link in the ``Create`` tab. Choose the most suitable template for your task and then click on ``Create Project``. For our tutorial, we will choose the _Emotion Detection_ template, and customize the ``Desing Layout`` section as shown below:

```HTML
<!-- You must include this JavaScript file -->
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

<!-- For the full list of available Crowd HTML Elements and their input/output documentation,
      please refer to https://docs.aws.amazon.com/sagemaker/latest/dg/sms-ui-template-reference.html -->

<!-- You must include crowd-form so that your task submits answers to MTurk -->
<crowd-form answer-format="flatten-objects">

    <!-- Your image file URLs will be substituted for the "image_url" variable below
          when you publish a batch with a CSV input file containing multiple image file URLs.
          To preview the element with an example image, try setting the src attribute to
          "https://s3.amazonaws.com/cv-demo-images/basketball-outdoor.jpg" -->
     <crowd-classifier
      header="Choose the option that best describes the relation between the English and French sentences."
      name="divergent"
      categories="['completely unrelated',
                   'a few words in common but convey unrelated information about them',
                   'mostly the same meaning, except for some details',
                   'exact same meaning']"
    >

    <classification-target>
    <p><img src="${image_url}" style="max-width: 100%; max-height: 250px" /></p>
    </classification-target>

    <full-instructions header="Guidelines for comparing English and French text">
        You are asked to rate how <strong>close the meaning</strong> of the French and English text are, on a scale from 1 to 4.

        <p>
        <strong>1:</strong> English and French texts are <strong>completely unrelated</strong> <br><br>

         <i><u> Example </i></u> <br>
         <font color="blue"> <i> Egnlish: The girl remained missing as of April 2016. </i></font> <br>
         <font color="deeppink"> <i> French: L'Union athlétique libournaise a disparu en 2016.</i></font> <br>
        </p>

      </full-instructions>
      
    <short-instructions>
    You are asked to rate how close the meaning of the French and English text are, on a scale from 1 to 4.
    </short-instructions>
</crowd-form>
```

Once you are satisfied with the result you can preview the task and finish. Note that ``${image_url}`` is a template variable that will be substituted with the actual name of the image from your CSV file when you publish a tasks using this template as described in the next section.

-------------------------------------------------------------------

### Publishing batches

Now the project is set up we can go ahead and publish batches of tasks. This is simply done by clicking on the ``Publish Batch`` button on the new project and uploading a CSV file containing your HIT data. Note that you should add the name of the template variabel (e.g. imaga_url) as a header to your CSV file. Once the CSV file is uploaded, MTurk will create an individual HIT for each row in your file.
 
Enjoy! :coffee:

-------------------------------------------------------------------

### References

* [Pre-screen MTurk workers with custom qualifications (Katherine Wood blog post)](https://katherinemwood.github.io/post/qualifications/)
* [Getting started with the Mechanical Turk API (Katherine Wood blog post)](https://katherinemwood.github.io/post/mturk_dev_intro/)
* [Setting Up Accounts and Tools (AWS Documentation)](https://docs.aws.amazon.com/AWSMechTurk/latest/AWSMechanicalTurkGettingStartedGuide/SetUp.html)
* [Tutorial: How to label thousands of images using the crowd (MTurk blog post)](https://blog.mturk.com/tutorial-how-to-label-thousands-of-images-using-the-crowd-bea164ccbefc)
* [Tutorial: Understanding HITs and Assignments (MTurk blog post)](https://blog.mturk.com/tutorial-understanding-hits-and-assignments-d2be35102fbd)
* [Qualifications and Worker Task Quality (MTurk blog post)](https://blog.mturk.com/qualifications-and-worker-task-quality-best-practices-886f1f4e03fc)
* [Tutorial: A beginner’s guide to crowdsourcing ML training data with Python and MTurk (MTurk blog post)](https://blog.mturk.com/tutorial-a-beginners-guide-to-crowdsourcing-ml-training-data-with-python-and-mturk-d8df4bdf2977)
* [Introducing Amazon Mechanical Turk API support for IAM credentials (MTurk blog post)](https://blog.mturk.com/introducing-mechanical-turk-api-support-for-iam-credentials-8f2de8cd6afb)
* [Boto3 Documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)
* [AWS Identity and Access Management: What is IAM? (AWS Documentation)](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)
* [Requister UI Guide: Publishing a Batch (AWS Documentation)](https://docs.aws.amazon.com/AWSMechTurk/latest/RequesterUI/PublishingYourBatchofHITs.html)
* [Using CSV Files to Create Multiple HITs in the Requester UI (MTurk blog post)](https://blog.mturk.com/using-csv-files-to-create-multiple-hits-in-the-requester-ui-22a25ec563dc)
* [Hands-On AI Part 7: Augment AI with Human Intelligence Using Amazon Mechanical Turk](https://software.intel.com/en-us/articles/hands-on-ai-part-7-augment-ai-with-human-intelligence-using-amazon-mechanical-turk)
