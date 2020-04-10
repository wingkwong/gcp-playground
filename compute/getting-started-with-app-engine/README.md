# Getting Started With App Engine

## Objectives
- Install the Cloud SDK for App Engine
- Preview an App Engine application running locally in Cloud Shell.
- Deploy an App Engine application, so that others can reach it.
- Disable an App Engine application, when you no longer want it to be visible.

## Prerequisites
- Existing GCP Project

## Install the Cloud SDK for App Engine

Run the following command to install the gcloud component that includes the App Engine extension for Python 3.7:
```
gcloud components install app-engine-python
```

Initialize your App Engine app with your project and choose its region:
```
gcloud app create --project=$DEVSHELL_PROJECT_ID
```

When prompted, select the region where you want your App Engine application located.

![image](https://user-images.githubusercontent.com/35857179/78959163-ad004c00-7b1c-11ea-9dd3-b299118ea6a2.png)

Clone the source code repository for a sample application in the hello_world directory:
```
git clone https://github.com/GoogleCloudPlatform/python-docs-samples
```

![image](https://user-images.githubusercontent.com/35857179/78959206-cb664780-7b1c-11ea-86d2-8d5b532324c5.png)

Navigate to the source directory:
```
cd python-docs-samples/appengine/standard_python37/hello_world
```


## Run Hello World application locally

Execute the following command to download and update the packages list.
```
sudo apt-get update
```

Set up a virtual environment in which you will run your application.

Python virtual environments are used to isolate package installations from the system.
```
sudo apt-get install virtualenv
```

```
virtualenv -p python3 venv
```
If prompted [Y/n], press Y and then Enter.

Activate the virtual environment.
```
source venv/bin/activate
```

Navigate to your project directory and install dependencies.
```
pip install  -r requirements.txt
```

Run the application:
```
python main.py
```

```python
from flask import Flask


# If `entrypoint` is not defined in app.yaml, App Engine will look for an app
# called `app` in `main.py`.
app = Flask(__name__)


@app.route('/')
def hello():
    """Return a friendly HTTP greeting."""
    return 'Hello World!'


if __name__ == '__main__':
    # This is used when running locally only. When deploying to Google App
    # Engine, a webserver process such as Gunicorn will serve the app. This
    # can be configured by adding an `entrypoint` to app.yaml.
    app.run(host='127.0.0.1', port=8080, debug=True)
```

In Cloud Shell, click Web preview (Web Preview) > Preview on port 8080 to preview the application.

To access the Web preview icon, you may need to collapse the Navigation menu.

![image](https://user-images.githubusercontent.com/35857179/78959274-09fc0200-7b1d-11ea-9d43-602fe08c3d96.png)

Result:

![image](https://user-images.githubusercontent.com/35857179/78959314-2435e000-7b1d-11ea-9f58-44db379b4fba.png)

To end the test, return to Cloud Shell and press Ctrl+C to abort the deployed service.

Using the Cloud Console, verify that the app is not deployed. In the Cloud Console, on the Navigation menu (Navigation menu), click App Engine > Dashboard.

Notice that no resources are deployed.

## Deploy and run Hello World on App Engine

To deploy your application to the App Engine Standard environment:

Navigate to the source directory:
```
cd ~/python-docs-samples/appengine/standard_python37/hello_world
```

Deploy your Hello World application.
```
gcloud app deploy
```

This app deploy command uses the app.yaml file to identify project configuration.

```yml
runtime: python37
```

Launch your browser to view the app at http://YOUR_PROJECT_ID.appspot.com
```
gcloud app browse
```

Copy and paste the URL into a new browser window.

Result:

![image](https://user-images.githubusercontent.com/35857179/78959494-b0e09e00-7b1d-11ea-932a-f3035988e62d.png)

## Disable the application

App Engine offers no option to Undeploy an application. After an application is deployed, it remains deployed, although you could instead replace the application with a simple page that says something like "not in service."

However, you can disable the application, which causes it to no longer be accessible to users.

In the Cloud Console, on the Navigation menu (Navigation menu), click App Engine > Settings.

![image](https://user-images.githubusercontent.com/35857179/78959524-c8b82200-7b1d-11ea-8775-d0278e74b1a1.png)

Click Disable application.

![image](https://user-images.githubusercontent.com/35857179/78959550-d8d00180-7b1d-11ea-85f5-82bb6a29e5f7.png)

Read the dialog message. Enter the App ID and click DISABLE.

If you refresh the browser window you used to view to the application site, you'll get a 404 error.

![image](https://user-images.githubusercontent.com/35857179/78959572-ebe2d180-7b1d-11ea-8294-24527e700047.png)