For the Knative control plane to function is needs a few required components. There are some optional components to install as well.  Sometimes there are technology choices for some of the layers. All the components are tested with specific versions of Knative. This scenario has been tested with this version of Knative and will be periodically upgraded to keep up with the improvements.

`export KNATIVE_VERSION=0.16.0`{{execute}}

The following installations will assume this version.

## Install CLI tools

`curl -L https://github.com/knative/client/releases/download/v{KNATIVE_VERSION}/kn-linux-amd64 -o /usr/bin/kn | chmod +x -`{{execute}}

https://storage.googleapis.com/knative-nightly/client/latest/kn-linux-amd64

## Installing Knative Serving

Serving is the primary layer that provides an abstraction for stateless request-based scale-to-zero services.

Install the Custom Resource Definitions (CRDs) specific for declaring Knative objects:

`kubectl apply --filename https://github.com/knative/serving/releases/download/v${KNATIVE_VERSION}/serving-crds.yaml`{{execute}}

Notice Knative adds extensions (CRDs) to the Kubernetes API.

`kubectl get crds | grep .knative.`{{execute}}

Install the service-core component.

`kubectl apply -f https://github.com/knative/serving/releases/download/v${KNATIVE_VERSION}/serving-core.yaml`{{execute}}

It will take a few moments to start.

`kubectl get deployments,pods,services --namespace knative-serving`{{execute}}

The serving layer is comprised of ...

## Install networking layer

Knative supports a variety of Kubernetes networking layers such as:

- Ambassador
- Contour
- Gloo
- Istio
- Kong
- Kourier

## Istio

`export ISTIO_VERSION=1.6.7 && curl -L https://istio.io/downloadIstio | sh -`{{execute}}

`export PATH="$PATH:/root/istio-${ISTIO_VERSION}/bin"`{{execute}}


## Kourier
For this scenario, install Kourier and add integration with Knative.

The following commands install Kourier and enable its Knative integration.

Install the Knative Kourier controller:

`kubectl apply --filename https://github.com/knative/net-kourier/releases/download/v${KNATIVE_VERSION}/kourier.yaml`{{execute}}

To configure Knative Serving to use Kourier by default:

```bash
kubectl patch configmap/config-network \
  --namespace knative-serving \
  --type merge \
  --patch '{"data":{"ingress.class":"kourier.ingress.networking.knative.dev"}}'```{{execute}}

It will take a few moments to start.

`kubectl get deployments,pods,services --namespace kourier-system`{{execute}}

Fetch the External IP or CNAME:

`kubectl --namespace kourier-system get service kourier`{{execute}}

Save this for configuring DNS below.
