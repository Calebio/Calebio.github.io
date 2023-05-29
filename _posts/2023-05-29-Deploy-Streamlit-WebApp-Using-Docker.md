---
layout: post
title: Deploying a Streamlit Web App to Azure using Docker
subtitle: Step-by-Step Guide to deploy Dockerized Streamlit Web Application
categories: Site
tags: [Docker, Streamlit, Python, Azure]
banner: "/<path/>"
---

This is a step-by-step guide on how to deploy a Streamlit Dashboard using docker and Azure the Prerequisites are:
- [Docker](https://www.docker.com/products/docker-desktop/) and [Docker Account](https://hub.docker.com/login)
- Any IDE of your choice I'm using vscode
- [Azure account](http://portal.azure.com/signin/index/) and [Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)

I assume we have the prerequisites ready so we will crack on with building a very simple streamlit dashboard.

1. We will make the home directory we will be working from. in case I called it `testDir` inside of the `testDir` directory,
we will create another directory called `app` and inside the `app` directory we will create the files we need to get 
the Streamlit dashboard to run locally first before we containerize.

2. Inside the `/app` directory we will create the main file that holds the dashboard, we will call it `streamlit_app.py`
and paste the following code in this file.

```python
from collections import namedtuple
import altair as alt
import math
import pandas as pd
import streamlit as st

"""
# Welcome to Streamlit!

Edit `/streamlit_app.py` to customize this app to your heart's desire :heart:

If you have any questions, checkout our [documentation](https://docs.streamlit.io) and [community
forums](https://discuss.streamlit.io).

In the meantime, below is an example of what you can do with just a few lines of code:
"""

with st.echo(code_location='below'):
   total_points = st.slider("Number of points in spiral", 1, 5000, 2000)
   num_turns = st.slider("Number of turns in spiral", 1, 100, 9)

   Point = namedtuple('Point', 'x y')
   data = []

   points_per_turn = total_points / num_turns

   for curr_point_num in range(total_points):
      curr_turn, i = divmod(curr_point_num, points_per_turn)
      angle = (curr_turn + 1) * 2 * math.pi * i / points_per_turn
      radius = curr_point_num / total_points
      x = radius * math.cos(angle)
      y = radius * math.sin(angle)
      data.append(Point(x, y))

   st.altair_chart(alt.Chart(pd.DataFrame(data), height=500, width=500)
      .mark_circle(color='#0068c9', opacity=0.5)
      .encode(x='x:Q', y='y:Q'))
```

3. We will then create the `requirements.txt` file and past the dependencies, we need to run this web app
it should look like this ![alt text](/assets/images/banners/Requirements.jpg)

4. Now we want to be sure that the streamlit app runs locally so after saving the code, open the terminal in your vscode, 
if you're not already in the `app` directory move to the `/app` directory with `cd app`, and use this code to run your streamlit app
`streamlit run streamlit_app.py`.
You should have an output like this ![first run streamlit output](/assets/images/banners/runStreamlit.jpg)

5. Click on the ip address on the output or copy the ip address and paste it into your browser you should have a result like: ![first browser output](/assets/images/banners/first-dashborad-streamlit.jpg). <br/>
Use `ctl c` to stop the web application.

First, we want to make sure we have docker and azure-cli installed and that we are logged in to the Azure portal.
Check if docker is installed `docker --version` if you have docker installed you should get an output like: ![Docker output](/assets/images/banners/docker-version-check.jpg).
check azure-cli `az --version` if azure-cli is installed you should have an output like ![az-cli output](/assets/images/banners/az-cli-version.jpg)

**NOW LET's CONTAINERIZE**
Now we have confirmed that we have what we need to containerize the web application. 
1. We will add a Dockerfile to help build the Docker image we need. Create a file and name it `Dockerfile` there's no file extension for this file. <br/>
Paste this code into your `Dockerfile``

```
# app/Dockerfile

FROM python:3.9-slim

WORKDIR /app

RUN apt-get update && apt-get install -y \
    build-essential \
    curl \
    software-properties-common \
    git \
    && rm -rf /var/lib/apt/lists/*

COPY . .

RUN pip3 install -r requirements.txt

EXPOSE 8501

HEALTHCHECK CMD curl --fail http://localhost:8501/_stcore/health

ENTRYPOINT ["streamlit", "run", "streamlit_app.py", "--server.port=8501", "--server.address=0.0.0.0"]
```

2. Now we want to login to our Azure from the CLI using the `az login` command.
If after your login and you get a message saying something like `No subscriptions found for your@email.com.`

3. You will need to use the tenant ID to log in so your subscription can be added. something like `az login <TENANT ID>`

Side Note: You can follow these steps to get your tenantID
- Go to your Azure portal and type in `tenantID` and then click on Tenant properties and then you can copy it.

## Create a repository on Azure Container registries to store your docker image

We will do this on the Azure portal

- Search for `Container registries` click on it and click ib create <Image>

- You should have something like this <Image> You can create a new resource group if you don't have one.
You also have to choose a name for your registry and it has to be unique. 

- Once you have chosen a resource group and a name for your registry click on `review + create` and go to create the registry.

- Your registry dashboard should look like this <Image>

- One more thing we want to do is to enable the Admin user and get login credentials to use in pushing a docker to this registry
Click `Access keys` on the left and then enable Admin user, you will get your credentials and then you want to leave this page open
you will need it shortly. should look like this <Image>

**Now go back to the IDE in my case vscode**

- We will try to login to the Azure container registry so we can push a docker image there, using this command `az acr login --name <Registry name>`
in my case `az acr login --name testdeployregistry`

- You will be asked for a username and password. This username and password are the ones we saw in the `Access keys` section, so go on to input the credentials.

- Tag the image using this format `docker tag  <docker_username/image_name login_server/image_name:v1` 
in my case `docker tag  08102414530/streamlit testdeployregistry.azurecr.io/streamlit:v1`

- Push the image to Azure container registry `docker push login_server/image_name:v1` 
in my case `docker push testdeployregistry.azurecr.io/streamlit:v1`.
Your output should look like this <Image>

If you have successfully pushed, we will confirm the pushed image on the Azure portal

- Go back to the Azure portal and Search for `Container registries`
- Click on the registry you created <Image>
- On the left side, scroll down and click on `Repositories` There you can see the image we just pushed <Image>.

**Now we will go back to vscode to run the docker image**

- On the terminal run `docker run --rm -p 8501:8501 login_server/image_name:v1` in my case `docker run --rm -p 8501:8501 testdeployregistry.azurecr.io/streamlit:v1`.
Your output should look like this <Image> click on the i

- Here the final result! <Image> if you made it to this point congratulations!!
