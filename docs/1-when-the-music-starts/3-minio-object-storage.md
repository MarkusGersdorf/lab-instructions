## Object Storage

Object storage provides a flexible and scalable way to store large amounts of unstructured data efficiently, making it a popular choice for cloud storage, content distribution, and for our usecase as well! We will use object storage to store our datasets and model artifacts.

### Walk through the Object Storage

> Minio is one of the most popular object storage out there. It is tailored for cloud-native setups so it is fairly quick to spin up an instance for experimentations. It is also compatible with Amazon S3 API, accessible via a RESTful HTTP API, making integration with cloud-native applications and automation pretty straightforward.


1. For simplicity, a Minio instance is already installed in your dev environment for you. For the curious ones, we leveraged a `Helm chart` and ran below steps to deploy it.

```bash
helm repo add mlops https://rhoai-mlops.github.io/mlops/
helm install minio mlops/minio
```

4. You can access Minio via UI and check that there are already two buckets created for you. Run the below command on the terminal of Jupyter Notebook.

```bash
echo https://$(oc get route minio-ui --template='{{ .spec.host }}' -n <TEAM_NAME>)
```

Click to URL and use `minio` as username, `minio123` as password.

![minio-ui.png](./images/minio-ui.png)

`models` bucket is where we will store our buckets, and `pipelines` bucket is needed to store data science pipeline artifacts.

5. If you go back to OpenShift AI UI, you'll also see that two `Data Connections` created for you. The `Data connections` is the part where we provide our Minio configuration and bucket information. You can see the details by clicking the three dots on the right hand side > `Edit data connection`  Data connections also help us to expose the bucket information into our notebooks so that we can use these information without hardcode them into our code.

![data-connections.png](./images/data-connections.png)

You selected `models` data connection while creating the workbench. Because we will interract with this bucket during our experimentation phase.

One more tool to get familiar with before we dive into our dataset! 💪