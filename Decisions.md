# Project Decisions

This file records the main decisions and problems faced while setting up the EMS Kubernetes deployment.

## 1. Helm for Deployment

I started with normal Kubernetes YAML files and later added Helm.

The `ems/` chart is now used for deployment. The older `k8s/` files are kept for learning and reference.

We should not apply both sets of manifests to manage the same resources independently.

## 2. Backend Replicas and HPA

The backend was supposed to run with 2 replicas, but the count increased to 5.

I found a Horizontal Pod Autoscaler (HPA) that had been created using an older Kubernetes manifest. It was automatically changing the replica count.

For now, I removed the HPA and kept the replica count at 2 through Helm.

Do not apply the old HPA manifest again unless automatic scaling is needed.

## 3. Helm Upgrade Conflict

Helm failed to upgrade the backend Deployment because of a field ownership conflict involving `spec.replicas` and the `kube-controller-manager`.

If this happens again, check the HPA, Deployment configuration, and field ownership before changing or deleting resources.

## 4. Docker Image Tags

The pipeline currently pushes images using the `latest` tag.

This keeps the pipeline simple, but it makes it harder to identify and roll back to a particular build.

For now, we are keeping `latest`. Unique image tags can be added later.

## 5. Frontend API Requests

Using `localhost:8000` from the browser points to the user's own computer, not the remote backend.

The frontend uses `/api`, with Nginx forwarding API requests to the backend service.

This avoids hardcoding the backend server address in the browser.

## 6. Helm Test Template

The default Helm connection-test template referenced a service port value that was not defined in our chart.

This caused an error during template rendering, so I removed that unused test template.

## 7. Windows and Linux

Jenkins currently runs on Windows, so the pipeline uses Windows `bat` commands.

When moving Jenkins to Linux, I 'll change the required commands to `sh` and configure the Linux environment.

## 8. HPA and Replica Management

The backend initially used an HPA created from an older Kubernetes manifest. It changed the replica count from 2 to as many as 5.

I removed the HPA to keep the deployment simple. Helm now manages the backend replica count, which is set to 2 in `values.yaml`.

If automatic scaling is needed later, we should add the HPA to the Helm chart and remove the fixed replica setting when autoscaling is enabled.

## 9. Helm Rollback

To see the available revisions:

`helm history ems`

To roll back to a particular revision:

`helm rollback ems <revision-number>`
