First, I created an HTML file to be stored as a ConfigMap. Create a file named index.html with the following contents:

Create a ConfigMap with the HTML from the file you just created:
$ kubectl create configmap hello-world --from-file index.html

Then deploy the Nginx container deployment, Service, and Traefik Ingress resources with:
$ kubectl apply -f hello-world.yml

After a few seconds, you should be able to access port 80 on any member nodes (assuming networking is working), and get back:
$ curl localhost:80

<html>
<head>
  <title>Hello World!</title>
</head>
<body>Hello World!</body>
</html>
