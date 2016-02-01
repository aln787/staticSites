##Static site generation

###_(W/ Frozen-Flask and AWS S3)_
<p>
	</br>
	  <small>Alex Niderberg</a> | <a href="https://twitter.com/alexniderberg">@AlexNiderberg</a></small><br/>
	<small>Technical Product Owner / Master Software Engineer <br/><br/>@ Capital One Labs</small>
</p>


<img src="images/CapitalOneLabs_Overview.png" height="600">


<img src="images/CapitalOneLabs_Work.png" height="600">

---

#Frozen Flask


<img src="images/FrozenFlask.png" alt="idea" height="400">

- [Official Site](http://pythonhosted.org/Frozen-Flask/)
- [Tutorial I followed](https://nicolas.perriault.net/code/2012/dead-easy-yet-powerful-static-website-generator-with-flask/)
  - <small>**I need to submit a PR with a few fixes</small>
<!-- - [Place to submit PR with updates](https://github.com/n1k0/nicolas.perriault.net/) -->


<iframe id="ytplayer" type="text/html" width="640" height="390"
  src="http://www.youtube.com/embed/L0MK7qz13bU?autoplay=0&start=65"
  frameborder="0"/>


###How I got started
```shell
mkdir sample_project && cd $_
virtualenv venv
source venv/bin/activate
pip install Flask Frozen-Flask Flask-FlatPages
touch sitebuilder.py
# add content
python sitebuilder.py
# visit localhost:8000
mkdir pages
touch pages/hello-world.md
# add page content
mkdir templates
touch templates/base.html
# add content
touch templates/page.html
# add content
touch templates/index.html
# add content
```
- [Additional Details](https://nicolas.perriault.net/code/2012/dead-easy-yet-powerful-static-website-generator-with-flask/)
  - <small>**I need to submit a PR with a few fixes</small>
<!-- - [Place to submit PR with updates](https://github.com/n1k0/nicolas.perriault.net/) -->


<iframe src="http://giphy.com/embed/FdvUazOcLjwzK?hideSocial=true" width="680" height="567" frameborder="0" class="giphy-embed" allowfullscreen=""></iframe>
<!-- Polar Bear GIFS
RollAndSlide-XGHCQGcfyl6lW
Help-H2VD6psStWlJ6
GetTheBabby-vBVCam8nE7uxy
CuddleFest-2ur8NS5TYQmK4
Pouncing-2c2DyqhbixfUY
ConfusionCubsRunning-FdvUazOcLjwzK -->
<!-- Penguin GIFS 
Slap-wHSMEx2TtEo8
Flying?-tbAY4hlx9fzjy
Lets-GetOutOfHere-9hbECTMVSdG0
getOutOfTheHouse-B1JDGg2BgvfVe
JumpingPenguins-otnqsqqzmsw7K
penguinSlide_hereWeGo-9UCStxAde7lK -->


###Hands-on
- *These steps will prepare you for the AWS S3 deployment steps in the upcoming sections.

```
git clone https://github.com/aln787/frozenFlask.git && cd $_
virtualenv venv
#(todo)Add python version number to the command above, to ensure comparability
source venv/bin/activate
pip install
subl .
```

- Let's take a quick look at whole the project.


####The brains of this operation
```shell
subl sitebuilder.py
#or more sitebuilder.py
```

```python
import sys

from flask import Flask, render_template
from flask_flatpages import FlatPages
from flask_frozen import Freezer

DEBUG = True
FLATPAGES_AUTO_RELOAD = DEBUG
FLATPAGES_EXTENSION = '.md'

app = Flask(__name__)
app.config.from_object(__name__)
pages = FlatPages(app)
freezer = Freezer(app)

@app.route("/")
def index():
    return render_template('index.html', pages=pages)

@app.route('/<path:path>/')
def page(path):
    page = pages.get_or_404(path)
    return render_template('page.html', page=page)

if __name__ == "__main__":
    if len(sys.argv) > 1 and sys.argv[1] == "build":
        freezer.freeze()
    else:
        app.run(port=8002)
```


###View the basic starter versions of the site:
<br>
####Dynamic
```
python sitebuilder.py
#(todo) determine why this is not locating flask on the plane
# visit localhost:8002
```
<br>
####Static
```
open build/index.html
#if you have http-server installed
cd build && http-server -p 8083 && cd ..
```


####Simple changes to personalize the site base page
```
vi templates/base.html
```

```html
<!doctype html>
<html>
<head>
    <meta charset="utf-8">
    <title><!-- **Add your html site title here --></title>
</head>
<body>
    <h1><a href="{{ url_for("index") }}"><!-- **Add your html site title here --></a></h1>
{% block content %}
    <p>Default content to be displayed</p>
{% endblock content %}
</body>
</html>
```
- Also view review the other templates


###View the post
```
subl pages/frozen-flask.md
subl pages/hello-world.md
```

```markdown
title: Frozen Flask
date: 2016-01-27
tags: [programming, static-sites]

# Experimentation Using Frozen Flask
Site creation following the [this blog post](https://nicolas.perriault.net/code/2012/dead-easy-yet-powerful-static-website-generator-with-flask/).
```


###Update a post
```shell
subl pages/frozen-flask.md
```

```markdown
title: Frozen Flask
date: 2016-01-27
tags: [programming, static-sites]

# <replace with your content>
- <and your content details>
```


###View the updates version of the site:
<br>
####Dynamic
```
python sitebuilder.py
#(todo) determine why this is not locating flask on the plane
# visit localhost:8002
```
<br>
####Static
```
python sitebuilder.py build
open build/index.html
#if you have http-server installed
cd build && http-server -p 8083 && cd ..
```


###Let's get out of here!!
<iframe src="http://giphy.com/embed/9hbECTMVSdG0?hideSocial=true" width="680" height="567" frameborder="0" class="giphy-embed" allowfullscreen=""></iframe>
<!-- Lets-GetOutOfHere-9hbECTMVSdG0 -->

- Time time to put your static site up on S3.

---

#Why AWS / Register


###Gartner's Magic Quadrant - IaaS Providers
<img src="images/s3/AWS_Gartner_2015_MQ.png" height="500">


<iframe src="http://giphy.com/embed/vBVCam8nE7uxy?hideSocial=true" width="680" height="567" frameborder="0" class="giphy-embed" allowfullscreen=""></iframe>
<!-- GetTheBabby-vBVCam8nE7uxy -->


<img src="images/AWS_HomePage.png" height="500">

- [Register](https://portal.aws.amazon.com/gp/aws/developer/registration/index.html)

---

#AWS S3 Deploy using the AWS console


<img src="images/s3/AWS_S3.png" height="540">


<img src="images/s3/S3-CreateFolders_and_AddFiles.png" height="540">


####Files and Folders to add to S3
```
pwd
# <your path>/frozenFlask
cd build/
ls -lah
#drwxr-xr-x   3 <user>  <group>   102B Jan 27 01:23 frozen-flask
#drwxr-xr-x   3 <user>  <group>   102B Jan 27 01:23 hello-world
#-rw-r--r--   1 <user>  <group>   428B Jan 27 01:23 index.html
```


<img src="images/s3/S3-Enable-Static-Website-Hosting.png" height="540">


<img src="images/s3/S3-403.png" height="540">


<iframe src="http://giphy.com/embed/H2VD6psStWlJ6?hideSocial=true" width="680" height="567" frameborder="0" class="giphy-embed" allowfullscreen=""></iframe>
<!-- Help-H2VD6psStWlJ6 -->


<img src="images/s3/S3-Add-Bucket-Policy.png" height="540">


###Bucket policy to add
```
{
	"Version": "2008-10-17",
	"Statement": [
		{
			"Sid": "AllowPublicRead",
			"Effect": "Allow",
			"Principal": {
				"AWS": "*"
			},
			"Action": "s3:GetObject",
			"Resource": "arn:aws:s3:::frozen-flask-bucket2/*"
		}
	]
}
```


<img src="images/s3/S3-Done-OpenSite.png" height="440">

- [Example Simple Frozen-Flask Site](http://frozen-flask-bucket2.s3-website-us-east-1.amazonaws.com/)
- [S3 Bucket(*Permission required to view)](https://console.aws.amazon.com/s3/home?region=us-west-2#&bucket=frozen-flask-bucket2&prefix=)


<iframe src="http://giphy.com/embed/wHSMEx2TtEo8?hideSocial=true" width="680" height="567" frameborder="0" class="giphy-embed" allowfullscreen=""></iframe>
<!-- Slap-wHSMEx2TtEo8 -->
Note: That really sucks compared to just checking in your GH-pages branch.

---

#ASW CLI S3 Deploy


####There has to be a faster way!!
<iframe src="http://giphy.com/embed/9UCStxAde7lK?hideSocial=true" width="680" height="567" frameborder="0" class="giphy-embed" allowfullscreen=""></iframe>
<!-- penguinSlide_hereWeGo-9UCStxAde7lK -->


###AWS CLI Install
```shell
python --version
#Looking for above 2.6.5+ or 3.3+
curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"
unzip awscli-bundle.zip
./awscli-bundle/install -b ~/bin/aws
echo $PATH | grep ~/bin #See if $PATH contains ~/bin (output will be empty if it doesn't)
export PATH=~/bin:$PATH #Add ~/bin to $PATH if necessary
#Confirm the install worked correctly
aws help
```
- [Additional details / official AWS CLI install instructions](http://docs.aws.amazon.com/cli/latest/userguide/installing.html#install-bundle-other-os)


#Add screen shots


####Configure the AWS CLI
```shell
aws configure
#AWS Access Key ID [None]: <Your Key>
#AWS Secret Access Key [None]: <Your Secret>
#Default region name [None]: us-east-1
#Default output format [None]: json
```

<br>
####S3 bucket creation, content sync and configuration
```shell
#aws s3 rb s3://frozen-flask-cli --force  #Remove existing folder with this name
aws s3 mb s3://frozen-flask-cli
pwd
#<your path>/frozenFlask
aws s3 sync build/ s3://frozen-flask-cli --exclude '.DS_Store'
aws s3api put-bucket-policy --bucket frozen-flask-cli --policy file://bucketPolicy.json
aws s3api put-bucket-website --bucket frozen-flask-cli --website-configuration file://enableWebHosting.json
#aws s3api put-bucket-website help
```

- *Note: the AWS API requests for "put-bucket-website" fails with an unclear error messages, if your bucket name contains any capital letters.


####Now we are getting some love! Much better!!
<iframe src="http://giphy.com/embed/2ur8NS5TYQmK4?hideSocial=true" width="680" height="567" frameborder="0" class="giphy-embed" allowfullscreen=""></iframe>
<!-- CuddleFest-2ur8NS5TYQmK4 -->

---

#AWS Automation 
##W/ Python Boto3

---

#Additional Information


###Alternative tools
- Explored
	- Pelican, Jekyll
- Still to Explore
  - [Lektor](https://www.getlektor.com/), [Hugo](https://gohugo.io/), Hexo, Hyde, Brunch, Middleman, Harp, Expose, ...


###Links
- [Blog that inspired investigating frozen flask](http://lucumr.pocoo.org/2015/12/21/introducing-lektor/
)
- [Place to submit PR with updates](https://github.com/n1k0/nicolas.perriault.net/)
