---
tags: [devops, docker, reactjs]
---

[Create-react-app](https://github.com/facebookincubator/create-react-app) is a command line tool to help easily get a simple reactjs app started and going. It abstract away the learning curve and complexity of setting up so we dont have to deal with webpack, transpiling, and setting up a basic skeleton.

In this post, we will dockerize a basic app generated from `create-react-app` using [multi stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) provided by docker to create lean node image based on alpine linux.

### Create-react-app
```
npm install -g create-react-app
create-react-app testapp
cd testapp/
npm start
```

After the project is scaffolded. Take a look at `package.json`.

{% highlight json %}
{
  "name": "testapp",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "react": "^16.0.0",
    "react-dom": "^16.0.0",
    "react-scripts": "1.0.14"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject"
  }
}
{% endhighlight %}

Our `Dockerfile`.
{% highlight bash %}
FROM node:8.5.0-alpine as build
COPY . .
RUN npm install
RUN npm run build

FROM node:8.5.0-alpine as release
COPY --from=build /build ./build
RUN npm install -g serve
EXPOSE 5000
CMD [ "serve", "-s", "build" ]
{% endhighlight %}

In the dockerfile above, first we are building an image based on node of 8.5.0 alpine tag. This image is light weight and has what we needed to build the app. Next, it copy our source into the container and then we start running npm install to download the dependencies. Next, it run npm build, npm build will trigger react-scripts build, react-scripts will output a production optimized build to the 'build' folder.

Noticed that there is a second `FROM` in the dockerfile. This is the multi-stage build feature. What happens here is that the image building process will start fresh on this new image. Then only the resulting `Build` folder from the previous `stage` is copied into this new `stage`. Next, we simply install `serve` which is used to serve the build folder. 

### Conclusion
Docker multi-stage build is an awesome feature to keep production containers lightweight and fast to pull.
