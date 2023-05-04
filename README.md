# hello-aliyun-devops

Hello Alibaba Cloud DevOps! Just a quick CI/CD test with Alibaba Cloud

## Usage

1. Fork this repository
1. Create a personal instance of [Alibaba Cloud Container Registry \(ACR\)](https://www.alibabacloud.com/product/container-registry) and a namespace if not already
1. Create a repository and select your GitHub fork as the code source
1. In the build options for the repository, add a build rule that triggers a build for every commit to the `main` branch, and tag the resulting image as `latest` \(or otherwise\)
1. Click "Build" under "Actions" to manually trigger the build the first time, ensuring that an image will be available
1. Create an [ACK](https://www.alibabacloud.com/product/kubernetes) cluster. For simplicity, you might want to create a serverless cluster, i.e. ASK
1. Configure `kubectl` locally (or otherwise) to connect to your cluster
1. Use `kubectl` to quickly spin up and expose a deployment using the image `IMG_NAME` you just built:

   ```bash
   kubectl create deploy hello-devops --image=$IMG_NAME --replicas=2 --port=80
   kubectl expose deploy hello-devops --type=LoadBalancer
   ```
1. Fetch the load balancer IP using `kubectl get svc` or via the ACK web console \(or otherwise\) and visit that public IP to verify that the deployment is working. You should see text similar to what is shown below, modulo font size:

   ```
   Hello Alibaba Cloud DevOps!
   Version: 4
   ```
1. In the ACK web console with your cluster selected and under "Workloads > Deployments", choose your `hello-devops` deployment, click on the "Triggers" tab and create a "Redeploy" trigger
1. Copy the generated trigger URL to your repository under section "Trigger" - create a new trigger, paste the URL and name the trigger accordingly
1. Make code changes to your GitHub fork - for example, you might want to bump the version number in `index.html` - and commit your changes. If performing these actions via `git`, remember to `push` your changes as well
1. Watch and observe as an automated build is triggered under your ACR repository, a new image is created, and the trigger is activated which re-deploys the app on your ACK cluster. Wait a while, then re-visit the public load balancer IP associated with your `hello-devops` deployment and you shoud see the changes appear

Congratulations - you've created an automated CI/CD pipeline with Alibaba Cloud!

## Remarks

Since the image used in this demo is based on [Alibaba Cloud Linux](https://www.alibabacloud.com/product/alibaba-cloud-linux-2) 3 which depends on package mirrors and repositories only available within Alibaba Cloud, the image might not build and deploy correctly outside Alibaba Cloud, e.g. on other cloud platforms or locally
